# Changelog

## Repository retirement — 2026-10-09

### Changed

- Pin the CI toolchain to Go 1.26.9, which fixes the reachable standard-library
  vulnerabilities reported by govulncheck; retain the Go 1.26.6 language floor.
- Maintained development moved to [github.com/hollis-labs/libs/util/svcerr](https://github.com/hollis-labs/libs/tree/util%2Fv0.1.0/util/svcerr) in
  `github.com/hollis-labs/libs/util@v0.1.0` (`util/v0.1.0`).
- This standalone repository is retired after the replacement release was
  verified fetchable with successful module CI. README migration instructions
  identify the new import prefix; existing standalone tags and history are preserved.

All notable changes to go-svcerr are documented here. The format
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Write the entry for a release here BEFORE cutting its tag: the release workflow
refuses a tag whose CHANGELOG has no heading for it.

## v0.1.0 — 2026-09-29

### Added

- `Error` (status, code, safe message, field, internal cause) with `Unwrap` and a code-matching `Is`; `New`, `Wrap`, `WithField`, `WithStatus`.
- Six built-in codes (`invalid`, `not_found`, `conflict`, `permission`, `unavailable`, `internal`) with default HTTP statuses, `DefaultStatus`, and matching sentinels for `errors.Is`. `Code` is an open string type.
- `StatusFor` and `CodeFor`, which read the first `*Error` in a chain and never inspect error text.
- Optional `WriteJSON` and `Envelope` (`{"error":{"code","message","field"?}}`, Tether's nesting), in their own file. A non-`*Error` is written with the caller's fallback status and a generic body, never its own text.
- Hardening beyond the obvious shape: sentinels carry their code's status (a bare `&Error{}` literal would have yielded status 0), an unset or non-error status (anything outside 400..599, so a `WithStatus(200)` or a `204` fallback cannot report a failure as success) falls back to the code's default and then to 500 (so `WriteHeader` cannot panic), and an empty message falls back to the status text.
