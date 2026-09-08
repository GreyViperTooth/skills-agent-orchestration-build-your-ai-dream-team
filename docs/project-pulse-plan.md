# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s lightweight **Project Pulse** dashboard as a static, contributor-friendly web app. The dashboard will present multiple projects as accessible, responsive cards showing each project’s name, owner, current status, recent activity, priority or risk level, and a concise summary.

The implementation will use the existing multi-agent workflow:

- **Orchestrator** coordinates the phases, delegates work, and performs the final integration review.
- **Planner** defines this implementation plan and file ownership.
- **Designer** defines the dashboard’s information hierarchy, visual language, accessibility requirements, and responsive behavior.
- **Coder** implements the HTML, data, and VS Code launch configuration, then integrates the approved design.

The repository currently contains the Project Pulse brief and custom agent definitions, but the learner-facing application files do not yet exist. No application framework or package manifest is required; the app should remain dependency-free and use the browser’s native HTML, CSS, and JavaScript capabilities.

## Target files and ownership

| File | Primary owner | Responsibilities |
|---|---|---|
| `app/index.html` | Coder | Create the semantic dashboard shell, link `styles.css`, load `project-data.json`, render project cards, and expose all required project fields in the UI. |
| `app/styles.css` | Designer | Define the visual system, dashboard layout, card styling, status and priority treatments, typography, spacing, responsive behavior, focus states, and accessible color contrast. |
| `app/project-data.json` | Coder, with Designer input | Provide representative project content in a top-level `projects` array. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `.vscode/launch.json` | Coder | Provide strict JSON for the **Run Project Pulse Dashboard** launch configuration, serving `${workspaceFolder}/app` with `python3 -m http.server 5500` and opening `index.html`. |
| `docs/project-pulse-plan.md` | Planner | Preserve the implementation plan and coordination contract. |
| `docs/final-handoff.md` | Orchestrator | Document the participating agents, final implementation, validation results, and known limitations after the build. |

The Designer may provide a visual specification before implementation and may directly create or refine `app/styles.css`. The Coder must not overwrite design decisions without coordinating with the Designer. The Coder owns the integration of the final HTML, JSON, and launch configuration and is responsible for ensuring the styling hooks used by the HTML match the stylesheet.

## Functional requirements

The finished dashboard must:

- Use the exact page title or prominent heading **Project Pulse**.
- Render multiple visible project cards using the class name `.project-card`.
- Display each project’s:
  - Name
  - Owner
  - Status
  - Recent activity
  - Priority or risk level
  - Contributor-friendly summary where appropriate
- Load project content from `app/project-data.json`, whose root object contains a `projects` array.
- Reference both `styles.css` and `project-data.json` from `app/index.html`.
- Use a clear dashboard container with the `.dashboard` selector.
- Use polished card styling that includes rounded corners and shadows through `border-radius` and `box-shadow`.
- Remain usable on narrow screens and desktop layouts.
- Avoid requiring a build step, external package installation, or a backend application.

## Designer responsibilities

The Designer should establish and communicate:

1. **Information hierarchy**
   - Make the Project Pulse heading and overall project count or overview easy to find.
   - Give project names and statuses strong visual prominence.
   - Keep owner, recent activity, priority, and summary information scannable.

2. **Visual design**
   - Create a polished card-based dashboard rather than a plain document.
   - Define consistent spacing, typography, color tokens, borders, rounded corners, and shadows.
   - Use distinct but understandable visual treatments for statuses and priority levels.
   - Ensure risk or priority indicators do not rely on color alone; include visible text labels.

3. **Accessibility**
   - Use semantic landmarks and heading levels.
   - Ensure sufficient color contrast.
   - Preserve visible keyboard focus states.
   - Use meaningful labels for status, priority, and activity values.
   - Ensure the layout remains readable when text wraps or viewport width is reduced.
   - Respect reduced-motion preferences if transitions or animations are introduced.

4. **Responsive behavior**
   - Use a flexible grid or equivalent layout that collapses gracefully on small screens.
   - Prevent cards, badges, and metadata from overflowing.
   - Keep the first viewport recognizable as a Project Pulse dashboard at mobile widths.

## Coder responsibilities

The Coder should:

1. Create `app/project-data.json` with several representative projects and the required field names.
2. Create `app/index.html` with:
   - Standard document metadata and the exact `Project Pulse` title or heading.
   - A stylesheet link to `styles.css`.
   - A dashboard container using `.dashboard`.
   - A project-card template or rendering region using `.project-card`.
   - JavaScript that fetches and parses `project-data.json`, then renders the project fields.
   - Explicit loading and error states so a failed data request is visible rather than silently ignored.
3. Preserve the JSON field names exactly:
   - `name`
   - `owner`
   - `status`
   - `recentActivity`
   - `priority`
4. Use stable class hooks that match the Designer’s stylesheet.
5. Create `.vscode/launch.json` as strict JSON with no comments:
   - Add a configuration named `Run Project Pulse Dashboard`.
   - Serve from `${workspaceFolder}/app`.
   - Use `python3 -m http.server 5500`.
   - Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
   - Ensure the browser opens the dashboard page instead of the app directory listing.
6. Validate the integrated result in a browser and fix markup, data, or launch issues found during review.

## Ordered implementation phases

### Phase 1: Confirm scope and establish the contract

**Owner:** Orchestrator and Planner

Review:

- `.github/project-pulse-brief.md`
- `.github/agents/designer.agent.md`
- `.github/agents/coder.agent.md`
- Existing repository conventions and validation workflows

Confirm that the implementation is a dependency-free static app and that all four required output files are in scope.

**Deliverable:** Agreed file ownership, data shape, design requirements, and launch behavior.

### Phase 2: Produce the design direction

**Owner:** Designer

Define the dashboard layout and visual treatment before the implementation is finalized. The Designer should specify:

- Page structure and semantic regions
- Card layout and responsive breakpoints
- Status and priority badge treatment
- Typography and spacing hierarchy
- Accessibility and keyboard-focus requirements
- Required CSS hooks, including `.dashboard` and `.project-card`

The Designer may implement the initial `app/styles.css` within the assigned file scope.

**Dependency:** Phase 1 must be complete.

### Phase 3: Create the project data

**Owner:** Coder, informed by Designer

Create `app/project-data.json` with a top-level `projects` array and multiple realistic entries. Each entry must contain all required fields and values suitable for visible status and priority treatments.

The data should cover more than one status and priority so the UI can demonstrate its visual states. Text should be concise enough for cards but realistic enough to exercise wrapping and responsive behavior.

**Dependency:** The required data contract from Phase 1 must be agreed. This work can run in parallel with the Designer’s stylesheet work once the contract is fixed.

### Phase 4: Implement the dashboard markup and rendering

**Owner:** Coder

Create `app/index.html` and connect it to the Designer’s stylesheet and the JSON data. The page should:

- Render the dashboard shell immediately.
- Fetch `project-data.json` relative to the page.
- Render one `.project-card` per project.
- Display all required project fields.
- Present readable loading and data-error states.
- Keep the data-to-markup mapping explicit and easy to maintain.

**Dependency:** The data field contract must be settled. The Coder can work in parallel with Phase 3 if the schema is stable, but final integration must wait for the actual JSON file.

### Phase 5: Add the runnable preview configuration

**Owner:** Coder

Create `.vscode/launch.json` with the exact launch name and server behavior. The launch configuration must serve the `app` directory and open `index.html`, because fetching `project-data.json` from a `file://` URL is not a reliable preview path and opening the directory root would show a listing instead of the dashboard.

**Dependency:** The expected app path and port must be agreed. This can be authored in parallel with the HTML and CSS, but it must be tested after all app files exist.

### Phase 6: Integrate and review

**Owner:** Orchestrator, Designer, and Coder

Review the four implementation files together:

- Confirm HTML class hooks match CSS selectors.
- Confirm JSON field names match the rendering code.
- Confirm loading and error states are visible.
- Confirm badges and metadata remain readable across viewport sizes.
- Confirm the launch configuration opens the actual dashboard.
- Resolve any design or implementation conflicts before validation.

**Dependency:** Phases 2–5 must be complete.

### Phase 7: Validate and hand off

**Owner:** Orchestrator

Run the structural and browser validation described below. Record the final result and any limitations in `docs/final-handoff.md`.

**Dependency:** Integration review must be complete.

## Dependencies

### Runtime dependencies

- Python 3 for `python3 -m http.server 5500`.
- A browser capable of running standard HTML, CSS, JavaScript, and `fetch`.
- VS Code or Codespaces support for `.vscode/launch.json`.
- No npm packages, build tools, external APIs, or additional libraries are required.

### File dependencies

- `app/index.html` depends on `app/styles.css` for presentation.
- `app/index.html` depends on `app/project-data.json` for project content.
- The JavaScript data loader requires the app to be served over HTTP rather than opened directly from the filesystem.
- `.vscode/launch.json` depends on the `app/` directory and `index.html` existing at the configured paths.
- The HTML and CSS must share agreed class names, especially `.dashboard` and `.project-card`.
- The rendering logic and JSON must share the exact required field names.

## Parallel and sequential work

### Work that can run in parallel

After the data contract and file ownership are agreed:

- The Designer can create `app/styles.css`.
- The Coder can create `app/project-data.json`.
- The Coder can draft `.vscode/launch.json`.
- The Coder can prepare the semantic HTML structure and rendering approach in `app/index.html`.

These tasks are safe to parallelize because they have separate primary file ownership, provided the shared data schema and CSS hooks are documented first.

### Work that must be sequential

1. Repository and brief review must precede implementation.
2. The shared data schema and CSS hooks must be agreed before parallel implementation begins.
3. Final HTML integration must follow the Designer’s stylesheet contract and the agreed JSON shape.
4. Browser validation must follow creation of all app files and the launch configuration.
5. Final handoff documentation must follow validation so it reports the actual result rather than intended behavior.

## Edge cases and error states

- `project-data.json` is missing or cannot be fetched.
- `project-data.json` contains invalid JSON.
- The root object lacks `projects` or `projects` is not an array.
- A project is missing one of the required fields.
- A field contains an empty string or unusually long text.
- Status or priority values are unexpected and do not have a predefined visual variant.
- The browser opens the server root and displays a directory listing instead of `index.html`.
- The app is opened directly as a local file and the data request is blocked.
- The dashboard is viewed at a narrow mobile width.
- Keyboard users cannot see focus or distinguish status and priority from color alone.
- Long owner names or activity text overflow a card.
- Port `5500` is already in use; the launch configuration and validation should surface this clearly rather than silently switching ports.
- A stale server is already running, causing the preview to show old files; stop it and restart after changes.

The implementation should fail visibly and informatively for data-loading problems. It should not silently render an empty dashboard when the source data is unavailable.

## Validation expectations

### Static file validation

Confirm that:

- `app/index.html` exists and contains `Project Pulse`.
- `app/index.html` references `styles.css`.
- `app/index.html` references `project-data.json`.
- `app/index.html` contains or generates `.project-card` elements.
- The rendered markup exposes `status`, `recentActivity`, and `priority`.
- `app/styles.css` contains `.dashboard` and `.project-card`.
- `app/styles.css` includes `border-radius` and `box-shadow`.
- `app/project-data.json` parses as valid JSON.
- The JSON root contains `projects`.
- Every project contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json` parses as strict JSON with no comments.
- `.vscode/launch.json` contains `Run Project Pulse Dashboard`.
- The launch configuration uses `python3 -m http.server 5500`.
- The server working directory is `${workspaceFolder}/app`.
- `serverReadyAction` opens `http://localhost:%s/index.html`.

Use the repository’s existing validation conventions, including `python3 -m json.tool` for both JSON files where applicable. The existing exercise workflow also checks the required paths, selectors, key phrases, JSON structure, and launch configuration content.

### Browser validation

Using **Run Project Pulse Dashboard**:

- Verify the server starts successfully.
- Verify the browser opens `index.html`, not a directory listing.
- Verify the page heading identifies Project Pulse.
- Verify multiple project cards are visible.
- Verify each card shows the project name, owner, status, recent activity, and priority.
- Verify the data is loaded from `project-data.json`.
- Verify the layout is readable on desktop and narrow viewport widths.
- Verify status and priority remain understandable without relying only on color.
- Verify keyboard focus is visible for interactive elements, if any are introduced.
- Temporarily test or inspect the data-loading failure path to ensure an error is surfaced clearly.
- Stop the preview server after validation.

### Handoff validation

`docs/final-handoff.md` should identify:

- Orchestrator, Planner, Designer, and Coder.
- The completed Project Pulse dashboard.
- `app/index.html`, `app/styles.css`, and `app/project-data.json`.
- `.vscode/launch.json`.
- The launch name **Run Project Pulse Dashboard**.
- Structural and browser validation performed.
- Any remaining limitations, such as the absence of filtering, sorting, persistence, or backend integration.

## Open questions

No blocking questions remain based on the repository brief. The following implementation decisions may be made by the Orchestrator and recorded in the handoff if relevant:

- Which representative project names and statuses best reflect Mona’s team.
- Whether the contributor-friendly summary is stored as an additional optional JSON field or derived from the required fields.
- Whether the preview should open in the system browser or a VS Code-integrated browser, provided it opens `index.html`.
- Whether a small summary metric, such as active-project count, is included above the cards without expanding the required data model.
