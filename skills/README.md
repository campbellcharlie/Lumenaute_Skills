# Skills Directory

This directory contains all community-shared `.luma` skill files.

## Structure

Each skill is a single `.luma` file (gzipped tarball) containing:
- `recipe.toml` - Skill metadata and learned content
- `embedding.json` - Semantic vector (optional)

## Naming Convention

Use descriptive names that indicate the skill's purpose:
- `kubernetes_pod_debug.luma`
- `python_pytest_runner.luma`
- `git_merge_conflict_resolver.luma`

## Adding Skills

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full process.

Quick checklist:
1. Export skill from Lumenaute
2. Add `.luma` file to this directory
3. Update `../index.toml` with metadata
4. Submit pull request

## Verification

All skills are automatically validated on PR:
- TOML format check
- File size limits (10MB max)
- Required fields present
- No duplicates
- Tarball structure valid

---

**Current skill count**: Check [index.toml](../index.toml)
