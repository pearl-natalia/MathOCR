<h1>Neural Network - Digit Detection</h1><br><br>
<p align="center"><img width="479" alt="image" src="https://github.com/pearl-natalia/digit-detection/assets/145855287/11c7ccf7-debe-4e6f-b9b9-4a03b3a13208"></p>


<h2>The Model</h2>
<p>Density of input layer: 728 (we will use 28x28 pixel images (each neuron takes 1 pixel for input)</p>
<p>Density of output layer: 10 (for digits 0-9) computed with forward propogation</p>
<p>Layers: 2 ("Layer zero" isn't technically a layer as it takes in no parameters)</p>

<h2>The Math</h2>
<p>Dataset of images gets converted into a 2D array (each instance of the datatset is a row, and within each row is an array of 728 elements (pixels)).</p>
<img width="627" alt="image" src="https://github.com/pearl-natalia/digit-detection/assets/145855287/bb959403-e11c-4682-a908-969bf6a04f6b">

<h2>Greyscale Table</h2>
 <img width="758" alt="Screenshot 2024-05-14 at 10 19 16 PM" src="https://github.com/pearl-natalia/digit-detection/assets/145855287/bfc7c385-b2dc-4753-9e4d-c85ba2dba37d">



<h2>Step 1: Segmenting Image into Terms & Math Operators</h2>
<p>Resize: Scale down image to reduce computational overhead (I used width of 1000px, height based on aspect ratio, INTER_AREA interpolation method to preserve image quality).</p>
<p>Threshold: Convert to greyscale and using open cv's threshold method (pixels above certain value become white, then invert img so text = white, backgrounf = black). This will format image for dilation.</p>
<p>Dilation: Extend white to merge nearby digits together, allowing machine to detect with digits belong together in a term. This gives meaining to the numbers. Pixels are dilated with a kernel (see below); to identify terms, a kernel with a large width is ideal incase digits are spaced out.</p>
<p align="center"><img width="740" alt="image" src="https://github.com/pearl-natalia/OCR-from-scratch/assets/145855287/6dcc8dbc-399e-469d-9352-690ae3364acc"></p>
<p>Contours: Use OpenCV's findContours() function to identify bounding edges of the terms + operators in the equation. This function will operate on the dilated image, using RETR_EXTERNAL to omit any inner contours and CHAIN_APPROX_NONE to store all points of the contour for a more accurate result. OpenCV uses Suzuki's algorithm to calculate contour points.</p>








