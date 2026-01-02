# Lumenaute Skills

**Community-shared skills for Lumenaute**

This repository contains portable `.luma` skills that can be imported into any Lumenaute instance. Skills are learned capabilities distilled from frontier models (Claude, GPT-4, Gemini) and packaged for local execution.

## 🎯 What are Lumenaute Skills?

Skills are **portable knowledge bundles** that capture:
- Oracle reasoning traces (how frontier models solve problems)
- Tool usage patterns
- Context-specific solutions
- Semantic embeddings for smart matching

Once installed, your local Lumenaute model can execute these skills **without calling the frontier API** - making them:
- ⚡ **Fast** - Local execution only
- 💰 **Free** - Zero API costs after learning
- 🔒 **Private** - No data leaves your machine

## 📦 Skill Format

Skills use the `.luma` format:
- **Container**: Gzipped tarball
- **Format**: TOML (easy for small models to generate)
- **Contents**:
  - `recipe.toml` - Skill metadata and learned trace
  - `embedding.json` - Semantic vector (optional)

See [SKILL_FORMAT_SPEC.md](SKILL_FORMAT_SPEC.md) for full specification.

## 🚀 Quick Start

### Install a Skill

```bash
# Search for skills
lumenaute skills search kubernetes

# Install from community
lumenaute skills install kubernetes_debug

# List installed skills
lumenaute skills list
```

### Export Your Own Skill

```bash
# After Lumenaute learns something new, export it
lumenaute skills export --id 42 --identity "my_skill_v1_llama_8b_4bit_d2048"

# This creates: my_skill_v1_llama_8b_4bit_d2048.luma
```

### Share with Community

1. Export your skill (see above)
2. Fork this repo
3. Add your `.luma` file to `skills/`
4. Update `index.toml` with metadata
5. Submit a pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📚 Available Skills

<!-- This section is auto-generated from index.toml -->

Browse the [skills directory](skills/) or check [index.toml](index.toml) for the full catalog.

### Categories

- **DevOps** - Kubernetes, Docker, CI/CD
- **Development** - Debugging, Git workflows, testing
- **Research** - Literature search, paper analysis
- **System Admin** - Log analysis, performance tuning

## 🏗️ Skill Quality

Each skill includes quality metrics:
- **Quality Score** (0-1) - How well it solves the problem
- **Efficiency Score** (0-1) - Speed and resource usage
- **Success Rate** (0-1) - Reliability across different contexts
- **Usage Count** - Times successfully executed

## 🔐 Security

### Trust Model

- ⚠️ **Community skills are untrusted by default**
- Skills contain **data/traces only** (no executable code)
- Tools referenced must exist in your local installation
- Hash verification ensures integrity

### Before Installing

1. Review the skill's `recipe.toml`
2. Check the oracle model and training data
3. Verify the hash matches
4. Inspect tool requirements

## 🤝 Contributing

We welcome contributions! Here's how the economics work:

### The Arbitrage

If you have a flat-rate frontier API subscription (Claude Pro $20/mo, Gemini free, etc.), you can:
1. Learn unlimited skills at **zero marginal cost**
2. Share them with the community
3. Everyone benefits from your API subscription

This creates **exponential network effects** - the more users share, the smarter everyone's local models become, for free.

### How to Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Skill naming conventions
- Quality requirements
- Testing guidelines
- Pull request process

## 📖 Documentation

- [Skill Format Specification](SKILL_FORMAT_SPEC.md)
- [Contributing Guide](CONTRIBUTING.md)
- [Lumenaute Main Repo](https://github.com/campbellcharlie/Lumenaute_V2)

## 📊 Statistics

- **Total Skills**: [auto-updated]
- **Total Downloads**: [auto-updated]
- **Contributors**: [auto-updated]

## 📜 License

Individual skills may have different licenses (specified in each `recipe.toml`). Most community skills use **MIT License** by default.

The repository structure and tooling: **MIT License**

---

**Built with Lumenaute** - Local-first AI that learns from the best, runs on your machine.
