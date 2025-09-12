# 🔹 Architecture / Concept

## SBOM-centric approach

### Syft يولد SBOM
### Grype يستخدم SBOM للتحقق من vulnerabilities

## JS-specific layers

### AuditJS و Retire.js يغطوا Node.js و JS client libraries.

## System / Docker layer
### Trivy يفحص كل الملفات fs
### Trivy يفحص Docker image (لو مفعل).

## Artifact storage
## Scan report محفوظة للتحليل - compliance.

## Fail-on-policy
### HIGH/CRITICAL vulnerability يوقف workflow


# Security Scan Workflow

```mermaid
flowchart TD
    A[GitHub Actions Trigger] --> B[Checkout repository]
    B --> C[Node.js Setup]
    C --> D[Install Dependencies]

    %% SBOM Layer
    D --> E[Syft: Generate SBOM]
    E --> F[Grype: Scan SBOM]
    F --> G[Upload Grype Report]

    %% JS-specific Layer
    D --> H[Retire.js Scan]
    H --> I[Upload Retire.js Report]
    D --> J[AuditJS Scan]
    J --> K[AuditJS Report Stored]

    %% Filesystem & Docker Layer
    D --> L[Trivy Scan Filesystem]
    L --> M[Upload Trivy FS Report]
    D --> N[Build Docker Image]
    N --> O[Trivy Scan Docker Image]
    O --> P[Upload Trivy Image Report]

    %% Artifacts Layer
    E --> Q[Upload SBOM]

    %% Fail Policies
    F -->|HIGH vulnerability| X[Workflow FAIL]
    H -->|HIGH vulnerability| X
    J -->|HIGH vulnerability| X
    L -->|HIGH/CRITICAL vulnerability| X
    O -->|HIGH/CRITICAL vulnerability| X
