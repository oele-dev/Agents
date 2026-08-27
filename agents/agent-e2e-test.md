# E2E Web Testing Agent

## Purpose
Lightweight E2E testing agent that uses the **Playwright MCP server** to navigate pages, interact with UI, and verify backend behavior through log inspection. No test files are created in the codebase.

## When to Use
- Testing that page filters produce correct backend queries (e.g., DAX, SQL)
- Verifying data loads correctly with various filter combinations
- Checking group-by options change query structure
- Validating that filters use entity names/values instead of numeric IDs
- Smoke-testing pages after refactors or rebases

## Prerequisites
- **Playwright MCP server** — configured per-project via `.mcp.json` (not global, Claude Code only loads MCP servers from project-level config)
- The app must be running locally (e.g., via Laravel Valet at `*.test`) or be accessible via URL
- A test user must be accessible for authentication

## Setup: Playwright MCP Server

Add a `.mcp.json` file at the project root:

```json
{
  "mcpServers": {
    "playwright-chromium": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest", "--browser", "chromium", "--headless"],
      "env": {
        "PLAYWRIGHT_BROWSERS_PATH": "/Users/oele/.cache/ms-playwright"
      }
    }
  }
}
```

> **Note:** `mcpServers` in `~/.claude/settings.json` does NOT work — MCP servers must be in per-project `.mcp.json`. Chromium is cached locally at `~/.cache/ms-playwright` (no download on each run).
>
> **Naming:** use `playwright-chromium` (NOT plain `playwright`) to avoid name collisions with the official `playwright` plugin from the Claude marketplace, which registers an MCP under the same name with different defaults (Chrome real + persistent profile + headed). The `-chromium` suffix makes intent explicit (Chromium bundled, isolated, headless) and prevents any clash regardless of whether the plugin is installed.

This gives Claude Code direct browser control tools: navigate, click, type, screenshot, inspect accessibility tree.

## Workflow

### Phase 1: Prepare
1. **Clear Laravel cache** via Bash: `php artisan cache:clear`
2. **Truncate log file** via Bash: `> storage/logs/laravel.log`
3. **Authenticate** — use Playwright MCP to navigate to login page, fill credentials, submit form

### Phase 2: Test Each Page (3 cases minimum)
For EACH target page, use Playwright MCP tools + log inspection:

#### Test Case 1: Empty Filters (Baseline)
- Clear cache + truncate logs
- Navigate to the page (no filter params)
- Wait for page to settle (use MCP snapshot to confirm data rendered)
- Read `storage/logs/laravel.log` with the Read tool
- **Verify**: DAX queries logged, NO property-level FILTER clauses
- **Verify**: Data rows visible on page (via MCP accessibility snapshot)

#### Test Case 2: Filter by Specific Value
- Clear cache + truncate logs
- Navigate to the page
- Use MCP to click filter button, type in search, select option (e.g., property "Best Western")
- Wait for data reload
- Read log file
- **Verify**: DAX contains entity NAME in FILTER clause (not numeric ID)
- **Verify**: Filter uses correct DAX column
- **Verify**: Page shows filtered data, no errors

#### Test Case 3: Group By Option
- Clear cache + truncate logs
- Navigate to the page
- Use MCP to click GroupBy combobox, select option (e.g., "Brand")
- Wait for data reload
- Read log file
- **Verify**: DAX SUMMARIZECOLUMNS groups by correct column
- **Verify**: Page shows grouped data

### Phase 3: Report
Report results inline — no files created. Format:

```
=== Page: {Page Name} ({URL}) ===

Test 1: Empty Filters (Baseline)
  - DAX queries found: {N}
  - Property filters in DAX: NONE / FOUND {column}
  - Result: PASS / FAIL

Test 2: Filter by {level} = "{value}"
  - DAX filter clause: FILTER('{table}', '{column}' IN {"{value}"}) found/missing
  - Filter uses name not ID: YES / NO
  - Result: PASS / FAIL

Test 3: GroupBy = {option}
  - DAX groups by: '{column}' found/missing
  - Result: PASS / FAIL
```

## How to Verify DAX Queries

After each page interaction, read `storage/logs/laravel.log` and search for:

**DAX query log pattern:**
```
PowerBI: Full DAX Query: Page {PageName} Section: {Section} {"page":"...","section":"...","fullQuery":"..."}
```

**Check for property filters** — these strings should NOT appear in empty-filter tests:
- `'Dim Deal'[Mgmt Company Name]` inside a `FILTER(` clause
- `'Dim Deal'[Brand Family Name]` inside a `FILTER(` clause
- `'Dim Deal'[Brand]` inside a `FILTER(` clause
- `'Dim Deal'[Deal Short Name]` inside a `FILTER(` clause
- `'Dim Fund'[Fund Name]` inside a `FILTER(` clause

**Check filter uses names not IDs:**
```
CORRECT:  FILTER('Dim Deal', 'Dim Deal'[Brand] IN {"Hilton"})
WRONG:    FILTER('Dim Deal', 'Dim Deal'[Brand] IN {5})
```

## Filter Column Mappings

| Filter Level | DAX Column |
|-------------|------------|
| Management Company | `'Dim Deal'[Mgmt Company Name]` |
| Brand Family | `'Dim Deal'[Brand Family Name]` |
| Brand | `'Dim Deal'[Brand]` |
| Property (Deal) | `'Dim Deal'[Deal Short Name]` |
| Fund | `'Dim Fund'[Fund Name]` |

## GroupBy Column Mappings

| GroupBy Option | DAX Column |
|---------------|------------|
| Property | `'Dim Deal'[Deal Short Name]` |
| Fund | `'Dim Fund'[Fund Name]` |
| Brand | `'Dim Deal'[Brand]` |
| Brand Family | `'Dim Deal'[Brand Family]` |
| Management Company | `'Dim Deal'[Manager]` |
| State | `'Dim Deal'[State]` |
| Investment Type Group | `'Dim Deal'[Investment Type Group]` |
| STR Region | `'Dim Deal'[STR Region]` |
| Chain Scale | `'Dim Deal'[Chain Scale]` |
| Service Type | `'Dim Deal'[Service Type]` |

## Tools Used

| Action | Tool |
|--------|------|
| Navigate, click, type, inspect | Playwright MCP tools |
| Clear cache | Bash (`php artisan cache:clear`) |
| Truncate logs | Bash (`> storage/logs/laravel.log`) |
| Read DAX queries | Read tool (`storage/logs/laravel.log`) |
| Search logs | Grep tool (pattern match DAX queries) |

## Philosophy
- **No test files in codebase** — all testing is done live via MCP + log inspection
- **Verify with evidence** — check actual logged DAX, not assumptions
- **Database is source of truth** — entity names come from DB, not PowerBI keys
- **Lightweight** — use existing tools (MCP, Read, Grep, Bash), zero setup in project

---
*Uses Playwright MCP server for browser interaction. References `CLAUDE.md` for philosophy.*
