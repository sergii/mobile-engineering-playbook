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

## Architecture bias semantics

`architecture_bias` values describe **likelihood and relative product pressure** for a class of applications.

They do **not** mean:

- implementation priority;
- required architecture;
- required dependency;
- package installation instruction;
- a feature that every product of this type must have.

The current scale is:

```text
low         = uncommon or usually secondary
low_medium  = sometimes relevant
medium      = commonly relevant
medium_high = likely to become important
high        = central product force for this archetype
```

Bias values help an agent ask better questions earlier. They must never bypass the normal decision ladder.

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

For machine-readable archetypes, either use the scale above or explicitly define another scale and its semantics. Never leave ordinal values ambiguous.

Avoid creating archetypes merely to enumerate app categories.
