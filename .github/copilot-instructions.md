# Crônicas de Westeros — AI Agent Guidelines

## Project Overview

**Crônicas de Westeros** is a **MkDocs-based wiki** for a Game of Thrones-inspired RPG played via WhatsApp. The site publishes to GitHub Pages and serves as the authoritative source for lore, rules, houses, factions, and characters.

- **Build tool**: MkDocs with Material theme
- **Language**: Portuguese (all documentation)
- **Deployment**: GitHub Actions → GitHub Pages
- **Content structure**: `docs/` folder with organized markdown subdirectories

## Essential Architecture

### Content Organization

The `docs/` folder mirrors the navigation structure in `mkdocs.yml`:

```
docs/
├── index.md                    # Homepage
├── lore/                       # World history (index.md + historia.md)
├── casas/                      # Playable Houses (index + individual house files)
├── faccoes/                    # Factions within houses (mirrors casas structure)
├── personagens/                # Characters by house/role
├── regras/                     # Game systems & rules
└── como-jogar.md              # WhatsApp gameplay guide
```

**Key pattern**: Most sections have `index.md` (overview) + individual markdown files. The generated `site/` folder is auto-built—**never edit directly**.

### Navigation Configuration

`mkdocs.yml` is the single source of truth for:
- Site name, URL, and deployment settings
- Navigation structure (the `nav:` key)
- Theme configuration (Material theme with Portuguese locale)

**When adding new pages**:
1. Create markdown in `docs/`
2. Add entry to `mkdocs.yml` nav section
3. Push to `main` branch—GitHub Actions auto-deploys

### Content Pattern Example

Each house/faction section follows this structure:
- **Index page** (`casas/index.md`): Overview of all houses
- **Individual pages** (`casas/stark.md`, `casas/lannister.md`): Detailed house history, structure, characters
- Cross-references between related sections (e.g., `casas/` ↔ `faccoes/` for the same house)

## Development Workflow

### Building & Testing Locally

```bash
# Install dependencies
pip install mkdocs mkdocs-material

# Build and serve locally (rebuilds on file changes)
mkdocs serve
# Then navigate to http://127.0.0.1:8000
```

### Deployment

GitHub Actions automatically:
1. Triggers on pushes to `main` branch (see `.github/workflows/deploy.yml`)
2. Installs MkDocs + Material theme
3. Runs `mkdocs gh-deploy --force`
4. Deploys to `gh-pages` branch → GitHub Pages

**Never manually edit `site/` folder** — it's regenerated on each deploy.

## Content Guidelines

### Language & Tone

- **All content is in Portuguese** (pt)
- **Lore-first approach**: Treat game mechanics as in-world consequences, not abstract rules
- **Narrative structure**: History flows chronologically; politics are presented as character motivations, not game mechanics
- Example: *"Casa Stark foi a última Casa a governar todo Westeros"* rather than *"Stark has the strongest military stat"*

### Markdown Patterns

- **Headers**: Use `#` for page title, `##` for major sections
- **Emphasis**: Markdown formatting (`**bold**` for names/titles, `*italics*` for emphasis)
- **Lists**: Use `-` for unordered, indentation for nested structure
- **Cross-references**: Use relative links `[link text](../other-section/file.md)`

### Lore-Specific Conventions

- **Timeline format**: Uses "d.B" (depois de Bran / after Bran Stark era 1)
- **Character introduction**: Full name + title/role on first mention
- **House structure**: Always mention bloodline connections and regional control
- **Political implications**: Every lore detail should connect to current power dynamics

## Common Tasks

### Adding a New House or Faction

1. Create `docs/casas/newhouse.md` and `docs/faccoes/newhouse.md` (parallel structure)
2. Update `mkdocs.yml` `nav:` section to include both
3. Add entries to `casas/index.md` and `faccoes/index.md` with links
4. Include cross-references in new files linking to related pages

### Updating Character Lists

Characters are organized by house (e.g., `personagens/starks.md`). When adding characters:
- List name, title, and brief description
- Link to their house page
- Note their political alignment/faction if relevant

### Expanding Incomplete Sections

Many sections contain placeholder text *"Em construção"* (Under construction). Replace with:
- Structured content following established markdown patterns
- Appropriate cross-references to related pages
- Lore-consistent terminology and timeline references

## Critical Files for Navigation & Build

- `mkdocs.yml`: **The source of truth** for structure and theme settings
- `.github/workflows/deploy.yml`: Deployment automation (read-only unless updating CI/CD)
- `docs/index.md`: Homepage—explains the project's purpose
- `docs/lore/historia.md`: Master timeline reference for all historical content
