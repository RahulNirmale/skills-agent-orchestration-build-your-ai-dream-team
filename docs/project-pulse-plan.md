# Project Pulse implementation plan

## Summary

Build a small, static Project Pulse dashboard that helps contributors scan active projects, owners, status, recent activity, priority or risk, and a concise contributor-friendly summary. Follow the repository brief and exercise checks: make the dashboard polished, responsive, accessible, data-driven from JSON, and previewable through a VS Code launch configuration that opens the page rather than a directory listing.

The Planner owns this plan only. The Designer will provide visual, information-hierarchy, responsive, and accessibility guidance. The Coder will implement the agreed design in the app and launch files. The Orchestrator coordinates the handoffs, keeps file scopes explicit, and validates the integrated result. No production implementation is included in this planning step.

## Ordered implementation steps and file assignments

### 1. Confirm requirements and agree the design direction

- **Planner:** This plan, `docs/project-pulse-plan.md`.
- **Designer:** No repository file changes; provide a concise design direction for dashboard hierarchy, project-card layout, status/priority treatments, responsive behavior, and accessibility.
- **Orchestrator:** Confirm Designer guidance satisfies `.github/project-pulse-brief.md` and the checks in `.github/steps/3-step.md`; resolve any conflicts before coding.
- **Dependency:** Complete before implementation.

### 2. Implement the data contract and dashboard markup

- **Coder:** `app/project-data.json` and `app/index.html`.
- Use a top-level `projects` array. Every project object must include `name`, `owner`, `status`, `recentActivity`, and `priority`; provide short, contributor-friendly project context/summary as well, since the brief requests it.
- Use the exact page title **Project Pulse**. Link `styles.css`, load `project-data.json`, and render one visible `.project-card` per project. Show name, owner, status, recent activity, priority/risk, and summary.
- Keep rendering robust if data cannot be loaded or is malformed: show a clear, accessible error/empty state rather than a blank dashboard. Use semantic HTML, meaningful headings, and accessible text labels; do not rely on color alone for status or priority.
- **Dependency:** Agree on the JSON shape and card structure before styling is finalized.

### 3. Apply the visual system and responsive styling

- **Designer:** Provide design guidance only; no file ownership to avoid conflicts with implementation.
- **Coder:** `app/styles.css`.
- Implement the agreed hierarchy and dashboard/card layout, including the required `.dashboard` and `.project-card` selectors, clear status badges and priority treatment, readable spacing and typography, rounded corners, and shadows. Add responsive behavior for narrow screens and visible keyboard focus where interactive elements exist. Ensure contrast and reduced-motion considerations for any motion.
- **Dependency:** Follows the markup/class contract from step 2 and the Designer’s direction. This step may be developed alongside launch configuration after the markup contract is settled.

### 4. Add the VS Code preview configuration

- **Coder:** `.vscode/launch.json`.
- Create strict JSON without comments. Add the exact configuration name **Run Project Pulse Dashboard**; use `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html` so the UI opens rather than a directory listing.
- **Dependency:** Independent of CSS once the app directory and `index.html` entry point are agreed; can run in parallel with step 3 if the Coder’s file scopes are split explicitly.

### 5. Integrate and validate

- **Orchestrator:** Review all output files together; no additional file assignment unless a fix is needed. Delegate fixes to the original owner of the affected file.
- **Coder:** Run static checks and address implementation defects within assigned files.
- Confirm the preview starts through **Run Project Pulse Dashboard**, serves from `app/`, opens `index.html`, and displays populated cards. Stop the server after previewing.
- **Dependency:** Requires steps 2–4 to be complete.

## Dependencies and parallel work

- The Designer’s direction should precede coding so visual and accessibility decisions are not retrofitted.
- The JSON schema and HTML card structure should be settled before final CSS styling.
- `app/styles.css` and `.vscode/launch.json` can be implemented in parallel after the HTML entry point and `app/` serving requirement are agreed, because they are separate files and have no data dependency. Keep the assignments disjoint.
- Implementing JSON and HTML can be coordinated in one Coder task, or split only if the schema is agreed first and the files have clear, non-overlapping ownership.
- Integration/preview validation must happen after all app and launch files are ready.

## Edge cases and risks

- The dashboard must still communicate status and priority without color alone; check contrast, text labels, keyboard focus, and small-screen readability.
- Handle empty, missing, or invalid project data with an understandable state. Confirm JSON remains valid and property names match the markup’s data access.
- A static page opened directly with `file://` may not be able to fetch JSON consistently; use the configured local HTTP server for preview.
- The launch working directory and URL must agree: serve `app/` and open `/index.html`, not the server root.
- Port 5500 may already be occupied. If the preview fails for that reason, stop the other server or choose a consistent alternate port and update both the launch configuration and URL.
- Use only static assets and avoid adding frameworks, packages, or build steps unless the repository’s existing setup requires them.

## Validation expectations

1. Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
2. Parse `app/project-data.json` as JSON; check it has a top-level `projects` array and each record has `name`, `owner`, `status`, `recentActivity`, and `priority` (plus contributor-friendly summary text).
3. Check `app/index.html` has the exact title **Project Pulse**, references `styles.css` and `project-data.json`, and renders project cards with status, recent activity, and priority.
4. Check `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, and verify responsive layout and accessibility guidance in the rendered page.
5. Parse `.vscode/launch.json` as strict JSON. Confirm the named launch configuration, command, `cwd`, and browser URL meet the requirements.
6. Run **Run Project Pulse Dashboard** and verify the browser shows the dashboard at `index.html` with data loaded and no directory listing or console/network errors.
7. Review the final integrated result against `.github/project-pulse-brief.md` and the Step 3 grading checks.

## Open questions

- The brief does not specify a project count or actual project names, owners, activity, and priorities; use representative sample projects unless Mona’s team supplies real data.
- The brief requests a contributor-friendly summary but the required project fields do not name a `summary` property. Add a `summary` field unless the team prefers a different field name.
- The repository does not specify a visual brand palette. The Designer should choose an accessible, restrained palette consistent with a contributor-facing dashboard.
