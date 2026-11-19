
# Gemini 3.0测试用提示词模板

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/SCvddUuQzoYGuSxVew4c9Nw6nCc](https://kq4b3vgg5b.feishu.cn/docx/SCvddUuQzoYGuSxVew4c9Nw6nCc)

## MacOS操作系统

```
**Create a polished, interactive macOS-style web op****eratin****g system conta****ined ent****irely within**** a sing****le HTML file.
This**** “WebOS”**** shoul****d emulat****e the a****esthetics and ****interacti****ve beh****aviors of ma****cOS while be****ing fully funct**ional in the browser.
1. Visual Style &amp; Aesthetic
Theme: Clean macOS-inspired UI with rounded windows, translucent frosted-glass panels, soft drop shadows, and smooth animations.
Color Palette: Light or dark mode inspired by macOS (grays, soft whites, subtle gradients).
Effects:
Use CSS backdrop-filter: blur() to recreate translucent windows.
Smooth easing everywhere (cubic-bezier transitions).
Subpixel shadow detailing on dock icons, window focus rings.
Environment:
A dynamic wallpaper background (e.g., animated gradient, parallax stars, or subtle waves).
A bottom dock with hover magnification (“dock zoom” effect).
Assets:
All icons, shapes, and UI elements must be created programmatically using HTML/CSS/SVG.
No external images.
2. Window System &amp; Desktop Interaction
Windows:
Draggable, minimizable, closable, resizable.
Must include the macOS-style red/yellow/green traffic-light buttons.
Window focused state should visually stand out with a glow or border.
Applications to Implement:
Finder
Displays a mock file system (folders, files, preview panel).
TextEdit
Contains a full text editor with Save/Load (localStorage).
Browser
Includes an address bar + iframe to load real websites.
System Monitor
Animated SVG dashboard showing fake CPU, Memory, Network metrics.
Notes (optional)
Sticky notes on the desktop with drag-and-drop.
3. Dock Interaction
Hover magnification effect (icons grow smoothly).
Clicking an app:
Opens the corresponding window.
If already open, bring to front.
Right-click menu:
“Quit”
“Hide”
“Show All Windows”
4. Controls &amp; Input
Keyboard:
⌘ + W → Close window
⌘ + M → Minimize
⌘ + Tab → App switcher
Mouse:
Drag to move windows
Resize using edges or corners
5. Technical Constraints
Single File:
 Entire project must be generated as one &lt;html&gt; file containing:
HTML
CSS
JavaScript
 No external files except CDN libraries (if needed).
Performance:
 Windows and dock must be GPU-smooth at 60FPS.
Responsiveness:
 The WebOS must scale perfectly on:
Desktop
Tablet
Mobile
 without stretching UI elements.
```

## 基于SVG的霓虹效果的时钟

```
**Cre****ate a high****ly-polished,**** fully anima****ted S****VG clock wi****th a retro-f****uturistic Sy****nthwave aesthet**ic, embedded entirely within one HTML file.
1. Visual Style &amp; Atmosphere
Aesthetic: Bold neon Synthwave look using:
Hot pink
Electric cyan
Deep violet
Laser-grid accents
Scene Setup:
A glowing neon grid floor extending into the distance (SVG perspective trick).
A stylized horizon line with glowing mountains.
Animated stars or scanlines to reinforce the retro-digital feel.
Clock Style:
Segmented neon hour/minute/second hands with bloom-style glow.
Tick marks must pulse or breathe.
Neon outlines with layered blurs for depth.
2. Clock Mechanics
Time:
 The clock must reflect the actual local system time.
Animations:
Smooth second hand sweep (not ticking).
Minute/hour hands update continuously.
Hands should glow brighter when moving.
Special Effects:
Add a neon trail behind the second hand.
Add subtle parallax movement to background elements.
3. Interactivity Features
Hovering the clock increases glow intensity.
Clicking the clock toggles between:
Analog mode
“Digital neon mode” showing a retro segmented digital clock.
4. Technical Constraints
Single File:
 Everything must be inside a single &lt;html&gt; file.
 No external images.
 SVG must be inline &lt;svg&gt; with gradients, filters, masks.
Performance:
Use requestAnimationFrame for smooth 60FPS hand movement.
Avoid re-creating DOM nodes.
Use transform rotation for hands (no path recalculation).
Responsiveness:
The clock must scale beautifully and center itself on any screen size.
```

## 3D太空飞船游戏

```
Create a polished, retro-futuristic 3D spaceship web game contained entirely within a single HTML file using Three.js. The game should feature a &quot;Synthwave/Retrowave&quot; aesthetic with the following specifications:

### 1. Visual Style &amp; Atmosphere
*   **Aesthetic:** Dark, immersive 3D environment with a glowing, volumetric neon look. Use a color palette of deep purples, hot pinks, and electric cyans.
*   **Post-Processing:** You must implement Three.js EffectComposer with UnrealBloomPass to make the neon elements glow intensely.
*   **Environment:** 
    *   A dense, moving starfield background with rich features. Go beyond simple plane shapes here and apply shaders to make the game visually appealing. For example, simulating retro astra, stars, and planets in the background.
    *   Distance fog to fade distant objects smoothly into the darkness.
*   **Assets:** Use complex geometric primitives constructed programmatically (no external model imports).

### 2. Gameplay Mechanics
*   **Perspective:** Third-person view from behind the spaceship.
*   **Core Loop:** The player pilots the ship on the X and Y axis (2D plane) while enemies (neon blocks) spawn in the distance and fly toward the camera along the Z-axis.
*   **Combat:** 
    *   The player shoots laser bolts (glowing lines) to destroy enemies.
    *   **Collision:** When a laser hits a block, the block should shatter into a very complex particle explosion effect.
    *   **Game Over:** If a block hits the ship, the game ends.
*   **UI:** A minimal HUD displaying the current Score. A &quot;Game Over&quot; overlay with a &quot;Restart&quot; button.

### 3. Controls (Cross-Platform)
The game must detect the device type or input method:
*   **Desktop/Web:** 
    *   Use **Arrow Keys** or **WASD** for smooth movement (apply lerp or friction for a floaty, drift-like spaceship feel).
    *   Use **Spacebar** to fire.
*   **Mobile/Touch:** 
    *   Render a semi-transparent **Virtual Joystick** on the bottom-left of the screen (using HTML/CSS or Canvas API) to map touch movement to ship coordinates.
    *   Detect a **Tap** anywhere on the right side of the screen to fire lasers.

### 4. Technical Constraints
*   **Single File:** All HTML, CSS, and JavaScript (including the Three.js library and post-processing shaders imported via CDN) must be in one index.html file. Do not output any other text other than the html response.
*   **Performance:** Use object pooling for lasers and particles to ensure 60FPS performance.
*   **Responsiveness:** The canvas must resize dynamically to fit any screen size without stretching the aspect ratio.
```
