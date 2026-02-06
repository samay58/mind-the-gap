# Subway Map Skill

Generate interactive, London Tube-style architecture maps for any codebase.

## Trigger

Use this skill when the user says:
- `/subway`
- "generate a subway map"
- "create a codebase map"
- "visualize the architecture"

## Output

A single self-contained `subway-map.html` file in the project root.

---

## Generation Process

### 1. Analyze the Codebase

Use the Explore agent or read files directly to understand:

1. **Directory structure** — identify top-level directories and their purposes
2. **Entry points** — find `main`, `cli`, `app`, `index`, `server` files
3. **Import graph** — trace imports to build a dependency map
4. **Logical domains** — group related modules into "lines"

### 2. Classify Stations

Every module becomes a station with a type:

| Type | Shape | Criteria |
|------|-------|----------|
| `terminal` | Capsule | Entry points, CLI, main files, final outputs |
| `interchange` | Double-circle | Hub modules connecting multiple domains |
| `regular` | Roundel | Standard modules |

### 3. Design the Layout

**Line placement:**
- Each domain gets its own horizontal Y-level
- Space lines 140-180px apart vertically
- Data flows left-to-right (inputs left, outputs right)

**Station placement:**
- Space stations 140-200px apart horizontally
- Entry points at edges, hubs at intersections
- Start coordinates around (100, 120)

**Connections:**
- Primary paths: solid lines following station order
- Cross-domain links: dashed lines with `type: "branch"`

### 4. Write Descriptions

Every station needs TWO descriptions:

**`role`** — One plain-English sentence anyone can understand:
```
Good: "Talks to SEC servers and manages request rate limits"
Bad:  "Async HTTP client with token bucket rate limiter"
```

**`details`** — Technical specifics for engineers:
```
Good: "Async HTTP client using urllib. Token bucket rate limiter (10 req/s).
       Retry with exponential backoff for 429/5xx."
Bad:  "Handles HTTP requests"
```

### 5. Pick Colors

Use this TfL-inspired palette:

```javascript
const COLORS = {
  blue:   "#003688",  // Core functionality
  green:  "#007D32",  // Data processing
  orange: "#E87200",  // Output generation
  purple: "#9B0058",  // CLI/entry points
  cyan:   "#00A0E2",  // External integrations
  red:    "#DC241F",  // Error handling
  brown:  "#B36305",  // Storage/persistence
  yellow: "#FFD300",  // Configuration (low contrast, use sparingly)
};
```

### 6. Generate CONFIG

Build this structure:

```javascript
const CONFIG = {
  project: {
    name: "ProjectName",
    subtitle: "Brief description"
  },

  lines: {
    core: { color: "#003688", label: "Core" },
    data: { color: "#007D32", label: "Data" },
    // ... more lines
  },

  stations: {
    module_id: {
      name: "module.py",
      path: "src/module.py",
      line: "core",
      x: 100, y: 120,
      role: "Plain English description",
      details: "Technical details",
      type: "regular",  // "regular" | "interchange" | "terminal"
      feeds: ["downstream_module"],
      fed_by: ["upstream_module"],
      badge: "entry point"  // optional
    },
    // ... more stations
  },

  paths: [
    // Main line paths (solid)
    { line: "core", stations: ["entry", "processor", "output"] },

    // Cross-line connections (dashed)
    { line: "core", stations: ["processor", "external"], type: "branch" },
  ]
};
```

### 7. Write the HTML

Read `~/.claude/skills/subway/template.html`, then:

1. Find `/* __SUBWAY_CONFIG_START__ */` and `/* __SUBWAY_CONFIG_END__ */`
2. Replace the CONFIG between those markers with your generated CONFIG
3. Update `<title>` with the project name
4. Write to `subway-map.html` in project root

---

## Badge Guide

Optional badges for special roles:

| Badge | Use For |
|-------|---------|
| `"entry point"` | Where execution starts |
| `"hub"` | Central orchestration |
| `"output"` | Generates artifacts |
| `"external"` | External API integration |

---

## Interactive Features

The generated map includes:

- **Path tracing** — click station to highlight upstream/downstream flow
- **Search** — `/` focuses search, filters in real-time
- **Pan/zoom** — drag, scroll wheel, touch gestures
- **Legend filter** — click to show/hide lines
- **Detail panel** — double-click for full info
- **Dark/light** — toggle or system preference
- **SVG export** — download button
- **Keyboard** — Tab cycles, Enter selects, Escape closes

---

## Tips

1. **10-25 stations** is ideal. For larger codebases, focus on key modules.

2. **Left-to-right flow**. Inputs left, outputs right.

3. **Real boundaries**. Lines should represent architectural domains, not just directories.

4. **PM-readable roles**. If a PM can't understand it, rewrite it.

5. **Consistent spacing**. Even gaps between stations look cleaner.
