# CAD 3D Modeling Skill for Claude Code

Generate parametric 3D CAD models from text descriptions using Claude Code. Exports STEP files (for SolidWorks, Onshape, Fusion 360) and STL files (for 3D printing) with interactive browser-based 3D preview.

## Quick Start

```bash
cd path/to/your/project
```

```bash
git clone https://github.com/Ericonaldo/Claude-CAD-skill.git
```

### 1. Copy the skill files into your project

```bash
# From your project root:
cp -r Claude-CAD-skill/.claude/skills/cad-3d .claude/skills/
cp Claude-CAD-skill/.claude/commands/cad.md .claude/commands/
cp Claude-CAD-skill/.claude/commands/cad-preview.md .claude/commands/
```

Or to install globally (available in all projects):

```bash
cp -r Claude-CAD-skill/.claude/skills/cad-3d .claude/skills/
cp Claude-CAD-skill/.claude/commands/cad.md .claude/commands/
cp Claude-CAD-skill/.claude/commands/cad-preview.md .claude/commands/
```

### 2. Install dependencies

```bash
pip install cadquery
```

### 3. Use it

In Claude Code, you can now:

```
# Use the slash command
/cad an L-bracket with 4 mounting holes, 60mm wide

# Or just describe what you want naturally
> Design an electronics enclosure, 80x50x30mm with screw bosses in the corners

# Or reference an image
> Look at this image and model something similar [attach image]
```

## How It Works

```
You describe a part
        ↓
Claude writes CadQuery Python code (parametric)
        ↓
Script runs → generates STEP + STL + mesh data
        ↓
HTML preview generated (Three.js, open in browser)
        ↓
You iterate: "make it wider", "add fillets", etc.
```

## File Structure

```
your-project/
├── .claude/
│   ├── skills/
│   │   └── cad-3d/
│   │       ├── SKILL.md              # Skill instructions (Claude reads this)
│   │       ├── scripts/
│   │       │   ├── cad_template.py   # Template for new models
│   │       │   ├── generate_preview.py  # Builds HTML preview from mesh
│   │       │   └── setup.sh          # One-time setup
│   │       └── templates/
│   │           └── viewer_template.html  # Three.js viewer template
│   └── commands/
│       ├── cad.md                    # /cad slash command
│       └── cad-preview.md           # /cad-preview slash command
│
└── cad_output/                       # Generated files (gitignore this)
    ├── model.py                      # The parametric CadQuery script
    ├── model.step                    # → Import into SolidWorks/Onshape/Fusion
    ├── model.stl                     # → Send to 3D printer slicer
    ├── mesh.json                     # Mesh data for preview
    └── preview.html                  # → Open in browser for 3D preview
```

## Output Files

| File | Purpose | Open With |
|------|---------|-----------|
| `model.step` | Industry-standard CAD format | SolidWorks, Onshape, Fusion 360, FreeCAD |
| `model.stl` | 3D printing mesh | PrusaSlicer, Cura, BambuStudio |
| `preview.html` | Interactive 3D preview | Any web browser |
| `model.py` | Parametric source code | Edit dimensions, re-run |

## Example Prompts

```
/cad a pipe flange, OD 80mm, 6 bolt holes on a 62mm circle

/cad phone stand with 15 degree tilt angle

/cad raspberry pi case with ventilation slots

/cad gear with 20 teeth, module 2, 10mm thick

/cad wall-mount bracket for a shelf, needs to support 5kg
```

## Tips

- **Be specific about dimensions** — Claude will use defaults if you don't specify
- **Mention the use case** — "for 3D printing" vs "for CNC machining" affects design choices
- **Iterate freely** — "make the walls thicker", "add a cable slot on the side"
- **The script is parametric** — you can edit the Python file directly and re-run
- **Preview in browser** — just `open cad_output/preview.html`

## Importing into CAD Software

### Onshape
1. Open a new document
2. Click the `+` button → "Import"
3. Upload `model.step`
4. The model imports as a solid body you can edit

### SolidWorks
1. File → Open → select `model.step`
2. Import as solid body
3. Use FeatureWorks to recognize features if needed

### Fusion 360
1. File → Open → Upload `model.step`
2. The timeline will show "Base Feature"
3. Right-click → "Edit in Place" to modify

## Requirements

- Python 3.8+
- CadQuery 2.x (`pip install cadquery`)
- Claude Code with a Claude Pro/Max subscription or API key
- A web browser (for preview)
