---
layout: default
title: Changelog
nav_order: 10
---

# Changelog

All notable changes to the Entry Web SDK will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.13-beta.0] - 2026-02-02

### Added

- Comprehensive documentation site at [entry-web-sdk-docs](https://synapser-limited.github.io/entry-web-sdk-docs/)
- Troubleshooting guide with common issues and solutions
- Quick Reference page with copy-paste code snippets
- Error codes lookup table and handling patterns
- Next Steps section with resource links

### Changed

- Restructured package README for clarity - now links to comprehensive docs site
- Expanded SDK Reference with all public methods and parameters:
  - `getInstance(appName, environment, options?)` with debug logging support
  - `reset()` for testing and app switching
  - `deleteUser(userId)` for permanent deletion
  - `identifyUsersFromPhoto(base64Photo)` for photo-based identification
- Enhanced Integration Guide with callouts and improved Quick Links
- Added callouts (note, warning, important, highlight) throughout documentation
- Updated _config.yml with search enhancements and auxiliary links
- Improved navigation structure with proper nav_order values

### Fixed

- Broken documentation links in INTEGRATION.md (now using lowercase, no extensions)

---

## [1.0.11] - 2026-01-30

- Stable production ready release.

---

## Version Categories

- **Added** - New features
- **Changed** - Changes in existing functionality  
- **Deprecated** - Soon-to-be removed features
- **Removed** - Removed features
- **Fixed** - Bug fixes
- **Security** - Vulnerability fixes
