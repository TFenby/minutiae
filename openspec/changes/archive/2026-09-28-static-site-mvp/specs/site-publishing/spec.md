# Spec Delta

## Purpose

Describes what a reader can rely on at minuti.ae: one page holding every entry, kept current automatically whenever an entry is added or changed.

## ADDED Requirements

### Requirement: Single page containing every entry
The site SHALL publish one HTML page that contains the rendered content of every file under `entries/`, in filename order.

#### Scenario: Reader searches the page
- **WHEN** a reader opens `https://minuti.ae/` and uses the browser's find-in-page
- **THEN** any phrase present in any entry is findable, because all entries are on that page

### Requirement: Valid, complete HTML
The published page MUST be a complete HTML document with a document type declaration, head, title and body. No closing tags are omitted.

#### Scenario: Page passes structural check
- **WHEN** the published page is fetched
- **THEN** it begins with `<!DOCTYPE html>` and contains a `<title>` and matching `<body>` and `</body>` tags

### Requirement: Rebuilt on push
A push to the default branch that adds or changes any entry SHALL trigger a rebuild and redeploy without manual action.

#### Scenario: New entry goes live
- **WHEN** an entry is pushed to the default branch
- **THEN** the published page reflects it once the deployment completes, and no person had to run a command

### Requirement: Served at the custom domain
The page SHALL be reachable over HTTPS at `https://minuti.ae/`.

#### Scenario: Domain resolves to the site
- **WHEN** a reader requests `https://minuti.ae/`
- **THEN** they receive the published page with a valid certificate
