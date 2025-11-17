# Gum CLI Development Rules

## Core Principle: Single Entrypoint

* One main script at the repo root: `./main-cli.sh` using **gum**.
* All features integrate into this menu — no orphan scripts.
* The menu provides clear descriptions for every action to orient users.

## Implementation Strategy

1. Start with one essential command.
2. As features grow, create separate scripts in the `scripts/` folder.
3. The main CLI should call these scripts — keep `main-cli.sh` clean.
4. Test thoroughly before adding new features.
5. Ensure the menu order makes logical sense.
6. Group related actions together.

## For Every Menu Item

* Display the action name and a one-line description.
* Include default values where applicable.
* Use **gum**’s styling options for visual hierarchy.

## Parameter Control

* Use `gum input` for text inputs with default values.
* Use `gum choose` for option selection.
* Use `gum confirm` for yes/no decisions.

## Preview + Confirm (Mandatory)

* Always show the exact command before execution.
* Use `gum confirm` for any impactful operation.
* Allow the user to abort at the confirmation step.

## Critical Reminder for Every Task

After completing any development task, you must:

1. Add or update the corresponding action in `./main-cli.sh`.
2. Include a descriptive label and help text.
3. Add a command preview and confirmation step.
4. Test the new menu option end-to-end.
