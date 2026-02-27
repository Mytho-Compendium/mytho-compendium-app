# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Android encyclopedia app for world mythologies, built with Clean Architecture and a modern Kotlin stack. Designed to evolve from a single-module app to a full Kotlin Multiplatform (KMP) project targeting Android and iOS.

## Build Commands

```bash
./gradlew assembleMvvmDebug      # Build app with MVVM pattern (default)
./gradlew assembleMviDebug       # Build app with MVI pattern
./gradlew test                   # Run all unit tests
./gradlew testMvvmDebugUnitTest  # Run unit tests for a specific variant
./gradlew lint                   # Run Android Lint
./gradlew detekt                 # Run Detekt static analysis
./gradlew ktlintCheck            # Check Kotlin formatting
./gradlew ktlintFormat           # Auto-fix Kotlin formatting
```

> A `google-services.json` file is required to build. Generate one from the Firebase console and place it in `app/`. It is never committed to the repository.

## Architecture

**Clean Architecture** with strict unidirectional dependency flow:

```
ui/             → Compose screens and components
presentation/   → ViewModels, UI state holders
domain/         → Use cases, domain models, repository interfaces
data/           → Repository implementations, data sources, mappers
```

**MVVM** is the default and production pattern. **MVI** is implemented in parallel via the `architecturePattern` build variant dimension (`mvvm` / `mvi`) as a learning reference — both variants live in the same codebase.

## Module Evolution

The project progresses through three structural phases (tracked via atomic, educative commits):

- **Phase 1 (current)**: Single `app/` module — all layers coexist, focus on clean layering
- **Phase 2**: Multi-module split into `app/`, `core/` (core-ui, core-domain, core-data), and `features/` (feature-mythology, feature-entity, feature-search)
- **Phase 3**: KMP migration — `shared/` module with `commonMain`/`androidMain`/`iosMain`, plus `androidApp/` and `iosApp/` entry points (migration happens in-place, no new repo)

## Tech Stack

| Layer | Library |
|:---|:---|
| UI | Jetpack Compose |
| DI | Hilt |
| Navigation | Jetpack Navigation Compose |
| Local DB | Room |
| Networking | Retrofit + kotlinx.serialization |
| Async | Kotlin Coroutines + Flow |
| Image loading | Coil |
| Logging | Timber |
| Testing | MockK + Truth |
| Static analysis | Detekt + ktlint + Android Lint |
| Monitoring | Firebase Crashlytics + Analytics |

## Code Quality

All checks run automatically in CI on every PR. Configuration lives in `/config/`:

- `config/detekt/detekt.yml` — Kotlin static analysis rules
- `config/ktlint/.editorconfig` — Kotlin formatting rules
- `config/lint/lint.xml` — Android Lint rules

## CI/CD

`.github/workflows/pr-checks.yml` runs lint, detekt, ktlint, and unit tests on every PR.
`.github/workflows/merge-main.yml` runs a full build plus instrumented tests on merge to `main`.

## Branches

`main` always reflects the default production stack. `learning/` branches are **permanent, never-merged** references that each replace one default library with an alternative (e.g., `learning/di-koin` replaces Hilt with Koin, `learning/db-sqldelight` replaces Room with SQLDelight). Do not merge these into `main`.

## Related Repositories

- **mytho-compendium-server** — Ktor backend, Docker, databases
- **mytho-compendium-content** — Content pipeline, Claude Code skills, data ingestion