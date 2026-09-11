# Application Archetypes

Archetypes capture reusable context for broad classes of mobile products without turning that context into universal engineering policy.

They sit between the core playbook and a specific product:

```text
Mobile Engineering Playbook
        ↓
Application Archetype
        ↓
Product-specific rules
```

## What an archetype is

An archetype may describe:

- dominant user interaction patterns;
- common product states;
- likely domain components;
- architectural pressures;
- expected importance of offline behavior, server state, gestures, forms, media, or real-time updates;
- useful first vertical slices;
- tools that may become valuable and the conditions that justify them.

## What an archetype is not

An archetype is not:

- a starter kit;
- a package manifest;
- a directory structure to copy blindly;
- a mandatory architecture;
- a substitute for product requirements.

Do not apply an archetype silently because an application looks similar. The product repository or the user should explicitly reference it.

Project-specific rules always override archetype guidance.

## Current archetypes

- [`camera-operational`](./camera-operational/) - camera-first operational workflows involving physical-world objects, identification, confirmation, and operational state changes.

## Adding a new archetype

Add a new archetype when multiple products share a meaningful set of forces that are too domain-specific for the core playbook.

A useful archetype should usually include:

```text
<archetype>/
  README.md
  archetype.yaml
  slices.md
```

Keep the human explanation in Markdown and the compact machine-readable profile in YAML.

Avoid creating archetypes merely to enumerate app categories.
