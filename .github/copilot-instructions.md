# Copilot Instructions for my-horizon Theme

## Scope
- Follow these instructions for all changes in this workspace.
- Prefer minimal, targeted edits that preserve existing behavior and naming.

## Session Start Sync Policy
- At the start of each new coding session, run a single theme sync before edits:
  - `shopify theme pull --theme 199280329034`
- After that initial sync, do not auto-sync again in the same session unless explicitly requested.
- If sync fails, stop and report the exact error and a short next-step suggestion.
- After each completed change, report the list of modified files.

## Project Context
- This is a Shopify theme using Liquid templates and modular JavaScript in assets.
- JavaScript uses strict type checking via JSDoc and jsconfig settings in assets/jsconfig.json.
- Modules use the `@theme/*` path alias for internal imports.

## General Guardrails
- Match existing code style and architecture before introducing new patterns.
- Avoid large rewrites, broad refactors, and formatting-only diffs unless requested.
- Preserve accessibility and performance behavior already present in nearby code.
- Keep comments brief and only for non-obvious logic.

## JavaScript Conventions
- Use ES modules and named imports/exports consistent with surrounding files.
- For UI behavior, prefer existing custom elements architecture:
  - Extend `Component` from assets/component.js when building interactive elements.
  - Use `ref` attributes and `requiredRefs` instead of repeated query selectors.
  - Use lifecycle methods (`connectedCallback`, `disconnectedCallback`, `updatedCallback`) correctly.
- Clean up all listeners, observers, and animation frame IDs in `disconnectedCallback`.
- Preserve null safety and JSDoc typing patterns expected by `checkJs` and strict null checks.
- Reuse utilities from assets/utilities.js (debounce, throttle, requestIdleCallback, document-ready helpers) before adding new helpers.
- For non-critical work, defer execution with requestIdleCallback/document-ready helpers to reduce main-thread contention.

## Liquid Conventions
- Keep section/snippet composition patterns intact (`render`, `content_for`, block-driven layouts).
- Use `{% liquid %}` blocks for multi-step assignments and control flow when practical.
- Keep schema JSON valid and preserve existing key ordering/style unless a change requires updates.
- Maintain translation key usage (`t:` keys) and avoid hardcoding user-facing strings where localized keys exist.
- Preserve accessibility attributes and semantic structure (for example `aria-*`, `role`, skip links, `aria-current`).

## CSS and Styling Conventions
- Prefer existing design tokens and CSS custom properties over hardcoded values.
- Follow existing breakpoint patterns (notably around 750px) and layout utilities already in use.
- Keep CSS changes localized to the related section/snippet/component.

## Section Rendering and Hydration
- Preserve section hydration/rendering flows and avoid breaking data attributes used by JS.
- Do not remove or rename IDs, `data-*` attributes, `ref` attributes, or custom element tags without updating all dependent JS/Liquid code.

## Change Validation Checklist
- Verify imports resolve with `@theme/*` alias where appropriate.
- Verify custom element cleanup and event wiring are symmetrical.
- Verify Liquid output still matches expected structure for linked scripts/styles.
- Verify no regressions to a11y attributes, keyboard flow, or reduced-motion handling.
- If behavior changes, document assumptions and edge cases in the PR/summary.
