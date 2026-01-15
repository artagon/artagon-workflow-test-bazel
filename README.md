# Artagon Workflow Test - Bazel

[![CI](https://github.com/artagon/artagon-workflow-test-bazel/actions/workflows/ci.yml/badge.svg)](https://github.com/artagon/artagon-workflow-test-bazel/actions/workflows/ci.yml)

Test repository for validating [artagon-workflows](https://github.com/artagon/artagon-workflows) Bazel CI reusable workflow.

## Purpose

Validates that `bazel_multi_ci.yml` reusable workflow:
- Builds Bazel projects correctly
- Runs tests and reports results
- Supports different Bazel versions (latest, 7.x)
- Handles multiple configurations (release, debug)
- Works with custom build targets
- Generates code coverage

## Project Structure

```
.
├── .bazelrc              # Bazel configuration (release, debug, coverage)
├── BUILD.bazel           # Root BUILD file with all targets
├── WORKSPACE             # Workspace marker
├── src/
│   ├── greeter.h         # Library header
│   ├── greeter.cc        # Library implementation
│   ├── main.cc           # Main program
│   └── greeter_test.cc   # Unit test
└── .github/workflows/
    └── ci.yml            # CI configuration
```

## Targets

| Target | Type | Description |
|--------|------|-------------|
| `//:greeter` | `cc_library` | Greeter library |
| `//:hello` | `cc_binary` | Main executable |
| `//:greeter_test` | `cc_test` | Unit tests |

## Test Matrix

| Test | Bazel Version | Configs | Custom Targets |
|------|---------------|---------|----------------|
| Default | latest | release, debug | `//...` |
| Latest | latest | release, debug | `//...` |
| Version 7 | 7.x | release, debug | `//...` |
| Custom Configs | latest | release, debug | `//...` |
| Custom Targets | latest | release, debug | `//:hello //:greeter_test` |
| No Coverage | latest | release, debug | `//...` |

## Triggers

- **Push to main** - Validates changes
- **Pull requests** - Pre-merge validation
- **Daily schedule** (2 AM UTC) - Catch upstream breaking changes
- **Manual dispatch** - On-demand testing
- **Repository dispatch** - Triggered by [trigger_test_repos.yml](https://github.com/artagon/artagon-workflows/blob/main/.github/workflows/trigger_test_repos.yml)

## Running Locally

```bash
# Build all
bazel build //...

# Test all
bazel test //...

# Build with config
bazel build --config=release //...
bazel build --config=debug //...

# Coverage
bazel coverage --config=coverage //...
```

## Related

- [artagon-workflows](https://github.com/artagon/artagon-workflows) - Main workflow repository
- [bazel_multi_ci.yml](https://github.com/artagon/artagon-workflows/blob/main/.github/workflows/bazel_multi_ci.yml) - Workflow being tested
- [Testing Guide](https://github.com/artagon/artagon-workflows/blob/main/docs/TESTING.md)
