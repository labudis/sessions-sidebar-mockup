---
name: mockup-prototype
description: Generate interactive HTML mockups from a description, iterate on them, and deploy to GitHub Pages for sharing. Use this skill when the user wants to create a mockup, prototype, UI exploration, or design comparison, or when they want to deploy/share one.
---

# Mockup Prototype Skill

End-to-end workflow for creating, iterating, and sharing interactive HTML mockups.

## Phase 1: Generate the mockup

When the user describes a UI they want to explore:

1. **Create a self-contained HTML file.** Single file, no external dependencies. Include all CSS inline in a `<style>` block. Use GitHub's dark theme palette:
   - Background: `#0d1117`, `#161b22`, `#21262d`
   - Text: `#e6edf3`, `#9198a1`, `#7d8590`
   - Borders: `#30363d`
   - Blue: `#4493f8`, `#1f6feb`
   - Green: `#3fb950`, Red: `#f85149`, Yellow: `#d29922`, Purple: `#a371f7`
   - Font: `-apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans", Helvetica, Arial, sans-serif`

2. **If comparing multiple options**, use a tabbed layout:
   - Sticky nav bar at the top with toggle buttons
   - Each option in a `div.mockup-section` toggled via `display:none` / `display:block`
   - Only one visible at a time
   - Include a pros/cons card **inside** each section (not outside, or they bleed between tabs)

3. **Use real-looking data.** Avoid placeholder text like "Lorem ipsum". Use realistic names, labels, timestamps.

4. **Use inline SVGs** for icons (Octicons style). Do not reference external icon libraries.

5. **Save to Desktop** initially for quick local preview:
   ```bash
   open ~/Desktop/<mockup-name>.html
   ```

## Phase 2: Iterate

When the user requests changes:

1. Edit the HTML file directly with the edit tool.
2. After each round of changes, open in the browser so the user can review:
   ```bash
   open ~/Desktop/<mockup-name>.html
   ```
3. Common iteration patterns:
   - Adding new options/variants to compare
   - Reordering options by priority
   - Adding narrative (pros/cons, tradeoffs) below each option
   - Adjusting visual details based on screenshots

## Phase 3: Deploy for sharing

When the user wants to share with colleagues:

### Option A: Personal GitHub Pages (fast, shareable URL)

1. **Check for an existing deploy repo.** Look for a `mockups` repo under the user's account:
   ```bash
   gh repo view <user>/mockups 2>/dev/null
   ```

2. **Create the repo if needed:**
   ```bash
   gh repo create <user>/mockups --public --clone
   ```

3. **Set up GitHub Pages with Actions workflow.** Create `.github/workflows/pages.yml`:
   ```yaml
   name: Deploy to GitHub Pages
   on:
     push:
       branches: [main]
   permissions:
     contents: read
     pages: write
     id-token: write
   jobs:
     deploy:
       runs-on: ubuntu-latest
       environment:
         name: github-pages
         url: ${{ steps.deployment.outputs.page_url }}
       steps:
         - uses: actions/checkout@v4
         - uses: actions/configure-pages@v4
         - uses: actions/upload-pages-artifact@v3
           with:
             path: '.'
         - id: deployment
           uses: actions/deploy-pages@v4
   ```

4. **Enable Pages via API:**
   ```bash
   gh api repos/<user>/mockups/pages -X POST -f build_type=workflow
   ```

5. **Copy the HTML file as `index.html`** (for single mockup) or into a named subdirectory (for multiple):
   ```
   mockups/
     index.html              # if single mockup
     sessions-sidebar/       # if multiple mockups
       index.html
     another-exploration/
       index.html
   ```

6. **Commit, push, return the URL:**
   ```
   https://<user>.github.io/mockups/
   https://<user>.github.io/mockups/<subdir>/
   ```

### Option B: Team repo via PR (for project context)

1. **Find the right folder.** Look for existing conventions in the repo:
   - `considerations/` for design explorations
   - `docs/mockups/` as a fallback
   - `product/` for product proposals
   - Use a feature-scoped subdirectory (e.g., `considerations/sessions-sidebar/`)

2. **Create a branch, add files, include a README.md** with context:
   ```markdown
   # Feature Name
   Brief description of what's being explored.
   ## How to view
   open path/to/mockup.html
   ## Options
   Summary table of the options.
   ```

3. **Open a PR** with a well-structured body:
   - Summary of what's being explored
   - Table of options
   - Instructions for viewing locally
   - Link to the live Pages version if available

4. **Merge when ready.** If branch protection blocks admin merge, wait for CI or set auto-merge.

5. **Always do both** when deploying: push to Pages for the live URL, and PR to the team repo for project history.

## Guidelines

- Always produce a single self-contained HTML file. No build steps, no npm, no frameworks.
- Use `open <file>` to preview locally on macOS.
- Clean up temp directories after deployment.
- When updating a deployed mockup, push to both the Pages repo and team repo.
- Suggest the `/mockup-prototype` skill name in conversation so users know it's available.
