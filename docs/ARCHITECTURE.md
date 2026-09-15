# Architecture

## Overview

`ref-brusher` is a PySide6 desktop application that converts loosely formatted references into structured citation records and renders GB/T 7714-2015 output.

## Request flow

```text
Pasted references
  -> RefFormatterController
  -> background WorkerThread
  -> Orchestrator
  -> metadata providers
  -> ReferenceVerifier
  -> formatter
  -> reviewed desktop result
```

## Packages

- `views/`: the main window and user-facing widgets.
- `ui_framework/`: reusable window, dialog, chart, splash, and style primitives.
- `workers/`: background query execution so network calls do not freeze the UI.
- `services/orchestrator.py`: provider selection and result coordination.
- `services/api_engines/`: Crossref, OpenAlex, Semantic Scholar, DBLP, and CNKI adapters.
- `services/formatter.py`: name normalization and GB/T 7714-2015 rendering.
- `models/`: citation domain objects.
- `core/verifier.py`: candidate validation and confidence checks.
- `logic/`: Chinese-literature search support.

## External boundaries

Provider responses are untrusted network data. Engines should keep timeouts, rate limits, and graceful fallbacks. No API key is committed; optional keys must remain in local configuration or environment variables.
