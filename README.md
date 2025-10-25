# Artagon Workflows - Bazel CI Test Repository

This repository tests the [artagon-workflows](https://github.com/artagon/artagon-workflows) Bazel CI reusable workflow.

## Purpose

Validates that the `bazel_multi_ci.yml` reusable workflow:
- Builds Bazel projects correctly
- Runs tests and reports results
- Supports different Bazel versions
- Handles multiple configurations (release, debug)
- Works with custom build and test targets
- Uploads build artifacts
- Works with various configurations

## Test Matrix

| Test | Bazel Version | Configs | Custom Targets | Tests |
|------|---------------|---------|----------------|-------|
| Default | latest | - | - | ✓ |
| Latest | latest | - | - | ✓ |
| Version 7 | 7.x | - | - | ✓ |
| Custom Configs | latest | release, debug | - | ✓ |
| Custom Targets | latest | - | //src:hello //src:hello_test | ✓ |
| Skip Tests | latest | - | - | ✗ |

## Triggers

This workflow runs on:
- **Push to main** - Validates changes to this test repo
- **Pull requests** - Validates PRs before merge
- **Daily schedule** (2 AM UTC) - Catches breaking changes in artagon-workflows
- **Manual dispatch** - On-demand testing with custom Bazel version/config
- **Repository dispatch** - Triggered by artagon-workflows on releases/updates

## Project Structure

```
.
├── WORKSPACE               # Bazel workspace configuration
├── BUILD.bazel            # Bazel build configuration
├── src/
│   ├── BUILD.bazel        # Source-level build config
│   ├── hello.h            # Header file
│   ├── hello.cpp          # Implementation
│   ├── main.cpp           # Main program
│   └── hello_test.cpp     # Test file
└── .github/
    └── workflows/
        └── ci.yml         # Workflow testing configuration
```

## Test Project

This is a minimal Bazel project with:
- **Language**: C++
- **Build System**: Bazel
- **Targets**:
  - `//src:hello` - Library target
  - `//src:hello_main` - Binary target
  - `//src:hello_test` - Test target

## Running Locally

```bash
# Build all targets
bazel build //...

# Build specific target
bazel build //src:hello_main

# Run all tests
bazel test //...

# Run specific test
bazel test //src:hello_test

# Clean
bazel clean
```

## Bazel Configurations

This project supports multiple configurations that can be specified with `--config`:

```bash
# Build with release config
bazel build --config=release //...

# Build with debug config
bazel build --config=debug //...
```

## Related

- [artagon-workflows](https://github.com/artagon/artagon-workflows) - Main workflow repository
- [Bazel Multi CI Workflow](https://github.com/artagon/artagon-workflows/blob/main/.github/workflows/bazel_multi_ci.yml) - The workflow being tested
- [Testing Strategy](https://github.com/artagon/artagon-workflows/blob/main/.model-context/TESTING_STRATEGY.md) - Overall testing approach
