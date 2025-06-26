# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the **Visual Studio Code (Code - OSS)** repository - the complete source code for Microsoft's open-source code editor. The codebase is a sophisticated TypeScript/JavaScript application built on Electron, featuring a modular architecture with platform services, the Monaco Editor, and an extensive extension system.

## Development Commands

### Core Build & Development
```bash
# Full compilation (TypeScript + extensions)
npm run compile                    # Full build
npm run watch                      # Watch mode for development
npm run watch-client               # Watch core client code only
npm run watch-extensions           # Watch extensions only

# Development servers with hot reload
npm run watchd                     # Watch with deemon daemon
npm run kill-watchd                # Stop watch daemon
npm run restart-watchd             # Restart watch daemon

# Launch development instance
./scripts/code.sh                  # Launch dev VSCode (Linux/Mac)
./scripts/code.bat                 # Launch dev VSCode (Windows)
```

### Testing Framework
```bash
# Unit tests
npm run test-node                  # Node.js unit tests
npm run test-browser               # Browser-based tests
npm run test-browser-no-install    # Skip Playwright install

# Integration & smoke tests
npm run smoketest                  # Full end-to-end tests
npm run smoketest-no-compile       # Skip compilation step

# Specific test patterns
npm run test-node -- --grep "pattern"  # Run specific tests
```

### CLI Development (Rust)
```bash
# CLI development
npm run compile-cli                # Build Rust CLI
npm run watch-cli                  # Watch CLI changes
```

### Web Development
```bash
# Web version development
npm run compile-web                # Build web version
npm run watch-web                  # Watch web changes
./scripts/code-web.sh              # Launch web server
./scripts/code-server.sh           # Launch code server
```

### Code Quality & Validation
```bash
# Linting and formatting
npm run eslint                     # ESLint checks
npm run stylelint                  # CSS/SCSS linting

# TypeScript validation
npm run monaco-compile-check       # Monaco editor types
npm run vscode-dts-compile-check   # VSCode API types
npm run valid-layers-check         # Architecture validation
npm run tsec-compile-check         # Security type checking

# Code hygiene
npm run hygiene                    # Code style validation
npm run precommit                  # Pre-commit hooks
```

### Extension Development
```bash
# Extension building
npm run watch-extensions           # Watch all extensions
npm run compile-extensions-build   # Production extension build

# Built-in extension management
npm run download-builtin-extensions    # Download extensions
npm run update-grammars               # Update syntax grammars
```

## Architecture Overview

### Core Source Structure (`/src/vs/`)
- **`base/`** - Foundation utilities, cross-platform abstractions
- **`platform/`** - Platform services (files, configuration, lifecycle)
- **`editor/`** - Monaco Editor engine (text editing, syntax highlighting)
- **`workbench/`** - Main application UI (panels, views, commands)
- **`code/`** - Application entry points and platform-specific code
- **`server/`** - Remote development server components

### Key Components
- **Monaco Editor**: Standalone text editor that powers the editing experience
- **Extension Host**: Isolated process for running extensions safely
- **Workbench Services**: Dependency injection system for platform services
- **Language Server Protocol**: Integration with language services
- **Terminal Integration**: Built-in terminal with xterm.js

### Extension System (`/extensions/`)
Built-in extensions provide core functionality:
- Language support (TypeScript, Python, etc.)
- Git integration and SCM
- Debug adapters
- Themes and syntax highlighting
- Markdown support

Each extension has its own TypeScript compilation and webpack bundling.

### Build System Architecture
- **Gulp**: Primary build orchestration
- **TypeScript**: Main compilation with multiple tsconfig files
- **Webpack**: Extension bundling and optimization
- **Electron**: Desktop application packaging
- **SWC**: Fast TypeScript compilation for development

### Testing Strategy
- **Unit Tests**: Mocha-based tests for individual components
- **Integration Tests**: Cross-component functionality testing
- **Smoke Tests**: End-to-end user workflow validation
- **Automation Tests**: Playwright-driven UI automation
- **Monaco Tests**: Dedicated editor engine testing

## Development Workflow

### First Time Setup
1. Clone repository and install dependencies: `npm install`
2. Build everything: `npm run compile`
3. Launch development instance: `./scripts/code.sh`

### Daily Development
1. Start watch mode: `npm run watch` or `npm run watchd`
2. Make changes to TypeScript/JavaScript files
3. Changes are automatically recompiled
4. Reload VSCode instance to see changes

### Extension Development
1. Navigate to specific extension: `cd extensions/[extension-name]`
2. Start extension watch: `npm run watch-extensions`
3. Use F5 in VSCode to launch Extension Development Host

### Testing Changes
1. Run relevant test suite: `npm run test-node` or `npm run test-browser`
2. For UI changes: `npm run smoketest`
3. Validate with: `npm run hygiene && npm run eslint`

## Platform-Specific Notes

### Electron Integration
- Uses Electron 35.5.1 for desktop application
- Main process vs renderer process architecture
- Native module integration (node-pty, ripgrep, etc.)

### Web Version Support
- Builds can run in browsers without Electron
- Separate compilation pipeline for web assets
- Service worker integration for offline functionality

### Remote Development
- Code server can run remotely with local client
- SSH, container, and WSL integration
- Extension host can run remotely

## Performance Considerations

### Memory Management
- Uses `--max-old-space-size=8192` for Node.js builds
- Lazy loading of extensions and services
- Web workers for heavy computations

### Build Optimization
- Incremental TypeScript compilation
- Webpack code splitting for extensions
- Source map generation for debugging

## Key Dependencies

### Runtime Dependencies
- **Electron 35.5.1**: Desktop application framework
- **xterm.js**: Terminal emulator component
- **Monaco Editor**: Core text editing engine
- **Tree-sitter**: Syntax highlighting parser
- **Ripgrep**: Fast text search utility

### Development Dependencies
- **TypeScript 5.9+**: Primary language
- **Playwright**: Browser automation for testing
- **Mocha**: Test framework
- **ESLint**: Code linting
- **Gulp**: Build task orchestration

## Extension API

### Core APIs
Extensions interact with VSCode through well-defined APIs:
- `vscode.window`: UI interactions
- `vscode.workspace`: File system access
- `vscode.commands`: Command registration
- `vscode.languages`: Language feature providers

### Extension Lifecycle
1. Extension activation based on `activationEvents`
2. Main function called with extension context
3. Services registered through dependency injection
4. Cleanup on deactivation

This codebase represents one of the largest and most sophisticated open-source editors, with careful attention to performance, extensibility, and cross-platform compatibility.