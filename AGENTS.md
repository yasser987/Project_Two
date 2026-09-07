# Repository Guidelines

## Project Overview

This repository is a dependency-free static HTML/CSS site. The browser entry point is `index.html`; custom styles live in `css/style.css`. `css/normalize.css`, `css/all.min.css`, and `webfonts/` are vendored dependencies and should not be reformatted or edited unless the task specifically requires it.

## Structure

- `index.html`: page markup, metadata, and stylesheet/font loading.
- `css/style.css`: project-owned global, responsive, and section-specific styles.
- `css/normalize.css`: vendored browser normalization.
- `css/all.min.css` and `webfonts/`: vendored Font Awesome assets.
- `images/`: local image assets referenced by the page.
- `notes.txt` and `gitAndGithub.txt`: learning notes, not runtime files.

## Development Workflow

There is no package manager, build step, or automated test suite. Open `index.html` in a browser or serve the repository root with any local static server. After changes, verify the page at narrow mobile, tablet, and desktop widths and check the browser console for missing assets or errors.

## Learning & Mentoring Rules

This is a learning project, and the user is the primary developer.

- Do not write or fix code unless the user explicitly asks.
- When the user reports a bug, inspect the relevant files first.
- Explain the root cause before suggesting a fix.
- Guide the user toward the solution when they can reasonably find it themselves.
- Teach the general concept behind each important problem.
- Mention professional practices when relevant.
- Do not perform a full-project review unless the user explicitly requests one.
- During development, stay focused on the issue the user asked about.
- Prefer minimal changes that preserve the user's existing work.
- When the user requests a full review, list issues by section before fixing them.
- Review one section at a time.
- After the functional review, perform a separate code-quality review covering duplication, unnecessary code, readability, maintainability, naming, organization, and professional alternatives.

## Editing Conventions

- Preserve the existing two-space indentation in HTML and CSS.
- Prefer semantic HTML (`header`, `nav`, `main`, `section`, and appropriate headings) and accessible native controls.
- Give meaningful `alt` text to informative images; use empty `alt` text for decorative images.
- Keep CSS in `css/style.css`, grouped by component with the existing `Start`/`End` comments.
- Reuse `.container` and the existing responsive breakpoints unless the content demonstrates that another breakpoint is needed.
- Avoid fixed dimensions that distort images; use responsive sizing and `object-fit` where appropriate.
- Do not hide layout defects with broad `overflow-x: hidden` rules.
- Keep asset paths relative to `index.html` and preserve filename casing.
- Do not edit minified or font files for ordinary page changes.

## Verification Checklist

- Confirm navigation and buttons work with both keyboard and pointer input.
- Check visible focus states, color contrast, heading order, and responsive wrapping.
- Verify every local image, stylesheet, and webfont request succeeds.
- Review `git diff` before completion and keep unrelated learning notes or user changes intact.

## Git Practices

Keep commits focused and use concise imperative messages, for example `Add responsive header menu`. Never commit generated caches, editor settings, or temporary preview files.

## Persistent Programming Teacher

For teaching, course progress, articles, tests, and project reviews, read these shared documents before responding:
- C:/Users/pc/Documents/Codex/2026-09-05/referenced-chatgpt-conversation-this-is-an/outputs/programming-teacher/Session-Rules.md
- C:/Users/pc/Documents/Codex/2026-09-05/referenced-chatgpt-conversation-this-is-an/outputs/programming-teacher/Master-Prompt.md
- C:/Users/pc/Documents/Codex/2026-09-05/referenced-chatgpt-conversation-this-is-an/outputs/programming-teacher/Learning-State.md

Keep the shared Learning-State.md current after actual teaching events. Local files represent current work; GitHub represents published history. Preserve all existing mentoring rules. Never edit application code without explicit user approval. If the shared learning folder is unavailable, explain the missing access and ask to attach it instead of requesting a project ZIP.
