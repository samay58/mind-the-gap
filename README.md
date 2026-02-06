# /subway — Codebase Subway Maps

Generate beautiful, interactive London Tube-style architecture diagrams for any codebase. One command, zero config.

![Subway Map Example](https://github.com/user-attachments/assets/placeholder.png)

## What It Does

Type `/subway` in any project and Claude will:

1. **Analyze your codebase** — scans structure, traces imports, identifies entry points
2. **Design the map** — groups modules into "lines", positions stations, routes connections
3. **Generate a self-contained HTML file** — no dependencies, opens in any browser

The result is an interactive visualization where:
- **Stations** = modules/files in your codebase
- **Lines** = logical groupings (by domain, layer, or directory)
- **Paths** = data flow and dependency connections

## Features

| Feature | Description |
|---------|-------------|
| **Path Tracing** | Click any station to highlight its full upstream/downstream flow |
| **Search** | Press `/` to filter stations in real-time |
| **Dark/Light Mode** | Toggle or follows system preference |
| **Pan & Zoom** | Mouse drag, scroll wheel, touch gestures |
| **Legend Filters** | Click to show/hide entire lines |
| **Detail Panels** | Double-click for full module descriptions |
| **SVG Export** | Download static version for docs |
| **Keyboard Nav** | Tab cycles, Enter selects, Escape closes |

## Installation

### Option 1: Copy to your Claude skills folder

```bash
# Create the skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# Clone or copy the subway folder
cp -r subway ~/.claude/skills/
```

### Option 2: Symlink from a shared location

```bash
# If you keep skills in a central repo
ln -s /path/to/your/skills/subway ~/.claude/skills/subway
```

### Verify installation

The skill should be available immediately. Type `/subway` in any project.

## Usage

### Basic

```
/subway
```

Claude will analyze the current project and generate `subway-map.html` in the project root.

### Open the result

```bash
open subway-map.html  # macOS
xdg-open subway-map.html  # Linux
start subway-map.html  # Windows
```

## Design Philosophy

Inspired by Harry Beck's 1931 London Underground map:

- **Octilinear routing** — lines snap to 0°, 45°, 90° angles
- **Roundel stations** — the iconic circle-with-bar motif
- **TfL color palette** — bold, saturated colors that work in light and dark
- **Cream background** — the classic Tube poster aesthetic

### Station Types

| Type | Shape | Use For |
|------|-------|---------|
| **Terminal** | Capsule | Entry points, CLI, main files |
| **Interchange** | Double-circle | Hub modules connecting domains |
| **Regular** | Single roundel | Standard modules |

### Two-Tier Descriptions

Every station has:
- **Role** — one plain-English sentence for anyone (PMs, designers, new devs)
- **Details** — technical specifics for engineers

## Color Palette

```
Blue    #003688  — Primary/core functionality
Green   #007D32  — Data processing/transformation
Orange  #E87200  — Output/generation
Purple  #9B0058  — CLI/entry points
Cyan    #00A0E2  — External integrations
Red     #DC241F  — Error handling/validation
Brown   #B36305  — Storage/persistence
Yellow  #FFD300  — Configuration (use sparingly)
```

## Examples

### Python CLI (EdgarPack)

5 lines, 18 stations mapping a SEC filing pipeline:
- SEC Data → Parse → Pack → CLI → Site

### TypeScript/React (PDF Voice Tool)

6 lines, 19 stations mapping a streaming PDF-to-audio system:
- Frontend → API → Extract → Enrich → TTS → Stream

## Tips for Good Maps

1. **10-25 stations is ideal** — for larger codebases, map key modules or create domain-specific maps

2. **Left-to-right flow** — inputs on left, outputs on right

3. **Meaningful line groupings** — lines should represent real architectural boundaries

4. **Clear role descriptions** — if a PM can't understand it, rewrite it

## File Structure

```
~/.claude/skills/subway/
├── README.md           # This file
├── SKILL.md            # Instructions Claude follows
├── template.html       # The HTML template
└── examples/
    ├── edgarpack.html      # Python CLI pipeline (18 stations)
    └── pdf-voice-tool.html # TypeScript streaming app (19 stations)
```

## How It Works

1. Claude reads `SKILL.md` for generation instructions
2. Explores your codebase using Glob, Grep, and Read tools
3. Builds a dependency graph and identifies logical groupings
4. Generates a `CONFIG` object with stations, lines, and paths
5. Injects the config into `template.html`
6. Writes the complete HTML to your project

The template is ~45KB of self-contained HTML/CSS/JS with no external dependencies (except Google Fonts for Inter, with system font fallbacks).

## Customization

After generation, you can edit `subway-map.html` directly:

- **Adjust positions** — change `x` and `y` values in stations
- **Rename lines** — update the `lines` object
- **Add/remove stations** — modify the `stations` object
- **Restyle** — CSS custom properties at the top of the file

## License

MIT — use it, modify it, share it.

## Credits

- Design inspired by Transport for London's iconic Underground map
- Built for [Claude Code](https://claude.ai/code)
- Created by [@samaydhawan](https://twitter.com/samaydhawan)
