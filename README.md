# MILLYSH

MILLYSH is my personal operating system on the internet: part portfolio, part software laboratory, and part home for whatever I want to build next.

It exists to make coding fun again. This repository gives me a place to learn unfamiliar technologies, experiment without needing a business case, and collect the projects that come out of that work.

The public site should become a living portfolio rather than a gallery I maintain by hand. As I add projects, they should naturally appear with their purpose, status, technology, and links. Behind that public surface, MILLYSH can grow into a private set of tools and workflows built specifically for me.

## Principles

- **Build for curiosity.** Learning and enjoyment are valid reasons to add something.
- **Keep the center stable and the edges experimental.** The main site connects everything, while side projects can choose their own technologies and deployment schedules.
- **Prefer working vertical slices.** Prove a small idea from interface to deployment before building a generalized platform around it.
- **Extract patterns after they repeat.** Shared packages should represent knowledge earned from real projects, not abstractions invented in advance.
- **Make public and private boundaries explicit.** A personal operating system can have a public portfolio without exposing its private controls or data.
- **Let the portfolio emerge from the work.** Projects register with MILLYSH; the site presents them.

## Architectural direction

MILLYSH is a monorepo with a central home site, independently deployable projects, shared building blocks, and infrastructure expressed as code.

```text
apps/
  home/              # The public milly.sh experience
  <project>/         # Independently deployable experiments

packages/
  ui/                # Shared components
  design-tokens/     # Color, typography, spacing, and motion
  project-registry/  # Metadata used to present projects
  tooling/           # Shared development conventions

infra/               # Domains, deployments, and cloud resources
```

This is a direction, not a requirement that every directory exist immediately. The repository should grow in response to working software.

Side projects should generally behave like independent buildings on the same property. They can live on their own subdomains, deploy separately, and use different technologies without forcing the main site to change with them.

## Technology playground

The current technologies under consideration include:

- **StyleX** for design tokens and the component library
- **TanStack Start** for the main site
- **SST** for infrastructure as code
- **Cloudflare** for hosting and related infrastructure

These choices are experiments, not the identity of the project. A tool stays when using it makes MILLYSH more capable, understandable, or enjoyable.

## First milestone

The first milestone is a walking skeleton: the smallest complete version that proves the major parts can work together.

1. Create a real `milly.sh` homepage.
2. Establish a small visual language with StyleX.
3. Define one project in a project registry and render it on the homepage.
4. Deploy the homepage to Cloudflare.
5. Document what should be shared or automated before adding another project.

The goal is not to design the final platform. The goal is to make one complete path work, learn from it, and let that experience shape the architecture.

## Development

This repository uses pnpm workspaces and Turborepo. Repository-specific commands and conventions live in [`AGENTS.md`](./AGENTS.md).
