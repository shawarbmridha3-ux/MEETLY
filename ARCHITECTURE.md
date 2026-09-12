# MEETLY Architecture Overview

## System Architecture

This document provides an architectural overview of the MEETLY Flutter application and its CI/CD pipeline.

```mermaid
graph TB
    subgraph Development["Development Environment"]
        A["Source Code<br/>Flutter Project"]
        B["Dependencies<br/>pubspec.yaml"]
    end
    
    subgraph VCS["Version Control"]
        C["GitHub Repository<br/>shawarbmridha3-ux/MEETLY"]
    end
    
    subgraph CI["CI/CD Pipeline<br/>GitHub Actions"]
        D["Checkout Code"]
        E["Verify Project Archive<br/>MEETLY_COMPLETE_PROJECT.zip"]
        F["Extract & Locate<br/>pubspec.yaml"]
        G["Set up Flutter SDK<br/>v3.24.0"]
        H["Verify Installation<br/>flutter doctor"]
        I["Install Dependencies<br/>flutter pub get"]
        J["Build APK<br/>flutter build apk --release"]
        K["Verify APK Output"]
    end
    
    subgraph Artifacts["Build Artifacts"]
        L["APK Release<br/>app-release.apk"]
        M["Artifact Storage<br/>30-day retention"]
    end
    
    subgraph Deployment["Distribution"]
        N["GitHub Artifacts<br/>Download Section"]
    end
    
    A -->|Push| C
    C -->|Trigger Workflow| D
    D --> E
    E --> F
    F -->|Set PROJECT_DIR| G
    G --> H
    H --> I
    I -->|Working Directory<br/>PROJECT_DIR| J
    J --> K
    K -->|Success| L
    L --> M
    M --> N
    
    style Development fill:#e1f5ff
    style VCS fill:#f3e5f5
    style CI fill:#fff3e0
    style Artifacts fill:#e8f5e9
    style Deployment fill:#fce4ec
```

## Component Details

### Development Environment
- **Flutter Project**: Mobile application source code written in Dart
- **Dependencies Management**: Managed via `pubspec.yaml` configuration file

### Version Control
- **GitHub Repository**: `shawarbmridha3-ux/MEETLY`
- **Project Archive**: `MEETLY_COMPLETE_PROJECT.zip` containing the complete Flutter project

### CI/CD Pipeline (GitHub Actions)
The build workflow follows these steps:

1. **Code Checkout** - Retrieves the latest code from the repository
2. **Archive Verification** - Ensures the project archive exists before proceeding
3. **Project Extraction** - Unzips the archive and locates the Flutter project structure
4. **Flutter Setup** - Installs Flutter SDK v3.24.0 on the Ubuntu runner
5. **Installation Verification** - Runs `flutter doctor` to validate the environment
6. **Dependency Installation** - Executes `flutter pub get` to fetch all dependencies
7. **APK Build** - Compiles the app into a release APK using `flutter build apk --release`
8. **Output Verification** - Validates that the APK was generated at the expected location
9. **Artifact Upload** - Stores the APK with 30-day retention

### Build Artifacts
- **APK File**: Release-optimized Android application package
- **Storage**: GitHub Artifacts with 30-day retention policy

### Distribution
- **Access Point**: GitHub Artifacts section of the workflow run
- **Format**: Standard Android APK ready for installation

## Build Workflow Trigger
- **Type**: Manual workflow dispatch (`workflow_dispatch`)
- **Platform**: Ubuntu Latest (`ubuntu-latest`)
- **Language**: Dart/Flutter

## Error Handling
The workflow includes comprehensive error checks at each stage:
- Archive existence validation
- `pubspec.yaml` discovery verification
- Dependency installation status checks
- APK build success validation
- APK output path verification

Each step includes detailed logging with success (✓) and failure (❌) indicators for easy troubleshooting.
