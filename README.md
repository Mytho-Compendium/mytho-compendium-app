# mytho-compendium-app

> Android encyclopedia app for **Mytho Compendium** — built with Clean Architecture, a modern Kotlin stack, and designed to evolve from a single-module app to a full Kotlin Multiplatform project targeting Android and iOS.

## Project Overview

This repository contains the entire mobile codebase for Mytho Compendium, a digital encyclopedia of world mythologies. The app starts as a pure Android application and progressively evolves through well-defined phases, each tracked via atomic, educative commits that reflect real-world best practices.

The app will eventually become a **Kotlin Multiplatform (KMP)** project. The migration happens in-place inside this repository, no new repo will be created. The Gradle module structure will be reorganized to accommodate shared KMP modules and an iOS target when the time comes.

## Tech Stack

| Layer | Technology                                             |
|:---|:-------------------------------------------------------|
| Language | Kotlin                                                 |
| UI | Jetpack Compose                                        |
| Architecture | Clean Architecture (Domain / Data / Presentation / UI) |
| Async | Kotlin Coroutines + Flow                               |
| DI | Hilt                                                   |
| Navigation | Jetpack Navigation Compose                             |
| Local DB | Room                                                   |
| Networking | Retrofit + kotlinx.serialization                       |
| Image loading | Coil                                                   |
| Logging | Timber                                                 |
| Static analysis | Detekt + Lint + ktlint                                 |
| CI | GitHub Actions                                         |
| Crash reporting | Firebase Crashlytics                                   |
| Analytics | Firebase Analytics                                     |
| Multiplatform _(future)_ | Kotlin Multiplatform (KMP)                             |
| iOS UI _(future)_ | SwiftUI                                                |

## Architecture

The project follows **Clean Architecture** with a strict separation of concerns across three layers:

```text
ui/             → Compose UI
presentation/   → ViewModels, UI state
domain/         → Use cases, domain models, repository interfaces
data/           → Repository implementations, data sources, mappers
```

**MVVM** is the default and primary architecture pattern. **MVI** is implemented alongside it via build variants, serving as a practical learning reference and direct comparison between the two patterns.

## Module Structure

The project evolves through three distinct structural phases:

**Phase 1 - Single module**

```text
app/
```

One Gradle module. All layers coexist. Focus is on clean code, correct layering, and strict linting rules before any modularization.

**Phase 2 - Multi-module**

```text
app/                        # Entry point, DI wiring, navigation host
core/
  ├── core-ui/              # Shared Compose components, theme, typography
  ├── core-domain/          # Shared domain interfaces and base use cases
  └── core-data/            # Shared data infrastructure, network client
features/
  ├── feature-mythology/    # Mythology list and detail
  ├── feature-entity/       # Gods, creatures, artifacts, etc.
  └── feature-search/       # Cross-mythology search
```

**Phase 3 - KMP migration**

```text
shared/
  ├── commonMain/           # Shared business logic, domain, data
  ├── androidMain/          # Android-specific implementations
  └── iosMain/              # iOS-specific implementations
androidApp/                 # Android entry point
iosApp/                     # iOS entry point (Xcode project)
```

## Build Variants

| Dimension             | Variants                  |
|:----------------------|:--------------------------|
| `architecturePattern` | `mvvm` _(default)_, `mvi` |

Example build targets: `mvvmDebug`, `mvvmRelease`, `mviDebug`

This setup allows switching between architecture patterns at build time without modifying source code, making this project a practical learning reference for both patterns.

## Code Quality

This project enforces strict static analysis rules from day one. All checks run automatically in CI on every PR.

- **Detekt** - Kotlin static analysis, complexity rules, code smells
- **ktlint** - Kotlin code formatting
- **Android Lint** - Android-specific best practices

Configuration files live in `/config/`:

```text
config/
  ├── detekt/detekt.yml
  ├── ktlint/.editorconfig
  └── lint/lint.xml
```


## CI/CD

GitHub Actions pipelines live in `.github/workflows/`:

```text
.github/workflows/
  ├── pr-checks.yml     # Runs on every PR: lint, detekt, ktlint, unit tests
  └── merge-main.yml    # Runs on merge to main: full build + instrumented tests
```


## Firebase Setup

Firebase is configured for both **Crashlytics** (crash reporting) and **Analytics** (usage tracking). Configuration files (`google-services.json`) are **not committed** to the repository. They are injected via GitHub Actions secrets during CI builds.

> ⚠️ A `google-services.json` file is required to build. Generate one from the Firebase console and place it in the `app/` directory for local development.


## Getting Started

```bash
# Clone the repository
git clone https://github.com/your-username/mytho-compendium-app.git

# Open in Android Studio (Hedgehog or later recommended)
# Sync Gradle, build the project

# Run with MVVM (no server needed)
./gradlew assembleMvvmDebug
```

## Learning Branches

The `main` branch always reflects the default production stack. Dedicated `learning/` branches exist to explore and compare alternative libraries and approaches, each one is a complete, working migration from the default with educative commits explaining the differences and trade-offs.

| Branch                          | Default               | Alternative           |
|:--------------------------------|:----------------------|:----------------------|
| `learning/di-koin`              | Hilt                  | Koin                  |
| `learning/di-kodein`            | Hilt                  | Kodein                |
| `learning/db-realm`             | Room                  | Realm                 |
| `learning/db-sqldelight`        | Room                  | SQLDelight            |
| `learning/db-objectbox`         | Room                  | ObjectBox             |
| `learning/api-ktor-client`      | Retrofit              | Ktor Client           |
| `learning/api-okhttp-only`      | Retrofit              | OkHttp only           |
| `learning/api-graphql-apollo`   | REST / Retrofit       | GraphQL + Apollo      |
| `learning/api-graphql-ktor`     | REST / Retrofit       | GraphQL + Ktor Client |
| `learning/serialization-gson`   | kotlinx.serialization | GSON                  |
| `learning/serialization-moshi`  | kotlinx.serialization | Moshi                 |
| `learning/serialization-klaxon` | kotlinx.serialization | Klaxon                |
| `learning/image-glide`          | Coil                  | Glide                 |
| `learning/image-picasso`        | Coil                  | Picasso               |
| `learning/test-mockito`         | MockK                 | Mockito               |
| `learning/test-assertj`         | Truth                 | AssertJ               |
| `learning/test-strikt`          | Truth                 | Strikt                |
| `learning/test-robolectric`     | AndroidX Test         | Robolectric           |

> These branches are not merged into `main`. They are permanent references, each representing a standalone learning exercise and direct comparison with the default stack.


## Related Repositories

| Repository                             | Purpose                                              |
|:---------------------------------------|:-----------------------------------------------------|
| **mytho-compendium-app** _(this repo)_ | Android app, future KMP                              |
| **mytho-compendium-server**            | Ktor backend, Docker, databases                      |
| **mytho-compendium-content**           | Content pipeline, Claude Code skills, data ingestion |