# Canvas AI Page Context Generation Skill

An agentic skill that analyzes Drupal Canvas pages and produces reusable Markdown guideline documents that an AI Page Builder can use to automatically assemble new pages of the same type from content input alone.

## What it does

Given one or more Canvas page IDs and a page type, the skill:

1. Exports and parses the page structure via `ddev drush content:export`
2. Reads SDC component definitions, Twig templates, and CSS from the theme
3. Resolves color prop values to actual hex codes
4. Builds a visual model across all reference pages
5. Outputs a `<page-type>-page-guidelines.md` document structured for consumption by an AI page builder agent

The output document covers rendering constraints, the visual design system, section patterns, section ordering rules, and assembly rules — everything the component registry alone cannot provide.

---

## How to use this skill with Claude Code

### Step 1 — Copy the skill into your project

In your project root, create the `.claude/skills/` directory if it doesn't exist:

```bash
mkdir -p .claude/skills/canvas-ai-page-context-generation
```

Copy `SKILL.md` from this repo into that directory:

```bash
cp path/to/this/repo/.claude/skills/canvas-ai-page-context-generation/SKILL.md \
   .claude/skills/canvas-ai-page-context-generation/SKILL.md
```

Or clone this repo and copy the whole `.claude` directory into your project root.

Your project structure should look like:

```
your-project/
└── .claude/
    └── skills/
        └── canvas-ai-page-context-generation/
            └── SKILL.md
```

### Step 2 — Open the project in Claude Code

Launch Claude Code from your project root:

```bash
claude
```

Claude Code automatically loads skills from `.claude/skills/` in the working directory.

---

## Usage

### Automatic invocation

Claude will invoke this skill automatically when you say things like:

- "Generate page assembly guidelines for page IDs 42, 57, 103"
- "Create AI page builder context for our Article pages"
- "Analyze canvas pages and document how to build a Product page"
- "Generate component guidelines for Landing Pages"

### Manual invocation

You can also invoke it directly with the slash command:

```
/canvas-ai-page-context-generation
```

### What Claude will ask for

If not already provided, Claude will ask for:

- **Canvas page IDs** — one or more IDs of existing pages of the target type (comma-separated)
- **Page type** — e.g. `Article`, `Product`, `Landing Page`, `Event`

### Output

The skill saves a `<page-type>-page-guidelines.md` file to your project root. This file is intended to be fed as context to an AI page builder agent when assembling new pages.

---

## After generating the guideline document

Once the guideline document has been generated, add it to the site as a new **AI Context item** and attach it to both:

- **Canvas AI Template Builder** agent
- **Canvas AI Page Builder Agent**

This makes the guideline available as live context whenever either agent assembles or templates a page of that type.
