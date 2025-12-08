# Forge CAD Preload Plugin

CAD library documentation preload plugin for the [Forge/Fabrikator](https://github.com/unforkableco/fabrikator) platform.

## Overview

This plugin preloads CAD library documentation, tutorials, and code examples directly into the AI agent's context. This enables the agent to write better OpenSCAD code by having immediate access to library references without needing to make search API calls.

## Features

- **Quick Reference**: Compact reference for common BOSL2 patterns and functions
- **Full Tutorials**: Detailed tutorials loaded from bundled assets
- **Code Examples**: Working OpenSCAD examples demonstrating library usage
- **Multi-Library Support**: Documents BOSL2, threads-scad, dotSCAD, Round-Anything, and NopSCADlib
- **Context-Efficient**: Prioritized injection to maximize relevance without overwhelming context

## Included Libraries

| Library | Description |
|---------|-------------|
| **BOSL2** | Belfry OpenSCAD Library v2 - Enhanced primitives, attachments, and more |
| **threads-scad** | Thread generation for bolts, nuts, and threaded holes |
| **dotSCAD** | Functional OpenSCAD with list comprehensions |
| **Round-Anything** | Advanced filleting and rounding operations |
| **NopSCADlib** | Vitamins (hardware) and printed parts library |

## Plugin Type

This is a **config-only** plugin - it requires no external backend service. All content is injected directly into the agent's system prompt from bundled assets.

## Installation

### In Fabrikator

1. Upload the plugin through the Admin interface
2. Enable for users or mark as a core plugin
3. Users enable/disable per project as needed

### Asset Structure

```
├── plugin.yaml           # Plugin configuration
├── assets/
│   ├── tutorials/        # Markdown tutorials
│   │   ├── Attachment-Overview.md
│   │   ├── Attachment-Attach.md
│   │   ├── Paths.md
│   │   └── ... more tutorials
│   └── examples/         # OpenSCAD code examples
│       ├── attachments.scad
│       ├── boolean_geometry.scad
│       └── ... more examples
└── README.md
```

## How It Works

The plugin uses Fabrikator's prompt injection system:

1. **Prepend (Priority 10)**: Quick reference card with essential syntax
2. **Append (Priority 15)**: Full tutorials from `assets/tutorials/*.md`
3. **Append (Priority 20)**: Code examples from `assets/examples/*.scad`
4. **Append (Priority 25)**: Usage reminders and best practices

The `contentFromAssets` feature loads file content dynamically when the agent session starts.

## Plugin Configuration

```yaml
plugin:
  slug: cad-preload
  name: CAD Library Preload
  version: 1.0.0

capabilities:
  prompts:
    agents:
      - agent: cad
        position: prepend
        priority: 10
        content: |
          ## BOSL2 QUICK REFERENCE
          ...

      - agent: cad
        position: append
        priority: 15
        contentFromAssets:
          - path: tutorials/*.md
            wrapper: "### {basename}\n\n{content}"
```

## Included Tutorials

### BOSL2 Attachments
- `Attachment-Overview.md` - Introduction to the attachment system
- `Attachment-Attach.md` - Using attach() for positioning
- `Attachment-Position.md` - Position and orient children
- `Attachment-Tags.md` - Tagging for boolean operations
- `Attachment-Color.md` - Color and visibility control

### BOSL2 Core
- `Shapes2d.md` - 2D shape primitives
- `Shapes3d.md` - 3D shape primitives  
- `Transforms.md` - Transformation helpers
- `Paths.md` - Path operations and sweeps
- `Beziers_for_Beginners.md` - Bezier curves tutorial

### Advanced Topics
- `VNF.md` - Vertex-Normal-Face mesh operations
- `Distributors.md` - Distributing objects in patterns
- `Mutators.md` - Shape modification operations

## Included Examples

| Example | Description |
|---------|-------------|
| `attachments.scad` | Basic attachment patterns |
| `boolean_geometry.scad` | Boolean operations with tags |
| `orientations.scad` | Orientation and rotation examples |
| `spring_handle.scad` | Complex multi-part assembly |
| `fractal_tree.scad` | Recursive geometry |
| `lsystems.scad` | L-system based patterns |

## Adding New Content

### Adding Tutorials

1. Create a Markdown file in `assets/tutorials/`
2. Use clear headers and code blocks
3. The filename becomes the section title

```markdown
# My Tutorial

## Introduction

This tutorial covers...

```scad
// Example code
cuboid([10, 10, 10]);
```
```

### Adding Examples

1. Create a `.scad` file in `assets/examples/`
2. Include comments explaining the code
3. Make it self-contained and runnable

```scad
// example_name.scad
// Demonstrates: Feature X
include <BOSL2/std.scad>

module my_example() {
    // ... code ...
}

my_example();
```

## Development

### Testing Locally

1. Copy the plugin to your Fabrikator `backend/plugins/core/` directory
2. Run the plugin seed script: `npm run plugins:seed`
3. Start a CAD agent session and verify content is injected

### Modifying Priority

Adjust the `priority` values in `plugin.yaml` to control injection order:
- Lower numbers = injected earlier
- Higher numbers = injected later

## License

MIT License - see [LICENSE](LICENSE) for details.

## Related Plugins

- [forge-plugins-searchdocs](https://github.com/unforkableco/forge-plugins-searchdocs) - Search documentation for details
- [forge-plugins-vision](https://github.com/unforkableco/forge-plugins-vision) - Visual validation of generated geometry

## Contributing

Contributions are welcome! To add new tutorials or examples:

1. Fork this repository
2. Add your content to `assets/tutorials/` or `assets/examples/`
3. Test with Fabrikator locally
4. Submit a pull request

