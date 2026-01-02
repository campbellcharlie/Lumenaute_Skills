# Lumenaute Skill Format Specification v1.0

## Overview

A `.luma` file is a portable, shareable representation of a learned capability that can be exported from one Lumenaute instance and imported into another.

## File Format

**Extension:** `.luma`
**Container:** Gzipped tarball (`.tar.gz` with `.luma` extension)
**Contents:** `recipe.toml` (skill metadata and content) + optional `embedding.json` (semantic vector)
**Encoding:** UTF-8
**Format:** TOML (easier for small models to generate than JSON)

## Why TOML?

Small language models struggle with JSON formatting (missing commas, trailing commas, incorrect escaping). TOML is more forgiving and easier to generate correctly, making it ideal for skills that may be created by local models learning from frontier oracles.

## Schema

### recipe.toml

```toml
format_version = "1.0"

[skill]
name = "kubernetes_debug"
description = "Debug failed Kubernetes pods and containers"
version = "1.0.0"
tags = ["kubernetes", "devops", "debugging"]
author = "community"
created = "2026-01-01T12:00:00Z"
license = "MIT"

[training]
oracle_model = "claude-sonnet-4"
target_model = "Llama-3.3-8B-Instruct-128K"
query = "How do I debug a failing Kubernetes pod?"
intent = "debugging"

[content]
output = """
To debug a failing pod:
1. kubectl describe pod <name>
2. kubectl logs <name>
3. Check events and resource limits
"""
tools = ["bash", "kubectl"]
reasoning_trace = """
[APPRENTICESHIP_PROTOCOL]
Student failed at Kubernetes debugging.
Oracle demonstrates systematic approach...
"""

[metadata]
quality = 0.92
efficiency = 0.88
usage_count = 147
success_rate = 0.89
hash = "sha256:abc123..."
exported_at = "2026-01-02T12:00:00Z"
```

### embedding.json (optional, stored separately)

```json
{
  "embedding": [0.123, 0.456, 0.789, ...]
}
```

**Note:** Embeddings are stored in a separate JSON file because TOML doesn't handle large arrays well (readability), and embeddings are typically 384-1024 floats.

## Field Descriptions

### Top-Level

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `format_version` | string | Yes | Skill format version (currently "1.0") |

### [skill]

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Unique skill identifier (lowercase, underscores) |
| `description` | string | Yes | Human-readable description |
| `version` | string | Yes | Semantic version (x.y.z) |
| `tags` | array[string] | No | Keywords for discovery |
| `author` | string | No | Creator (default: "community") |
| `created` | string | Yes | ISO 8601 timestamp |
| `license` | string | No | License identifier (default: "MIT") |

### [training]

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `oracle_model` | string | Yes | Frontier model used for training |
| `target_model` | string | Yes | Local model this skill was distilled for |
| `query` | string | Yes | Original query that created this skill |
| `intent` | string | Yes | Intent classification (debugging, search, etc.) |

### [content]

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `output` | string | Yes | The learned response/trace |
| `tools` | array[string] | No | Tools used in this skill |
| `reasoning_trace` | string | No | Full oracle reasoning trace |

### [metadata]

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `quality` | float | No | Quality score (0-1) |
| `efficiency` | float | No | Efficiency score (0-1) |
| `usage_count` | int | No | Times used by original author |
| `success_rate` | float | No | Success rate (0-1) |
| `hash` | string | No | Content hash (SHA-256) for verification |
| `exported_at` | string | No | Export timestamp (ISO 8601) |

### embedding.json (separate file)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `embedding` | array[float] | Yes | Semantic embedding vector (384-1024 dims) |

## Validation Rules

### On Import

1. **Format Version Check**
   - Must be "1.0" or compatible
   - Reject unknown versions

2. **Required Fields**
   - All required fields must be present
   - Reject if missing

3. **Size Limits**
   - Max file size: 10MB
   - Max embedding size: 1024 floats
   - Max output length: 100KB

4. **Name Validation**
   - Format: `^[a-z0-9_]+$`
   - Max length: 64 characters
   - No duplicates (or prompt to overwrite)

5. **Hash Verification** (if present)
   - Verify content matches hash
   - Warn if mismatch

## Example Usage

### Export

```bash
lumenaute skills export --id 42 --identity "k8s_debug_v1_llama_8b_4bit_d2048"
# Creates: k8s_debug_v1_llama_8b_4bit_d2048.luma
```

### Import

```bash
lumenaute skills import k8s_debug_v1_llama_8b_4bit_d2048.luma
# Validates and adds to local library
```

### Inspect

```bash
tar -xzf k8s_debug_v1_llama_8b_4bit_d2048.luma
cat k8s_debug_v1_llama_8b_4bit_d2048/recipe.toml
# Shows metadata without importing
```

## Compatibility

### Forward Compatibility
- Newer versions must support older formats
- Unknown fields should be ignored (not rejected)

### Backward Compatibility
- Breaking changes require major version bump
- v1.x must remain compatible with v1.0

## Security Considerations

1. **No Code Execution**
   - Skills contain data/traces, not executable code
   - Tools are references only (must exist in local installation)

2. **Path Traversal Protection**
   - Tarball extraction checks for ZipSlip attacks
   - All paths validated before extraction

3. **Trust Model**
   - Community skills are untrusted by default
   - Hash verification ensures integrity
   - User ratings/reports for quality/safety (future)

## Migration Path

### From Database to .luma

See `lumenaute/registry.py:export_skill()` for DB → .luma conversion logic.

## Future Extensions (v1.1+)

Potential additions without breaking v1.0:
- `skill.dependencies`: Other skills this requires
- `skill.categories`: Hierarchical categorization
- `content.examples`: Input/output example pairs
- `metadata.signature`: Cryptographic signature for verified skills
- `training.conversation`: Full multi-turn conversation context

## References

- TOML Spec: https://toml.io/
- Semantic Versioning: https://semver.org/
- ISO 8601 Timestamps: https://en.wikipedia.org/wiki/ISO_8601
