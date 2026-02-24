# Changelog

All notable changes to openbrew will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-02-24

### Added
- Initial release
- Bean tracking with roaster, origin, process, roast level, roast date, price, weight, and varietals
- Brew logging with method, dose in/out, grind, water temp, brew time, and rating
- Interactive prompts for adding beans and brews
- `beans` command — list all beans with brew counts and average ratings
- `brews` command — list brews with filtering by bean, method, date range, and limit
- `stats` command — overall statistics with top beans, method breakdown, and brew frequency
- `timeline` command — visual timeline of recent brews
- `top` command — rank beans by average rating
- `search` command — full-text search across all entries
- `tags` command — list all tags with usage counts
- `methods` command — brew method breakdown
- `finish` command — mark beans as finished, hiding them from the brew picker
- `sync` command — generate active-beans.txt for iOS Shortcuts integration
- `import` command — import entries from another journal file
- `edit` command — open journal in $EDITOR
- Tags (with `+` prefix) and notes (with `;` prefix)
- Flexible field name aliases (e.g. `temp` = `water_temp`)
- Coloured terminal output (respects `NO_COLOR`)
- Configurable journal location via `$OPENBREW` environment variable
- Example journal with sample data
- MIT license
