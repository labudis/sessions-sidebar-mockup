# Rapid UI Prototyping with GitHub Copilot CLI

Generate interactive HTML mockups, iterate in real time, and deploy them to GitHub Pages for fast feedback loops.

## Setup

Copy the `mockup-prototype` skill to your Copilot CLI skills directory:

```bash
mkdir -p ~/.copilot/skills/mockup-prototype
curl -o ~/.copilot/skills/mockup-prototype/SKILL.md \
  https://raw.githubusercontent.com/labudis/mockups/main/skill/SKILL.md
```

Then reload skills in your CLI session:

```
/skills reload
```

## Workflow

### 1. Describe what you want

Be specific about the UI element and context. Good prompts:

```
mockup a sessions panel in the GitHub issues sidebar showing
coding agent sessions, triage runs, and planning sessions
```

```
create a comparison of 3 different ways to show deployment
status on a PR page, with pros and cons for each
```

Copilot generates a self-contained HTML file you can open locally.

### 2. Iterate

Review in your browser and ask for changes:

- "add a 4th option that uses a dropdown instead"
- "include pros/cons below each option"
- "reorder so the header pill option comes first"
- "the narrative is bleeding between tabs, fix it"

Each round updates the file and reopens it.

### 3. Deploy

Say "deploy this" or "share this with my team" and Copilot will:

- Push to a personal GitHub Pages repo for a live URL
- Open a PR on your team repo for project history

You get a shareable link like:

```
https://yourname.github.io/mockups/
```

### 4. Keep iterating

Further changes are deployed with each update. Both the Pages site and the team repo PR stay in sync.

## Tips

- **Be specific about the look.** Mention "GitHub dark theme" or "match the issues page style" to get consistent results.
- **Use screenshots.** Attach a screenshot when something looks wrong. Copilot can see it and fix it.
- **Compare options.** Ask for multiple variants up front. Tabbed comparisons with pros/cons are great for async feedback.
- **Name your explorations.** Use descriptive names like "sessions-sidebar" rather than "mockup-1".

## Example session

```
> mockup how deployment environments could appear on a PR page,
  show 3 options: inline status checks, a dedicated tab, and a
  collapsible panel. Include pros and cons.

  [Copilot generates the HTML, opens in browser]

> the status checks option needs a progress bar for each environment

  [Copilot updates, reopens]

> deploy this so I can share with the team

  [Copilot pushes to Pages, opens PR on team repo]
  Live at: https://you.github.io/mockups/
```
