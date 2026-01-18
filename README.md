# Terrain Scanner 3D

An interactive 3D terrain visualization with a scanning cross-section effect and helicopter flyover. Built with Three.js.

## Features

### 3D Terrain Visualization
- **Procedural terrain generation** using multi-octave simplex noise
- 200×200 unit terrain with 192×192 segments for smooth detail
- Real-time vertex manipulation for dynamic height changes
- Height-based color gradients from dark teal to bright cyan

### Scanning Cross-Section
- Animated scanning plane that sweeps through the terrain
- Real-time cross-section line extraction showing terrain profile
- Glowing slice effect with customizable width and color
- Smooth behind-slice fade for depth perception

### Helicopter Model
- Wireframe helicopter with body, cockpit, tail, and rotors
- Animated main rotor (dual-blade) and tail rotor
- Patrol flight pattern with banking and pitch animations
- Follows the scanning slice through the terrain

### Cyberpunk/Synthwave Aesthetic
- Dark background (#0a0a0f) with cyan accents (#00ffc8)
- Grid overlay on terrain surface
- Scanline and vignette effects
- Glowing HUD elements with monospace font
- Bright, high-contrast lighting setup

### HUD Display
- **TOPO-SCAN v2.4** panel showing:
  - Altitude (m)
  - Speed (km/h)
  - Heading (degrees)
- **CROSS-SECTION** panel showing:
  - Slice Z position
  - Sample count
  - Refresh frequency
- GPS coordinates (latitude/longitude)

## Technical Implementation

### Technologies
- **Three.js** (r128) for 3D rendering
- **Custom GLSL shaders** for terrain materials
- **Simplex noise** for procedural generation
- Pure vanilla JavaScript (no framework)

### Key Components

#### Terrain Generation
- Multi-octave noise (4 octaves) for natural-looking landscapes
- Configurable height multiplier (25 units default)
- Real-time height updates each frame
- Auto-computed vertex normals for proper lighting

#### Shader System
- **Vertex shader**: Passes position, normal, and elevation data
- **Fragment shader**:
  - Grid line generation using derivatives
  - Height-based color gradients
  - Slice glow and highlighting
  - Dynamic lighting (ambient + diffuse)

#### Animation Loop
- 60 FPS target frame rate
- Continuous terrain morphing
- Slice plane movement and wrapping
- Helicopter patrol path with realistic motion
- Real-time HUD updates

### Camera Setup
- Fixed perspective camera (55° FOV)
- Positioned at (0, 175, 300) for full terrain view
- Always looks at origin (0, 0, 0)
- Responsive to window resize

### Lighting
- Key light: Bright cyan-tinted directional (#66ffe0)
- Fill light: Darker teal directional (#33ccb3)
- Ambient: Low-level teal (#0b6d5f)
- ACESFilmic tone mapping with 1.35 exposure

## File Structure

```
explorer-webpage/
├── terrain-cross-section-static-camera.html  # Main application
├── README.md                                  # This file
└── CLAUDE.md                                  # Development instructions
```

## Usage

Simply open `terrain-cross-section-static-camera.html` in a modern web browser. No build step or server required.

## Future Enhancements

- Multiple terrain types (desert, ocean, arctic, volcano)
- Terrain type switcher UI with keyboard shortcuts
- Camera controls (orbit, zoom)
- Adjustable scan speed
- Export/screenshot functionality
- Sound effects toggle
- Day/night mode