# Super Sonic Swing 🕸️

![Gameplay Preview](preview.gif)

**Super Sonic Swing** is a fast-paced physics-based 2D grappling game built with **HTML5 Canvas** and **Matter.js**.

The goal is simple: build enough momentum to **break the sound barrier** and cross the finish line at **supersonic speed**.

---

## Contents

- [How to Play](#how-to-play)
- [Features](#Features-✨)
- [Core Gameplay Flow](#core-gameplay-flow)
- [Game Mechanics & Physics](#Game-Mechanics-Physics)
- [Setup & Installation](#setup--installation)
- [Tech Stack](#tech-stack)
- [License](#license)

---

# How to Play 🎮

### Objective

Reach the finish line at the **far right side of the level**.

### The Catch

You must cross the finish line with speed **greater than or equal to the required speed**.  
If your speed is too low, the game will **bounce you backward** so you must try again.

### Controls

**Mouse / Touch Down**

Attach a grapple hook to the nearest anchor.

**Mouse / Touch Up**

Release the grapple and launch forward.

### Themes

Choose from four visual styles:

- Synthwave
- Cyberpunk
- Minimal Dark
- Minimal Light

---

# Features ✨

### Procedural Level Generation

Levels extend infinitely and become progressively harder.

### Custom Physics System

The game uses a tuned physics system with gravity, air resistance, and momentum.

### Dynamic Visual Effects

- Speed based camera zoom
- Motion trails
- Screen shake
- Mach cone shockwave when breaking the sound barrier

### Environmental Mechanics

The world contains interactive physics elements:

- Bouncy pads
- Anti-gravity zones
- Wind tunnels

---

# Core Gameplay Flow 🔄

```mermaid
graph TD
    Start([Start / Reset Level]) --> Wait[Airborne / Freefall]

    Wait -->|Input: Mouse Down| CheckHook[Find Nearest Anchor Hook]

    CheckHook --> HookValid{Hook Found?}
    HookValid -->|Yes| Yank[Create Grapple Constraint]
    HookValid -->|No| Wait

    Yank --> Swing[Swinging Motion]

    Swing -->|Input: Mouse Up| Fling[Release Grapple]
    Fling --> Wait

    Wait --> PhysicsCheck{Collision Check}

    PhysicsCheck -->|Bouncy Pad| Bounce[Reflect Velocity]
    Bounce --> Wait

    PhysicsCheck -->|Fall| Die[Reset Level]
    Die --> Start

    PhysicsCheck -->|Finish Line| FinishCheck{Speed >= Required Speed}

    FinishCheck -->|Yes| Win[Generate Next Level]
    Win --> Start

    FinishCheck -->|No| Fail[Push Player Back]
    Fail --> Wait
```

---

# Game Mechanics Physics ⚙️

The game builds additional physics mechanics on top of the **Matter.js rigid body engine**.

## 1. Grapple Constraint

When a player clicks, the nearest hook is detected and a **spring constraint** is created between:

Player `P` and Hook `H`.

Physics parameters:

`stiffness = 0.2`  
`damping = 0.05`

This force pulls the player toward the anchor point and produces the swinging motion.

---

## 2. The "Yank" (Attach Boost)

When a grapple successfully attaches, velocity receives a multiplier boost.

`v_new = v_current × λ`

where `λ = 1.1`.

---

## 3. The "Fling" (Release Boost)

First compute normalized velocity:

`v̂ = v_current / |v_current|`

Then apply release boost:

`v_new = v_current + (v̂ × F_fling)`

where `F_fling = 15`.

---

## 4. Supersonic Drag (Air Wall)

If player speed exceeds required speed:

`|v| ≥ V_req`

Velocity update:

`v_new = v_current × μ_drag`

where

`μ_drag = 0.998`

Physics timestep:

`Δt = 16.6 ms`

---

## 5. Dynamic Difficulty Scaling

`V_req(L) = V_base + (L − 1) × 5`

where

`V_base = 45`

---

## 6. Dynamic Camera Zoom

`ratio = min(|v| / V_base , 2.5)`

`Z_target = max(1 − (ratio × 0.3), 0.25) × Scale_base`

Smooth interpolation:

`zoom = lerp(currentZoom , Z_target , 0.02)`

---

# Setup & Installation 🛠️

1. Clone or download the repository  
2. Open `index.html` in a modern browser

Recommended local server:

`python -m http.server`

or **VS Code Live Server**

---

# Tech Stack 🏗️

**HTML / CSS**

UI layout, styling, animations

**JavaScript (ES6)**

Game logic and procedural generation

**Canvas API**

High-performance rendering

**Matter.js**

2D rigid body physics engine

---

# License 📄

© 2026 **Shounak Das**

All Rights Reserved.
