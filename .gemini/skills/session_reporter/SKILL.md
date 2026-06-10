---
name: session_reporter
description: Agent for generating high-quality reports (Markdown/HTML/PDF) and managing agent handoffs.
---
<!-- APAF Metadata
last_verified_date: 2026-06-10
apaf_approved: true
apaf_version: 1.0.0
apaf_org: APAF Bioinformatics
-->

# Reporter Agent Skill

## Purpose
To generate reproducible, publication-quality reports (HTML/PDF/Markdown) and standardize communication between agents via Handoff Reports.

## Core Capabilities

### 1. Document Generation
-   **Markdown (`.md`)**: Standard for documentation and simple reports.
-   **Structured Templates**: Support for parameterized templates in JSON, HTML, or Markdown.
-   **Rendering**: Methods to compile templates to HTML/PDF or plain text.

### 2. Handoff Protocol
Standardized communication for agent-to-agent workflows.
-   **Executive Summary**: High-level status.
-   **Artifacts**: Absolute paths to outputs.
-   **Next Steps**: Recommendations for the Orchestrator.

## Function Signatures

### `render_report(input, output_file = NULL, params = list(), ...)`
Renders an input template to the specified output format.
-   **Input**: Template file path.
-   **Output**: Rendered file path (or side effect).

### `create_report_skeleton(type = "analysis", file)`
Creates a new report file with standard headers and structure.
-   **Type**: "analysis" (default), "handoff".

### `log_handoff(summary, artifacts, next_steps, file = "handoff.md")`
Appends a structured handoff block to a tracking file.

## Usage Patterns

### A. Rendering a Report
```javascript
render_report("analysis/01_clustering.md", {
    params: { k: 5, method: "generated" }
});
```

### B. Creating a New Analysis
```javascript
create_report_skeleton("analysis", "analysis/02_network.md");
```

### C. Logging a Handoff
```javascript
log_handoff({
    summary: "Completed clustering with k=5.",
    artifacts: ["/path/to/clusters.csv", "/path/to/plot.pdf"],
    next_steps: "Proceed to network analysis."
});
```

## Best Practices
1.  **Parameterized Reports**: Use a dynamic params dictionary or frontmatter to make reports reusable.
2.  **Clean Environments**: Always render in isolated contexts to ensure reproducibility.
3.  **Handoffs**: Be explicit about artifact paths so the next agent can find them.

<!-- APAF Bioinformatics | SKILL.md | Approved | 2026-06-10 -->
