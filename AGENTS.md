# AGENTS.md

This document provides guidelines for agentic coding agents working in this repository.

## Project Overview

**Name**: 阵型生成器 (Formation Generator)
**Type**: Single-page web application
**Tech Stack**: Vanilla HTML/CSS/JavaScript (no frameworks)
**File**: `index.html` (~1200 lines)
**Language**: Chinese (Simplified)

## Build & Run

```bash
# No build required - open index.html directly in browser
# For development, use any static file server:
npx serve .          # serve current directory
python -m http.server 8000  # Python built-in server

# No lint/typecheck/test commands configured
# Manual browser testing required
```

## Testing

```bash
# No automated tests - manual browser testing required
# Test scenarios:
# 1. Generate single formation (填入数字 button)
# 2. Batch export (批量生成 section)
# 3. All 9 direction rules
# 4. Probability parameters (same_lie, same_num)
# 5. Export copy functionality
# 6. Image display (img/ folder with 1-34 PNG files)
# 7. Responsive scaling (resize browser window)
```

## Code Style Guidelines

### General
- Single HTML file containing HTML, CSS, and JS
- Keep functions small and focused (single responsibility)
- Use descriptive variable names (5+ characters)
- Avoid magic numbers - use named constants
- Comment WHY, not WHAT

### JavaScript Conventions
- Variables/functions: `camelCase`
- Constants: `SCREAMING_SNAKE_CASE`
- Classes: `PascalCase` (if used)
- Use `const` over `let`, avoid `var`
- Prefer arrow functions for callbacks
- Use template literals for string interpolation
- No external imports or dependencies

### CSS Conventions
- Class names: `kebab-case`
- Use CSS custom properties for colors (if added)
- Keep selectors simple and specific
- Group related styles together
- Use flexbox and grid for layout
- Responsive design with `@media` queries

### HTML Conventions
- Use semantic elements (`section`, `main`, `button`)
- Include `lang` attribute on `<html>`
- Use `data-*` attributes for JavaScript hooks
- Include `type="text/javascript"` for inline scripts
- Input validation with `min`/`max` attributes

### Error Handling
- Always check DOM element existence before use: `if (element) {...}`
- Use try/catch for operations that may fail
- Log errors with context: `console.error('Failed to X:', error)`
- Never swallow errors silently
- Validate numeric inputs before use
- Show user-friendly error messages in UI

### Formatting
- 2 spaces for indentation
- Maximum line length: 100 characters
- Use semicolons
- One blank line between function definitions
- Spaces around operators: `x + y`, not `x+y`
- Break long strings across multiple lines

## Numbers & Colors

- Numbers 1-20 each have specific colors (see `getCellColor` function, line ~685)
- Number 0 represents empty cell (transparent)
- Board values: 0 (empty) or 1-20 (numbered cells)
- Images in `img/` folder map to numbers via `imageMapping` array
- Image size: 150×190px for cards

## Repository Structure

```
D:\Agent\LevelMake\
├── index.html      # Main application (HTML+CSS+JS)
├── img/            # Card images (1.png to 34.png)
├── AGENTS.md       # This file
└── .vscode/        # VSCode settings
    └── extensions.json
```

## Direction Rules (阵型方向)

| ID | Name | Fill Order | Type |
|----|------|------------|------|
| 0 | 无规则 | Random column order | Column |
| 1 | 向上 | Bottom to top columns | Column |
| 2 | 向下 | Top to bottom columns | Column |
| 3 | 向左 | Right to left rows | Row |
| 4 | 向右 | Left to right rows | Row |
| 5 | 上下向内 | Edges to center (columns) | Column |
| 6 | 左右向内 | Edges to center (rows) | Row |
| 7 | 上下向外 | Center to edges (columns) | Column |
| 8 | 左右向外 | Center to edges (rows) | Row |

## Key Functions

- `fillBoard()` - Main generation logic with UI updates (line ~712)
- `generateSingleLevel()` - Batch generation without UI (line ~1022)
- `getFillOrder()` - Direction-based cell order calculation (line ~821)
- `calculatePool()` - Calculate card pool from UI inputs (line ~563)
- `calculatePoolForGenerate()` - Initialize pool for generation (line ~1102)
- `batchGenerate()` - Batch export to TXT file (line ~1153)
- `getCellColor()` - Number-to-color mapping (line ~685)
- `shuffleArray()` - Fisher-Yates shuffle (line ~812)
- `scaleBoard()` - Responsive board scaling (line ~1186)

## State Variables

- `currentPool` - Array of `{number, count}` objects
- `filledCells` - 2D array tracking placed numbers
- `poolCards` - Flattened array of cards to draw
- `imageMapping` - Shuffled image indices
- `placedCounts` - Track how many of each number placed

## Common Issues

1. **Null references**: Always check `document.getElementById()` results
2. **Missing functions**: `shuffleArray`, `getPoolCards`, `getNextCard` required
3. **DOM order**: Use `nextElementSibling` for iteration
4. **File operations**: Batch export uses `Blob` and `URL.createObjectURL`
5. **Image paths**: Use `img/${num}.png` format
6. **Board scaling**: Wrap board in `.board-wrapper` for transform scaling

## Probability Parameters

- `same_lie` (0-1): Probability of same number within a column/row group
- `same_num` (0-1): Probability of same number as adjacent cell
- Row directions: [3, 4, 6, 8]
- Column directions: [0, 1, 2, 5, 7]

## Environment Variables

None required - no backend, no external dependencies.

## Testing Checklist

- [ ] Board renders with correct dimensions
- [ ] All number cards display with correct images
- [ ] Generate button produces valid output
- [ ] Single level copy to clipboard works
- [ ] Batch export downloads file correctly
- [ ] All 9 direction rules produce different orders
- [ ] Probability parameters affect output
- [ ] Input validation prevents invalid values
- [ ] Responsive scaling works on window resize
- [ ] Image mapping shuffles correctly
