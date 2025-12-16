# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Spring Boot application written in Kotlin, serving as a code generation laboratory. The project uses Gradle as its build system and includes Spring Security for authentication/authorization capabilities.

**Tech Stack:**
- Kotlin 2.2.21 with Java 21 toolchain
- Spring Boot 3.5.8
- Spring Security
- Spring Web
- Jackson for JSON serialization
- JUnit 5 for testing

**Package Structure:**
- Base package: `ai.forge.app.codegen_lab`
- Main application class: `CodegenLabApplication.kt`

## Build & Development Commands

### Building the Project
```bash
./gradlew build
```

### Running the Application
```bash
./gradlew bootRun
```

### Running Tests
```bash
# Run all tests
./gradlew test

# Run a specific test class
./gradlew test --tests "ai.forge.app.codegen_lab.CodegenLabApplicationTests"

# Run tests with continuous build
./gradlew test --continuous
```

### Cleaning Build Artifacts
```bash
./gradlew clean
```

## Architecture Notes

The application follows Spring Boot conventions with a standard Kotlin/Spring project structure:

- `src/main/kotlin/` - Application source code
- `src/main/resources/` - Application resources and configuration (application.yaml)
- `src/test/kotlin/` - Test source code

The project currently has Spring Security enabled as a dependency, which means all endpoints will require authentication by default unless explicitly configured otherwise.

## Kotlin-Specific Configuration

The project uses:
- JSR-305 strict mode (`-Xjsr305=strict`) for null-safety annotations
- Kotlin Spring plugin for proper Spring proxy support with Kotlin classes
- Kotlin reflection for Spring framework functionality

## Custom Cursor Commands

The project includes custom Cursor commands in `.cursor/commands/`:

- `/check-deps` - Checks if dependencies in build.gradle.kts are using the latest versions by querying:
  - Gradle plugins: `https://plugins.gradle.org/plugin/{PLUGIN_NAME}`
  - Dependencies: `https://mvnrepository.com/artifact/{PACKAGE_NAME}`
