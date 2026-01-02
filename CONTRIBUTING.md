# Contributing to Lumenaute Skills

Thank you for contributing! This guide will help you share your learned skills with the community.

## 🎯 The Mission

**Make local AI smarter through skill sharing.**

If you have a flat-rate frontier API subscription (Claude Pro, Gemini, etc.), you can learn unlimited skills at **zero marginal cost** and share them with everyone. This creates exponential network effects - the more we share, the smarter everyone's local models become.

## 📋 Requirements

### Before Submitting

- [ ] Skill successfully executes locally
- [ ] Quality score ≥ 0.7 (check with `lumenaute skills info <id>`)
- [ ] Clear, descriptive name
- [ ] Useful tags for discovery
- [ ] Hash verification passes

### Skill Naming Convention

Format: `{function}_{version}_{model}_{size}_{quant}_d{dimension}`

Examples:
- `kubernetes_debug_v1_llama_8b_4bit_d2048`
- `git_workflow_v2_qwen_1.5b_8bit_d1536`

For simpler community names, use: `{category}_{specific_task}`
- `devops_k8s_pod_debug`
- `dev_python_debug`
- `research_paper_search`

## 🚀 Contribution Process

### 1. Export Your Skill

```bash
# After Lumenaute learns something successfully
lumenaute skills export --id <skill_id> --identity "skill_name_v1_llama_8b_4bit_d2048"

# This creates: skill_name_v1_llama_8b_4bit_d2048.luma
```

### 2. Inspect the Skill

```bash
# Extract and review
tar -xzf skill_name_v1_llama_8b_4bit_d2048.luma
cat skill_name_v1_llama_8b_4bit_d2048/recipe.toml

# Verify quality metrics
lumenaute skills info skill_name_v1_llama_8b_4bit_d2048.luma
```

### 3. Fork and Add

```bash
# Fork the repo on GitHub
git clone https://github.com/YOUR_USERNAME/Lumenaute_Skills.git
cd Lumenaute_Skills

# Add your skill
cp path/to/your_skill.luma skills/

# Calculate hash
sha256sum skills/your_skill.luma
```

### 4. Update index.toml

Add an entry to `index.toml`:

```toml
[[skills]]
name = "your_skill_name"
description = "Brief description of what this solves"
tags = ["category", "specific", "keywords"]
author = "your_github_username"
version = "1.0.0"
created = "2026-01-02T12:00:00Z"
file = "skills/your_skill.luma"

oracle_model = "claude-sonnet-4"
target_model = "Llama-3.3-8B-Instruct-128K"
intent = "debugging"  # or "search", "implementation", etc.

quality = 0.92
efficiency = 0.88
success_rate = 0.89
usage_count = 10
downloads = 0

hash = "sha256:YOUR_HASH_HERE"
size_bytes = 45231

tools = ["bash", "kubectl"]
dependencies = []
```

### 5. Submit Pull Request

```bash
git add skills/your_skill.luma index.toml
git commit -m "Add: your_skill_name - Brief description"
git push origin main

# Create PR on GitHub with:
# Title: "Add: your_skill_name"
# Description: What problem it solves, how to use it
```

## 📐 Quality Standards

### Minimum Requirements

- **Quality Score**: ≥ 0.7
- **Success Rate**: ≥ 0.75 (if available)
- **Description**: Clear, concise (1-2 sentences)
- **Tags**: At least 2 relevant tags
- **Tools**: List all required tools

### Testing Checklist

Before submitting, verify:

```bash
# Import test
lumenaute skills import your_skill.luma

# Execution test
lumenaute generate -q "Query that should trigger this skill"

# Verify it executed (check logs)
lumenaute skills list  # Should show usage_count increment
```

### Red Flags (Will Be Rejected)

- ❌ Quality score < 0.7
- ❌ Missing required tools in index
- ❌ Hash mismatch
- ❌ Malicious code/commands
- ❌ Secrets/API keys in skill content
- ❌ Copyright violations

## 🏷️ Tagging Guidelines

Use specific, searchable tags:

**Good Tags:**
- `kubernetes`, `debugging`, `pods`
- `python`, `pytest`, `unit-testing`
- `git`, `merge-conflicts`, `workflow`

**Bad Tags:**
- `cool`, `useful`, `good` (too generic)
- `ai`, `ml` (too broad)
- Single-letter tags

## 📦 Skill Categories

Organize your skill into one of these categories:

- **devops**: Kubernetes, Docker, CI/CD, infrastructure
- **development**: Debugging, Git, testing, code analysis
- **research**: Literature search, paper analysis, data collection
- **sysadmin**: Logs, performance, monitoring, backups
- **security**: Vulnerability scanning, auditing, compliance
- **data**: ETL, processing, analysis, visualization

## 🔐 Security Review

Skills undergo automated security checks:

1. **No executable code** - Only data/traces allowed
2. **Tool whitelist** - Must use approved tools
3. **No secrets** - API keys, passwords, tokens forbidden
4. **Hash verification** - Content must match declared hash
5. **Size limits** - Max 10MB per skill

## 🎨 Skill Description Guidelines

Write clear, actionable descriptions:

**Good:**
> "Debug failed Kubernetes pods using kubectl describe and logs analysis"

**Bad:**
> "Kubernetes stuff"

**Good:**
> "Resolve Python import errors by checking sys.path and module structure"

**Bad:**
> "Fix Python"

## 📊 Metrics Explanation

### Quality (0-1)
How well does it solve the problem?
- 0.9-1.0: Excellent, handles edge cases
- 0.7-0.9: Good, solves common cases
- <0.7: Needs improvement

### Efficiency (0-1)
Speed and resource usage
- 0.9-1.0: Very fast, minimal resources
- 0.7-0.9: Reasonable performance
- <0.7: Slow or resource-heavy

### Success Rate (0-1)
Reliability across contexts
- 0.9-1.0: Almost always works
- 0.7-0.9: Usually works
- <0.7: Often fails

## 🤝 Community Guidelines

- Be respectful and constructive
- Credit sources and inspirations
- Help others improve their skills
- Report issues and bugs
- Share success stories

## 📜 License

By contributing, you agree to license your skills under **MIT License** unless explicitly stated otherwise in the skill's `recipe.toml`.

## ❓ Questions?

- Open an issue in this repo
- Join discussions
- Check the [main Lumenaute repo](https://github.com/campbellcharlie/Lumenaute_V2)

---

**Thank you for making local AI smarter! 🚀**
