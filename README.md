# Pose Animator: Real-time Pose Detection & Character Animation

## 1. Introduction

The **Pose Animator** is a captivating web-based application that leverages the power of TensorFlow.js to perform real-time human pose detection and animate various character styles based on your movements. Simply enable your webcam, and watch as your poses are transformed into a robot, stick figure, neon outline, or a stylized human character on screen.

This project demonstrates the exciting capabilities of machine learning in the browser, offering a fun and interactive way to experience pose estimation.

## 2. Features

*   **Real-time Pose Detection:** Utilizes `posenet` for accurate and low-latency pose estimation from live webcam feed.
*   **Real-time Face Mesh Detection:** Integrates `facemesh` to overlay a detailed 3D face mesh.
*   **Multiple Character Styles:** Choose from "Robot", "Human", "Stick Figure", and "Neon" animations.
*   **Configurable Smoothing:** Adjust the animation smoothing factor to achieve a more fluid or responsive movement.
*   **Interactive UI:** Modern and responsive design with clear controls and real-time status updates.
*   **Performance Metrics:** Displays real-time FPS, keypoint count, and detection confidence.
*   **Camera Controls:** Easily start and stop the webcam feed.

## 3. Demo

Experience the Pose Animator live!
**[Live Demo on GitHub Pages](https://10xrashed.github.io/pose-animator/)**

*(Note: The actual output will display your live webcam feed and the animated character side-by-side.)*

## 4. Technologies Used

*   **HTML5:** Structure of the web page.
*   **CSS3:** Styling and responsive design.
*   **JavaScript (ES6+):** Core logic and interactivity.
*   **TensorFlow.js:**
    *   **PoseNet:** For real-time human pose estimation.
    *   **FaceMesh:** For real-time 3D face landmark detection.

## 5. Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### 5.1. Prerequisites

You only need a modern web browser (like Chrome, Firefox, Edge, or Safari) that supports `getUserMedia` (for webcam access) and JavaScript.

### 5.2. Installation

Since this is a single HTML file with inline CSS and JavaScript, there's no complex installation process.

1.  **Save the Code:** Copy the entire HTML content provided and save it as an `index.html` file on your computer.

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Pose Animator</title>
        <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.11.0"></script>
        <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/posenet@2.2.2"></script>
        <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/facemesh@0.0.5"></script>
        <style>
            /* ... (CSS code here) ... */
        </style>
    </head>
    <body>
        <!-- ... (HTML body code here) ... -->
        <script>
            /* ... (JavaScript code here) ... */
        </script>
    </body>
    </html>
    ```

### 5.3. Running the Application

1.  **Open `index.html`:** Navigate to the saved `index.html` file and open it with your preferred web browser.
2.  **Grant Camera Access:** Your browser will likely ask for permission to access your webcam. You *must* grant this permission for the application to function.

## 6. Code Structure

The project is contained within a single `index.html` file, which includes all HTML, CSS, and JavaScript.

### 6.1. HTML (`index.html`)

The HTML file sets up the basic page structure, links the TensorFlow.js libraries, defines the styling, and creates the UI elements.

*   **`video` tag:** Displays the live webcam feed. `transform: scaleX(-1)` is used to horizontally flip the video, making it feel like a mirror.
*   **`canvas` tag:** The drawing surface where the animated character and face mesh are rendered.
*   **Control Panel (`.controls`):** Contains buttons, dropdowns, sliders, and statistical displays.
    *   **Status (`#status`):** Shows the current state of model loading and camera activity.
    *   **Camera Buttons (`#startBtn`, `#stopBtn`):** Initiate and terminate webcam stream.
    *   **Character Style (`#characterStyle`):** Dropdown to select animation style.
    *   **Animation Smoothing (`#smoothingSlider`):** Range input to adjust smoothing.
    *   **Face Mesh Toggle:** Buttons to turn the Face Mesh overlay on or off.
    *   **Statistics Grid:** Displays `Keypoints`, `FPS`, `Confidence`, and `Detection` status.

### 6.2. CSS (`<style>` tag)

The CSS is embedded directly in the `<head>` section. It provides a modern, dark-themed, and responsive design for the application.

*   **Variables:** Uses CSS custom properties (`:root`) for easy theme customization (colors, fonts, etc.).
*   **Layout:** Employs Flexbox for arranging the video, canvas, and control panels.
*   **Styling:** Custom styles for buttons, sliders, select boxes, and status indicators, including hover effects and animations.
*   **Responsiveness:** Media queries ensure the layout adapts gracefully to smaller screens.

### 6.3. JavaScript (`<script>` tag)

The JavaScript logic is located at the end of the `<body>` tag.

*   **TensorFlow.js Library Imports:**
    ```html
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.11.0"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/posenet@2.2.2"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/facemesh@0.0.5"></script>
    ```
    These CDN links load the necessary TensorFlow.js core library and the pre-trained PoseNet and FaceMesh models.

*   **Global Variables:** `video`, `canvas`, `ctx`, `poseNet`, `faceMesh`, `isRunning`, `stream`, `smoothedKeypoints`, `smoothingFactor`, `showFaceMesh`, `fps` related variables.

*   **`init()` function:**
    *   Asynchronously loads the PoseNet and FaceMesh models.
    *   Updates the UI status to indicate loading progress and readiness.
    *   Enables the "Start Camera" button upon successful model loading.

*   **`startCamera()` function:**
    *   Requests access to the user's webcam using `navigator.mediaDevices.getUserMedia()`.
    *   Sets the webcam stream as the source for the `video` element.
    *   Starts the `detectPose()` loop once the video metadata is loaded.
    *   Updates UI elements for running status.

*   **`stopCamera()` function:**
    *   Stops all webcam tracks and clears the canvas.
    *   Resets UI elements to a stopped state.

*   **`detectPose()` function (Main Loop):**
    *   This is the core animation loop, called recursively using `requestAnimationFrame`.
    *   Estimates pose from the `video` element using `poseNet.estimateSinglePose()`.
    *   Applies a smoothing algorithm to the detected keypoints to reduce jitter.
    *   Estimates face landmarks using `faceMesh.estimateFaces()`.
    *   Updates the statistics (FPS, keypoints, confidence).
    *   Calls `drawCharacter()` to render the animation.

*   **`drawCharacter()` function:**
    *   Clears the canvas.
    *   Selects the appropriate drawing function (`drawStickFigure`, `drawRobot`, `drawNeon`, `drawHuman`) based on the `characterStyle` dropdown.
    *   Conditionally calls `drawFaceMesh()` if enabled.

*   **`drawStickFigure(keypoints, minConfidence)`:**
    *   Draws a simple stick figure using lines for limbs and a glowing circle for the head/nose.
    *   Uses dynamic shadows and pulsing keypoints for a futuristic effect.

*   **`drawNeon(keypoints, minConfidence)`:**
    *   Draws a stick figure with neon-style glowing lines and keypoints.
    *   The color dynamically shifts through the spectrum (hue rotation).

*   **`roundRect(ctx, x, y, width, height, radius)`:**
    *   Helper function to draw rounded rectangles, used for the "Robot" and "Human" body parts.

*   **`drawRobot(keypoints, minConfidence)`:**
    *   Draws a stylized robot character with segmented body parts (torso, head, limbs).
    *   Features glowing eyes and joints.

*   **`drawHuman(keypoints, minConfidence)`:**
    *   Draws a stylized human-like character with a distinct head, torso, and limbs.
    *   Includes facial features like eyes (with a subtle blink animation) and a mouth.

*   **`drawFaceMesh(face)`:**
    *   Draws the detected 3D face mesh as small dots connected by faint lines.

*   **Event Listeners:**
    *   `startBtn`, `stopBtn`: Handle camera activation/deactivation.
    *   `smoothingSlider`: Updates `smoothingFactor` and its displayed value.
    *   `characterStyle`: Triggers `drawCharacter` to update the rendering style immediately.
    *   `faceToggles`: Controls the `showFaceMesh` boolean.

## 7. Customization

### 7.1. Adding New Character Styles

To add a new character style:

1.  **Add Option to HTML:** In the `select` element with `id="characterStyle"`, add a new `<option>` tag with a unique `value` and display text.
    ```html
    <option value="myNewStyle">✨ My New Style</option>
    ```
2.  **Create Drawing Function:** Write a new JavaScript function (e.g., `drawMyNewStyle(keypoints, minConfidence)`) that takes the detected keypoints and confidence threshold. Use `ctx` to draw your custom character.
3.  **Update `drawCharacter()`:** Add a new `else if` condition in the `drawCharacter()` function to call your new drawing function based on the selected style.
    ```javascript
    if (style === 'stick') {
        drawStickFigure(keypoints, minConfidence);
    } else if (style === 'myNewStyle') {
        drawMyNewStyle(keypoints, minConfidence); // Call your new function
    }
    // ... other styles
    ```

### 7.2. Adjusting Model Parameters

You can fine-tune the PoseNet model loading parameters in the `init()` function:

*   **`architecture`**: ('MobileNetV1' or 'ResNet50') - Affects performance vs. accuracy.
*   **`outputStride`**: (8, 16, 32) - Smaller values give higher resolution but slower performance.
*   **`inputResolution`**: ({ width, height }) - The internal resolution the model processes. Adjusting `CANVAS_WIDTH` and `CANVAS_HEIGHT` will also affect this.
*   **`multiplier`**: (0.50, 0.75, 1.0) - For MobileNetV1, scales the number of channels. Lower values mean faster but less accurate.

```javascript
poseNet = await posenet.load({
    architecture: 'MobileNetV1',
    outputStride: 16,
    inputResolution: { width: CANVAS_WIDTH, height: CANVAS_HEIGHT },
    multiplier: 0.75 // Experiment with this
});
```

### 7.3. UI Enhancements

*   **Colors:** Modify the CSS variables in the `:root` selector to change the entire color scheme.
*   **Fonts:** Change `font-family` in the `body` style.
*   **Layout:** Adjust Flexbox properties in the CSS to re-arrange panels.
*   **New Controls:** Add more HTML input elements (e.g., checkboxes for individual keypoint visibility) and hook them up with JavaScript event listeners to control animation parameters.

## 8. Troubleshooting

*   **"Camera error" / Webcam not starting:**
    *   Ensure you have granted camera permissions to the browser.
    *   Check if your webcam is properly connected and not in use by another application.
    *   Refresh the page.
    *   Verify your browser settings for camera access.
*   **"Error: ... model not loaded"**:
    *   Check your internet connection; the TensorFlow.js models are loaded from CDN.
    *   Ensure the CDN links in the `<script>` tags are correct and accessible.
*   **Low FPS / Laggy animation:**
    *   Your computer might not be powerful enough for real-time processing.
    *   Try reducing the `inputResolution` or `multiplier` for PoseNet in the `init()` function.
    *   Close other demanding applications.
*   **Character jitters excessively:**
    *   Increase the `smoothingSlider` value to apply more animation smoothing.
*   **No character or face mesh appearing:**
    *   Ensure the camera is running and you are visible in the video feed.
    *   Check the browser's developer console for JavaScript errors.

## 9. Future Enhancements

*   **Customizable Keypoint Colors/Sizes:** Allow users to define how individual keypoints are rendered.
*   **Background Removal/Effects:** Integrate background segmentation to place the character in different virtual environments.
*   **Avatar Upload:** Enable users to upload their own 2D or 3D character models.
*   **Recording Feature:** Allow recording the animated output.
*   **More Advanced Models:** Experiment with newer or different TensorFlow.js models for pose/face estimation.
*   **Mobile Support:** Optimize for better performance and UI on mobile devices.
*   **Audio Integration:** Play sound effects based on movements or character interactions.

## 10. License

This project is open-source and available under the [MIT License](LICENSE).

## 11. Acknowledgements

*   **TensorFlow.js team:** For creating and maintaining the incredible TensorFlow.js library, PoseNet, and FaceMesh models.
*   **Google:** For the PoseNet and FaceMesh research.
*   The web development community for countless resources and inspiration.
