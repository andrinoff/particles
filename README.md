# Creative WebGL Particle System

![Made with-HTML5](https://img.shields.io/badge/Made%20with-HTML5-orange)
![Made with-CSS3](https://img.shields.io/badge/Made%20with-CSS3-blue)
![Made with-JavaScript](https://img.shields.io/badge/Made%20with-JavaScript-yellow)
![Made with-WebGL](https://img.shields.io/badge/Made%20with-WebGL-lightgrey)

A simple, interactive particle system created with WebGL. This project demonstrates how to render a large number of particles and apply basic physics for a dynamic and engaging visual effect. Move your mouse around to interact with the particles and see how they react!

## Features

- **50,000 Particles**: Renders a large number of particles smoothly.
- **Interactive Mouse Repulsion**: Particles are pushed away from the mouse cursor.
- **Simple Physics**: Includes a constant gravitational force and velocity calculations.
- **Screen Wrapping**: Particles that go off-screen reappear on the opposite side.
- **Dynamic Brightness**: The brightness of each particle is determined by its velocity.
- **Glowing Effect**: A fragment shader creates a soft, glowing look for the particles.

## Getting Started

To run this project locally, simply open the `index.html` file in your web browser. No special build steps are required.

For the best experience, it's recommended to serve the file using a local web server.

**Using Python:**
```bash
# Navigate to the project directory in your terminal
# If you have Python 3.x installed:
python -m http.server

# If you have Python 2.x installed:
python -m SimpleHTTPServer
```

**Using Node.js:**
```bash
# First, install the 'serve' package globally
npm install -g serve

# Then, navigate to the project directory and run:
serve
```
After starting the server, open your web browser and navigate to the local address provided (usually `http://localhost:8000` or `http://localhost:3000`).


## How It Works

The particle system is built entirely with browser technologies, using WebGL for high-performance rendering.

### Vertex Shader (`<script id="vertex-shader">`)

The vertex shader is the core of the particle simulation. It runs once for every single particle in each frame and is responsible for calculating its new position. It does this by:
1.  Reading the particle's initial `position` and `velocity` from a buffer.
2.  Calculating forces:
    * A constant `gravity` pulling particles down.
    * A `mouseForce` that pushes particles away from the cursor, with the force increasing as the particle gets closer.
3.  Updating the particle's `position` based on the elapsed `time` and the calculated forces.
4.  Implementing a `wrapping` logic, so when a particle moves off one edge of the screen, it reappears on the opposite edge.
5.  Passing a calculated `brightness` value (based on the particle's speed) to the fragment shader.

### Fragment Shader (`<script id="fragment-shader">`)

The fragment shader runs for every pixel of every particle point. Its job is to determine the final color of that pixel.
1.  It receives the `brightness` value from the vertex shader.
2.  It calculates the distance of the current pixel from the center of the point (`gl_PointCoord`).
3.  It uses `smoothstep` to create a soft, circular shape with a faded edge, giving the particle a glowing appearance.
4.  The final color is set to a blue tone, multiplied by the `brightness` and the calculated alpha for the soft edge.

### JavaScript (`<script>` block)

The main JavaScript code orchestrates everything:
1.  **Setup**: It gets the canvas element and the WebGL rendering context.
2.  **Shader Compilation**: It reads the shader source code from the `<script>` tags, compiles them, and links them into a WebGL `program`.
3.  **Data Initialization**: It creates a `Float32Array` to hold the initial data for all 50,000 particles (x, y, velocityX, velocityY).
4.  **Buffer Creation**: It sends this particle data to the GPU by creating a WebGL buffer.
5.  **Attributes & Uniforms**: It connects the JavaScript variables to the `attribute` and `uniform` variables inside the shaders. This allows the CPU to send data to the GPU.
6.  **Event Listeners**: It listens for `mousemove` to update the mouse position uniform and `resize` to adjust the canvas and viewport.
7.  **Render Loop**: It uses `requestAnimationFrame` to create a continuous loop that clears the canvas, updates time- and mouse-based uniforms, and draws all the particles on each frame. It also enables `blending` to create a beautiful additive color effect where particles overlap.
