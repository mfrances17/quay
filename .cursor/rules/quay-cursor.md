Migration Rules

Codebase Structure
    •   Legacy Angular code: located in \static.
    •   Never modify Angular code.
    •   Use only for reference (e.g. related React components, text content, API endpoints).
    •   New React code: located in \web\src and all subdirectories.
    •   All new work must go in \web\src.

⸻

General Guidelines
    •   Always follow React best practices.
    •   Always use Patternfly 5 components, and follow Patternfly 5 best practices.
    •   Reference official docs & demos:
        •   Docs: https://v5-archive.patternfly.org
        •   Components: https://v5-archive.patternfly.org/components/all-components
    •   Use previously converted PRs as models for patterns, structure, and best practices:
        •   https://github.com/quay/quay/pull/3870/files
        •   https://github.com/quay/quay/pull/3491/files
        •   https://github.com/quay/quay/pull/4121/files
        •   https://github.com/quay/quay/pull/4165/files
        •   https://github.com/quay/quay/pull/4174/files

⸻

Typescript Guidelines
    •   Never use the any type, it is bad practice. Always attempt to use a proper data type.

⸻

Forms
    •   Always use react-hook-form for all forms.
    •   Example reference: https://github.com/quay/quay/pull/4121/files
    •   Prefer controlled components with validation through react-hook-form.
    •   Follow Patternfly form layout and accessibility guidelines.

⸻

Styling
    •   Prefer Patternfly default styling whenever possible.
    •   For unique styles, use Patternfly variables/tokens.
    •   Never use inline styles, or embed styles directly in .ts or .tsx.
    •   Never create raw .css files for overrides.
    •   If overrides are unavoidable:
            •   Use SCSS with Patternfly tokens.
            •   Prefer className with Patternfly utility classes first.
            •   Prompt before introducing new SCSS overrides.

⸻

API Usage
    •   API endpoints are defined in Python files under \endpoints.
    •   When writing React API calls:
    •   Always check for an existing endpoint before using.
    •   Never modify backend code or create new endpoints.
    •   Prefer existing client utilities/hooks for requests if available.

⸻

Linting & Formatting
    •   Respect the existing ESLint/Prettier setup.
    •   Do not change rules.
    •   Never fix any linting or Problems pane issues unless explicitly instructed.

⸻

Tables
    •   Always use the QuayTable component (/web/src/components/QuayTable.tsx) when creating new or modifying existing tables.
    •   Always adhere to Patternfly best practices for tables.
    •   Always use the usePaginatedSortableTable hook (/web/src/components/QuayTable.tsx) for adding sort functionality to tables.
    •   Columns within tables must have sort functionality.
    •   Use the table in RepositoryList.tsx (/web/src/routes/RepositoryList/RepositoryList.tsx) or other tables we have already migrated as the model for adding tables and for table column sorting.

⸻

Cypress Tests
    •   Tests live in `\web\cypress\e2e`. Add new tests there.
    •   Follow Cypress best practices and existing test style.
    •   Use existing e2e tests as models. In particular, match style from:
            •   `notification-drawer.cy.ts`
            •   `mirroring.cy.ts`
    •   File names must end with `.cy.ts` and be descriptive of the feature.
    •   Mock API calls using `cy.intercept`, following patterns in `mirroring.cy.ts`.
    •   Prefer explicit waits/assertions (checking DOM or requests) over arbitrary `cy.wait()` calls.
    •   Do not refactor or modify existing tests unless asked.
    •   When adding data-testid to Select, add it to the MenuToggle inside the Select, not to the Select itself.
