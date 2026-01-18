# CLAUDE.md - Instructions for Claude Code

## Project Overview

Build an interactive 3D terrain scanner with a cross-section view and helicopter flyover. The scene uses Three.js to render a procedurally generated terrain with animated scanning effects.

## Current Implementation

### Technology Stack
- **Three.js (r128)** for 3D rendering
- **Custom GLSL shaders** for terrain materials and effects
- **Simplex noise** for procedural terrain generation
- Pure vanilla JavaScript

### Key Components

#### 1. Terrain System
- 200×200 unit plane with 192×192 segments
- Procedural height generation using 4-octave simplex noise
- Real-time vertex updates for animated topology
- Custom shader with:
  - Grid overlay
  - Height-based color gradients
  - Scanning slice highlight effect
  - Dynamic lighting

#### 2. Scanning Effect
- Animated slice plane that sweeps through the terrain (Z-axis)
- Real-time cross-section line extraction (512 samples)
- Glowing highlight effect around the slice
- Behind-slice fade for depth

#### 3. Helicopter Model
- Wireframe geometric helicopter
- Fixed position in center of scene
- Animated rotors (main dual-rotor + tail rotor)
- Slight bobbing motion for realism

#### 4. Camera
- Fixed perspective camera at (0, 175, 300)
- Always looks at origin
- Shows full terrain view
- 55° field of view

## Animation System

### Scrolling Topology (CURRENT REQUIREMENT)
The terrain mesh and helicopter should remain **fixed in position**, while the **topology (height pattern) scrolls** beneath them:

**Implementation approach:**
- Add a scrolling offset to the noise sampling coordinates
- In `getTerrainHeight()`, add an accumulating offset to the X or Z coordinate
- This creates the illusion that the terrain surface is flowing while the mesh stays still
- The helicopter remains centered and fixed

```javascript
// Example implementation
let scrollOffset = 0;

function getTerrainHeight(x, z, t = 0) {
  let height = 0;
  const scale = 0.02;

  // Add scrollOffset to make topology flow
  const scrollX = x + scrollOffset;

  height += simplex.noise2D(scrollX * scale, z * scale) * 1.0;
  // ... additional octaves

  return height * terrainHeight;
}

function animate() {
  scrollOffset += scrollSpeed * delta; // Accumulate offset
  updateTerrainMesh(); // Recompute heights with new offset
}
```

### Helicopter Behavior
- **Fixed position**: Center of the scene at (0, 35, 0) approximately
- **No patrol movement**: Remove circular/patrol path code
- **Keep animations**: Rotor spinning, slight vertical bob for realism
- **Optional**: Banking/tilting based on wind or movement feel

## Design Reference

### Cyberpunk/Synthwave Aesthetic
- Dark background (#0a0a0f)
- Primary accent: Cyan (#00ffc8)
- Secondary accents: Magenta (#f72585), Orange (#ff6b35)
- Grid lines on terrain
- Scanline and vignette effects
- Glowing neon HUD elements

### HUD Style
- Monospace font (Share Tech Mono)
- Header font: Orbitron
- Cyan text with glow effects
- Technical readout style
- Minimal, non-intrusive layout

## Future Features to Implement

### Terrain Types
When adding terrain variety, modify the noise function and colors:

1. **Mountains** - Higher amplitude, sharper peaks, coral/orange gradients
2. **Desert** - Gentle rolling dunes, tan/gold colors, lower amplitude
3. **Ocean** - Wave-like patterns, blue gradients, animated variation
4. **Arctic** - Icy formations, white/light blue, particle effects (snow)
5. **Volcano** - Dark rock colors, lava glow effects, smoke particles

### UI Controls
- 5 terrain type buttons along bottom
- Keyboard shortcuts (1-5) for quick switching
- Current terrain indicator
- Smooth crossfade transitions between terrains
- Neon button styling matching theme

### Controls
- Scan speed adjustment slider
- Helicopter altitude control
- Camera zoom/orbit (optional)
- Toggle HUD visibility
- Screenshot/export functionality

## Code Style Guidelines

- Clean, well-commented code
- Descriptive variable names
- Modular functions for each feature
- Constants at top of relevant sections
- Consistent naming conventions
- Use `const` and `let` appropriately

## Performance Considerations

- Limit vertex count for mobile compatibility
- Use efficient noise algorithms
- Minimize shader complexity
- Cache computed values when possible
- Use `requestAnimationFrame` for smooth animation
- Consider reducing quality settings for lower-end devices
