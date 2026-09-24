# Gravity-In-2D-Using-openGL
A real-time 2D physics simulation built in C++20 and Modern OpenGL (3.3 Core Profile) modeling Earth's gravity in 2D. The engine simulates the motion of balls under the influence of earth's gravitational force of attraction and accounting various factors like bouncing,collisions,boundary conditions etc.

## Technical Highlights

### 1. Earth Gravity & Kinematics
Objects experience constant downward acceleration representing surface gravity:

$$\vec{a} = \begin{bmatrix} 0 \\ -g \end{bmatrix} \quad \left(g = 9.81 \text{ m/s}^2 \text{ or code-scaled equivalents}\right)$$

Position and velocity are updated per frame over delta time
$$\vec{v}_{t+\Delta t} = \vec{v}_t + \vec{a} \cdot \Delta t$$
$$\vec{p}_{t+\Delta t} = \vec{p}_t + \vec{v}_{t+\Delta t} \cdot \Delta t$$

### 2. Wall Boundary Conditions & Energy Loss
When a body collides with screen boundaries, its velocity component normal to the wall is inverted and multiplied by a coefficient of restitution ($e < 1.0$) to simulate energy absorption:

* **Top/Side Walls:** Inverts velocity along the bounce axis with kinetic dissipation ($v_{\text{new}} = -0.80 \cdot v_{\text{old}}$).
* **Floor Boundary:** Inverts vertical speed while applying horizontal friction ($v_x = 0.98 \cdot v_x$) to simulate rolling resistance over time.

### 3. Pairwise Impulse Collision Response
When two balls overlap ($\text{distance} < r_A + r_B$), the engine handles interaction in two phases:
1. **Positional Correction:** Separates interpenetrating geometry proportionally based on inverse masses to eliminate sinking/sticking bugs:
   $$\Delta \vec{p}_A = \frac{m_B}{m_A + m_B} (\text{overlap}) \cdot \hat{n}$$
2. **Impulse Exchange:** Calculates elastic momentum transfer along the collision normal $\hat{n}$:
   $$j = \frac{-(1 + e) (\vec{v}_{\text{rel}} \cdot \hat{n})}{\frac{1}{m_A} + \frac{1}{m_B}}$$

### 4. Modern OpenGL Pipeline
* Procedurally generates triangle-fan circle meshes for rendering.
* Updates entity translation uniforms (`u_Offset`) alongside dynamic Vertex Buffer Objects (`GL_DYNAMIC_DRAW`) and Vertex Array Objects (VAO).
* Utilizes frame delta-time ($\Delta t$) for framerate independence across variable monitor refresh rates.

---

## Tech Stack

* **Language:** C++20
* **Graphics API:** OpenGL 3.3 (Core Profile)
* **Libraries:** GLFW 3.4, GLEW 2.3.1, GLM (OpenGL Mathematics)


---

## Project Structure

```text
├── main.cpp                # Core physics engine, update loops, and OpenGL setup
├── dependencies/
│   ├── include/            # Headers for GLFW, GLEW, and GLM
│   └── library/            # Pre-compiled static and dynamic libraries
├── Makefile                # Build targets and compiler flags
└── README.md



