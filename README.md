<h1>Mathematical OCR - Equation Recognition & Calculation</h1>
<h2>Step 1: Segmenting Image into Terms & Math Operators</h2>
<p>Resize: Scale down image to reduce computational overhead (I used width of 1000 px, height based on aspect ratio, INTER_AREA interpolation method to preserve image quality).</p>
<p>Threshold: Convert to greyscale and using OpenCv's threshold method (pixels above certain value become white(255), then invert img so text = white, background = black). This will format image for dilation.</p>
<p align="center"> <img width="200" alt="Screenshot 2024-05-14 at 10 19 16 PM" src="https://github.com/pearl-natalia/digit-detection/assets/145855287/bfc7c385-b2dc-4753-9e4d-c85ba2dba37d"></p>
<p>Dilation: Extend white regions to merge nearby digits together, allowing machine to detect which digits belong together in a term. This gives meaning to the numbers. Pixels are dilated with a kernel (see below); to identify terms, a kernel with a large width is ideal in case digits are spaced out.</p>
<p align="center"><img width="740" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/6dcc8dbc-399e-469d-9352-690ae3364acc"></p>
<p>Contour Points: Use OpenCV's findContours() function to identify bounding edges of the terms + operators in the equation. This function will operate on the dilated image, using RETR_EXTERNAL to omit any inner contours and CHAIN_APPROX_NONE to store all points of the contour for a more accurate result. OpenCV uses Suzuki's algorithm to calculate contour points.</p>
<p align="center"><img width="698" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/25dd7fb1-e90b-477a-964c-b674513d7ee1"></p>

<p>Bounding Rectangle: Use boundingRect() to identify smallest rectangle that encloses the contour points. This is to split image into smaller images of terms and operators.</p>
<p align="center"><img width="679" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/008ccda2-6fa9-4737-a024-45545afb2622"></p>

<p>Repeat: Repeat the process for each sub image to differentiate terms into digits, with a smaller kernel since digits are close to each other, and store values in a 3D array</p>
<p align="center"><img width="771" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/e198f43f-9e44-4257-812a-b7404dc68efd"></p>

<h2>Step 2: Neural Network to Identify Digits & Symbols</h2>
<p>Dataset: I used a dataset of 27,000 28x28 pixel images of digits 0-9, and symbols plus, minus, dot, multiply, slash for division, & variables w, y, z. The dataset is from <a href="https://github.com/wblachowski/bhmsds?tab=readme-ov-file"><i>wblachowski</i></a>, licensed under the MIT License. There will be an 80:20 train:test split.</p>

<p align="center"><img width="200" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/a5144f81-dcc1-4835-bbb5-9cf67e5b0db6"></p>

<p>Format: Convert to grayscale. Now resize the image to have height of 28 pixels, and aspect ratio accordinly (do not stretch image by forcing both dimensions to be 28 pixels!). Then append image onto center of 28x28 black canvas so input image is 28x28 pixels. Increase intensity of pixels (reducing their grayscale value if they meet a certain threshold to darken the handwriting.</p>
<p align="center"><img width="200" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/8cbc6225-14f7-4d6e-aeed-86b5d05ec4e9">
</p>

<p>Model: Consists of 3 layers, inner layers use Relu activation, and output layer uses softmax. Image to String: Iterate through the array of images, use the trained model to predict the digit/symbol it contains, append to a string representing the equation, and use python's built in eval() function to evalute the equation.</p>

<p align="center"><img width="653" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/9c8c8a48-2553-42d9-9511-7c42beee7887"></p>

















