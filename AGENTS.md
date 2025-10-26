# AGENTS.md

**Audience:** Developers, researchers, and operators of LLM agents in this repository
**Tech stack:** Python 3.11+, `uv` for environment and package management, CLI tools, optional MCP/HTTP tools

---

## Gum CLI Development Rules

### Core Principle: Single Entrypoint

* One main script at the repo root: `./main-cli.sh` using **gum**.
* All features integrate into this menu — no orphan scripts.
* The menu provides clear descriptions for every action to orient users.

### Implementation Strategy

1. Start with one essential command.
2. As features grow, create separate scripts in the `scripts/` folder.
3. The main CLI should call these scripts — keep `main-cli.sh` clean.
4. Test thoroughly before adding new features.
5. Ensure the menu order makes logical sense.
6. Group related actions together.

### For Every Menu Item

* Display the action name and a one-line description.
* Include default values where applicable.
* Use **gum**’s styling options for visual hierarchy.

### Parameter Control

* Use `gum input` for text inputs with default values.
* Use `gum choose` for option selection.
* Use `gum confirm` for yes/no decisions.

### Preview + Confirm (Mandatory)

* Always show the exact command before execution.
* Use `gum confirm` for any impactful operation.
* Allow the user to abort at the confirmation step.

### Critical Reminder for Every Task

After completing any development task, you must:

1. Add or update the corresponding action in `./main-cli.sh`.
2. Include a descriptive label and help text.
3. Add a command preview and confirmation step.
4. Test the new menu option end-to-end.

---

## Determinism & Reproducibility

* Use `uv` lockfiles and constraints for replicable environments.
* Follow the Gum CLI instructions described above for consistency in workflows.

---

## `uv` Workflow Rules

### Installation & Sync

```bash
# Create and use a project-local virtual environment
uv venv --python 3.11
source .venv/bin/activate

# Install using lock (required in CI and for agents)
uv sync --locked

# If you add a dependency:
uv add ruff==0.6.9
uv lock
```

### Constraints & Locking

* Maintain `uv.lock` under version control.
* Pin critical libraries in `constraints.txt`.
* Agents must not change constraints without Planner + Reviewer approval.

### Dev Utilities

```bash
# Lint & format
uv run ruff check .
uv run ruff format .

# Run tests
uv run pytest -q
```

### Make Targets (Optional)

```Makefile
.PHONY: setup lint test run
setup: ; uv sync --locked
lint:  ; uv run ruff check . && uv run ruff format --check .
test:  ; uv run pytest -q
run:   ; uv run python -m app
```

---

## Secrets & Config

* Never commit secrets — use a `.env` file (gitignored).
* Agents load configuration from `configs/agents.yaml` and environment variables.

**Example `.env.example`**

```
OPENAI_API_KEY=...
VECTORDB_URL=...
HTTP_PROXY=
```

**Example `configs/agents.yaml` (fragment)**

```yaml
agents:
  coder:
    model: "gpt-4o-mini"
    temperature: 0.2
    max_tokens: 2000
    tools: ["fs.read","fs.write","git.diff","git.apply","pytest.run"]
    stop_phrases: ["<END>"]
retriever:
  web:
    rate_limit: { requests_per_min: 15, burst: 5 }
    cache_ttl_s: 3600
```

### Security Rules

* Secrets are only read from process environment at runtime.
* Logs must redact any `*_KEY`, `*_TOKEN`, `*_SECRET` fields via `logging.yaml` filters.

---

## Coding Standards

* **Style:** black/PEP8, ruff default rules, Google-style docstrings, type hints everywhere.
* **Imports:** use absolute imports; no wildcard imports.
* **Errors:** never swallow exceptions — wrap with context and re-raise.

---

Would you like me to also apply consistent section numbering (e.g., `6.1`, `6.2`) to make it ready for integration into a full AGENTS.md document?
