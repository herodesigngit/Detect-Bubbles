🔍 Why Detect Bubbles?

Bubble analysis is crucial in many industries:

✅ Material Science – Analyzing foam structures & polymer coatings 

✅ Pharmaceuticals – Ensuring proper mixing in drug formulations 

✅ Food & Beverage – Controlling carbonation in beverages 

✅ Chemical Engineering – Monitoring gas-liquid reactions in reactors 

✅ Biomedical Research – Studying cellular bubble formations in fluids

Understanding bubble size, distribution, and shape is key to optimizing these processes.
⚠️ The Challenge: Detecting & Measuring Bubbles

Given a microscope image of bubbles, we needed to extract key insights:

✔ Total bubble count 

✔ Median bubble size (µm²) 

✔ Size distribution (min-max range)

 ✔ Average bubble size

At first, OpenCV’s contour detection worked well… until it didn’t. Here’s what went wrong.
💥 Challenges We Faced
1️⃣ Blurry Bubbles & Low Contrast

🔹 Some microscope images had uneven lighting, making bubble edges difficult to detect. 

🔹 Tiny bubbles merged together, making it hard to separate them.

✔ Solution: 

✅ Applied adaptive thresholding instead of a fixed threshold

✅ Used Gaussian blur to smooth the image while keeping edges
2️⃣ Triangular & Irregular Bubbles

🔹 Not all bubbles were perfect circles—some were triangular or distorted due to physical forces.

 🔹 OpenCV’s Hough Circle Transform failed to detect these non-circular bubbles.

✔ Solution:

 ✅ Instead of HoughCircles, we used contour approximation to detect irregular shapes.

 ✅ Applied circularity filtering:

    Circularity = 4π×AreaPerimeter24\pi \times \frac{\text{Area}}{\text{Perimeter}^2}4π×Perimeter2Area​

    Used it to distinguish real bubbles from noise

  3️⃣ Overlapping Bubbles – When One Becomes Many

🔹 Some bubbles merged together, forming clusters that OpenCV misidentified as a single bubble.

✔ Solution:

 ✅ Used watershed segmentation to separate touching bubbles 

✅ Applied distance transform to break large blobs into individual bubbles

⚙️ The Final OpenCV Pipeline

1️⃣ Preprocessing – Convert to grayscale & apply Gaussian blur 

2️⃣ Segmentation – Use adaptive thresholding & edge detection 

3️⃣ Morphological Operations – Clean up noise with dilation/erosion

4️⃣ Contour Detection – Identify bubbles, including irregular ones 

5️⃣ Watershed Algorithm – Split overlapping bubbles 

6️⃣ Feature Extraction – Compute area, circularity & distribution
