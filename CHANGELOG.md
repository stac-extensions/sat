# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Added `orbit_cycle` field ([#6](https://github.com/stac-extensions/sat/issues/6))
- Added `sat:anx_start_offset` and `sat:anx_end_offset` fields

### Changed

### Deprecated

### Removed

### Fixed

- JSON Schema checks `stac_extensions` field in Collections

## [v1.0.0] - 2021-03-04

### Added

- Added additional acquisition parameters ([#894](https://github.com/radiantearth/stac-spec/pull/894))
  - `sat:platform_international_designator`
  - `sat:absolute_orbit`
  - `sat:anx_datetime`

[Unreleased]: <https://github.com/stac-extensions/sat/compare/v1.0.0...HEAD>
[v1.0.0]: <https://github.com/stac-extensions/sat/tree/v1.0.0>
