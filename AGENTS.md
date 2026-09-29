# AGENTS.md

## Project overview

This repository contains a static web application for assigning examination tickets to students.

- `index.html` contains the page structure and dialogs.
- `styles.css` contains the responsive visual design.
- `app.js` contains Excel/Word parsing, ticket assignment, history, and export logic.
- SheetJS and Mammoth are loaded from a CDN; there is no build step.

## Running locally

Start a static server from the repository root:

```bash
python -m http.server 4173
```

Open `http://localhost:4173` in a browser.

## Required behavior

- Read groups from the sheet names in `students.xlsx`.
- Read student surnames from column A and first names from column B, starting at row 2.
- Parse `tickets.docx` headings in the form `Билет N`.
- Ignore malformed tickets and tickets containing fewer than three numbered questions.
- Keep generation disabled until both a group and a student are selected and valid tickets exist.
- Assign a random ticket only on a student's first generation.
- Reuse the first assigned ticket for every subsequent generation for the same group, surname, and first name.
- Append every generation to the history and mark repeated generations with `да`.
- Preserve chronological history and export all six required columns to `results.xlsx`.
- Close the result dialog with Escape without closing or resetting the application.
- Handle missing, empty, malformed, or unavailable files with a visible message instead of an uncaught error.

## Change guidelines

- Keep the project usable as a static website without a build tool or backend.
- Preserve the current Russian user interface unless a task explicitly requests another language.
- Maintain responsive behavior for desktop and mobile layouts.
- Do not remove automatic browser storage of the journal.
- Escape user-provided workbook and document text before inserting it into HTML.
- Keep unrelated refactors out of focused changes.

## Manual verification

1. Load the demo data and confirm the generate button remains disabled until both selections are made.
2. Generate a ticket and verify the dialog shows exactly three questions.
3. Press Escape and confirm only the dialog closes.
4. Generate again for the same student and confirm the ticket number is unchanged and `Повтор` is `да`.
5. Confirm both journal rows remain in chronological order.
6. Export `results.xlsx` and verify the six required columns.
7. Test an empty group and a malformed ticket and confirm the application remains usable.

## Before committing

Run a JavaScript syntax check:

```bash
node --check app.js
```

Then inspect the page at desktop and mobile widths.
