# Contributing to Refine

First off, thank you for considering contributing to Refine! 🎉

Refine is an open-source, React-based framework for building data-heavy enterprise applications, internal tools, admin panels, and B2B platforms rapidly. Community contributions are what make Refine great, and we welcome contributions of all kinds—from reporting bugs and improving documentation to submitting bug fixes and building new features.

This guide provides a clear, step-by-step walkthrough for first-time contributors and seasoned developers alike.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Finding Something to Work On](#finding-something-to-work-on)
- [Step-by-Step Contribution Guide](#step-by-step-contribution-guide)
  - [1. Prerequisites](#1-prerequisites)
  - [2. Fork and Clone the Repository](#2-fork-and-clone-the-repository)
  - [3. Install Dependencies](#3-install-dependencies)
  - [4. Repository & Monorepo Structure](#4-repository--monorepo-structure)
  - [5. Developing Packages and Running Examples](#5-developing-packages-and-running-examples)
  - [6. Working on Documentation](#6-working-on-documentation)
  - [7. Running Tests](#7-running-tests)
  - [8. Linting and Formatting](#8-linting-and-formatting)
  - [9. Conventional Commit Guidelines](#9-conventional-commit-guidelines)
  - [10. Creating a Changeset](#10-creating-a-changeset)
  - [11. Opening a Pull Request](#11-opening-a-pull-request)
- [Release Cycle](#release-cycle)
- [Troubleshooting & FAQs](#troubleshooting--faqs)
- [Getting Help & Community](#getting-help--community)

---

## Code of Conduct

We follow a [Code of Conduct](https://github.com/refinedev/refine/blob/main/CODE_OF_CONDUCT.md). Please make sure you read and abide by it when interacting with the community and maintainers.

---

## How Can I Contribute?

There are many ways to contribute to Refine:

- 🌟 **Star the Repository**: Show your support by starring Refine on [GitHub](https://github.com/refinedev/refine).
- 📖 **Improve Documentation**: Fix typos, clarify guides, or write tutorials for complex use cases.
- 🐛 **Report Bugs**: Submit detailed bug reports with reproduction links via [GitHub Issues](https://github.com/refinedev/refine/issues).
- 💡 **Suggest Features**: Propose new features, hooks, UI integrations, or data providers.
- 💻 **Contribute Code**: Fix open issues or implement requested features.
- 🔌 **Share Integrations**: Built a custom data provider, auth provider, or UI integration? Share it with the community on our [Integrations](https://refine.dev/integrations/) page.
- 💬 **Help Others**: Answer questions in our [Discord community](https://discord.gg/refine) or on [GitHub Discussions](https://github.com/refinedev/refine/discussions).

---

## Finding Something to Work On

If you are looking for somewhere to start:

1. Browse issues with the [`good first issue`](https://github.com/refinedev/refine/labels/good%20first%20issue) label. These are well-scoped tasks ideal for first-time contributors.
2. Check issues with the [`help wanted`](https://github.com/refinedev/refine/labels/help%20wanted) label for community-requested features and fixes.
3. Look at open [Documentation Issues](https://github.com/refinedev/refine/labels/documentation) if you prefer writing docs or examples.

> [!TIP]
> Before starting work on an issue, please leave a comment on the issue asking to be assigned so that maintainers and other contributors know you are working on it. This avoids duplicate effort!

---

## Step-by-Step Contribution Guide

Follow these steps to set up your local environment, make your changes, and submit your pull request.

```
Fork & Clone ➔ Install Dependencies ➔ Create Branch ➔ Develop & Test ➔ Lint & Format ➔ Create Changeset ➔ Commit & PR
```

### 1. Prerequisites

Make sure you have the following installed on your machine:

- **Node.js**: Version `18.x` or higher (we recommend using [`nvm`](https://github.com/nvm-sh/nvm), [`fnm`](https://github.com/Schniz/fnm), or [`volta`](https://volta.sh/)).
- **pnpm**: Version `9.x` or higher (Refine uses pnpm workspaces).
  ```bash
  npm install -g pnpm@latest
  # or using corepack
  corepack enable && corepack prepare pnpm@latest --activate
  ```
- **Git**: Version `2.x` or higher and a [GitHub](https://github.com/) account.
- **Visual C++ Redistributable** _(Windows users only)_: [Latest Supported MSVC](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist) if compiling native dependencies.

**Recommended VS Code Extensions:**

- [Biome](https://marketplace.visualstudio.com/items?itemName=biomejs.biome) for fast linting & formatting.
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) for markdown formatting.

---

### 2. Fork and Clone the Repository

1. **Fork the repository** on GitHub by clicking the **Fork** button at the top right of the [Refine repository](https://github.com/refinedev/refine).
2. **Clone your fork** to your local machine:
   ```bash
   git clone https://github.com/<YOUR-USERNAME>/refine.git
   cd refine
   ```
3. **Configure the upstream remote** so you can easily sync with the official repository:
   ```bash
   git remote add upstream https://github.com/refinedev/refine.git
   git fetch upstream
   ```
4. **Create a new branch** for your work from `main`:
   ```bash
   # Ensure your local main is up to date
   git checkout main
   git pull upstream main

   # Create and switch to your feature/bugfix branch
   git checkout -b fix/issue-description
   # or
   git checkout -b feat/feature-name
   ```

---

### 3. Install Dependencies

Refine uses **pnpm workspaces** to manage packages across the monorepo.

Run the following command from the root directory to install all dependencies and link packages:

```bash
pnpm install
```

> [!TIP]
> If you want to skip running build and prepare scripts during initial installation for faster setup, you can run:
>
> ```bash
> pnpm install --ignore-scripts
> ```

If you ever encounter corrupted build artifacts or lockfile discrepancies, you can clean and re-install with:

```bash
pnpm coffee
# or
pnpm nuke && pnpm install
```

---

### 4. Repository & Monorepo Structure

Refine is structured as a monorepo containing multiple packages, example apps, and documentation:

```
refine/
├── packages/              # All official Refine npm packages
│   ├── core/              # @refinedev/core (Headless core framework)
│   ├── antd/              # @refinedev/antd (Ant Design integration)
│   ├── mui/               # @refinedev/mui (Material UI integration)
│   ├── mantine/           # @refinedev/mantine (Mantine integration)
│   ├── chakra-ui/         # @refinedev/chakra-ui (Chakra UI integration)
│   ├── simple-rest/       # @refinedev/simple-rest (REST Data Provider)
│   ├── graphql/           # @refinedev/graphql (GraphQL Data Provider)
│   ├── supabase/          # @refinedev/supabase (Supabase Data Provider)
│   ├── react-router/      # @refinedev/react-router (React Router integration)
│   ├── nextjs-router/     # @refinedev/nextjs-router (Next.js router integration)
│   ├── devtools/          # @refinedev/devtools (Refine Devtools)
│   ├── cli/               # @refinedev/cli (Refine CLI)
│   └── ...                # Other packages and data providers
├── examples/              # 250+ example projects demonstrating integrations
│   ├── base-antd/         # Basic Ant Design starter example
│   ├── base-mui/          # Basic Material UI starter example
│   ├── tutorial-antd/     # Tutorial examples
│   └── ...
├── documentation/         # Refine official documentation website (Docusaurus)
├── .changeset/            # Changeset definitions for releases and changelogs
└── .github/               # GitHub workflows, templates, and actions
```

---

### 5. Developing Packages and Running Examples

#### Building Packages

You can build specific packages using `--scope` or build all packages:

```bash
# Build a single package (e.g. core)
pnpm build --scope @refinedev/core

# Build multiple packages
pnpm build --scope @refinedev/core --scope @refinedev/antd

# Build all packages in the monorepo
pnpm build:all
```

#### Running Packages in Development / Watch Mode

The best way to develop a package is to run it in watch mode together with an example app:

```bash
# Run @refinedev/antd alongside the base-antd example app
pnpm dev --scope @refinedev/antd --scope base-antd
```

**How it works:**

- When you edit code in `packages/antd/src`, the package automatically recompiles.
- The `base-antd` example immediately re-bundles and hot-reloads in your browser (typically at `http://localhost:3000` or `http://localhost:5173`).

#### Adding a Dependency to a Package

To add a new dependency to a specific package in the workspace:

```bash
# From the repository root:
pnpm --filter @refinedev/core add <dependency-name>

# Or navigate to the package folder:
cd packages/core
pnpm add <dependency-name>
```

---

### 6. Working on Documentation

Refine's documentation is built using [Docusaurus](https://docusaurus.io/).

To work on documentation:

```bash
# Option 1: Run documentation from the repository root
pnpm dev:docs

# Option 2: Run directly from the documentation folder
cd documentation
pnpm install
pnpm dev:docs     # Fast docs dev server (skips heavy type and prop generation)
pnpm dev:blog     # Start blog section only
pnpm dev          # Full documentation server (including live previews)
pnpm build        # Build production docs bundle to verify changes
pnpm serve        # Preview the production build locally
```

#### Live Previews & Sandpack

Refine uses [CodeSandbox Sandpack](https://sandpack.codesandbox.io/) for interactive live previews within doc pages. If you are adding or editing interactive examples, check the `<Sandpack />` component references in `documentation/docs/`.

---

### 7. Running Tests

Refine uses **Vitest** and **Jest** with [`@testing-library/react`](https://testing-library.com/docs/react-testing-library/intro/) for unit & component tests, and **Cypress** for end-to-end (E2E) testing.

#### Unit & Component Tests

```bash
# Run tests for a specific package
pnpm test --scope @refinedev/core

# Run tests for multiple packages
pnpm test --scope @refinedev/core --scope @refinedev/antd

# Run tests with coverage
pnpm test:coverage --scope @refinedev/core

# Run all tests across the monorepo
pnpm test:all
```

#### End-to-End (E2E) Tests

```bash
# Open Cypress interactive test runner
pnpm cypress:open

# Run Cypress tests headless
pnpm cypress:run
```

> [!NOTE]
> We expect proper tests for every new feature or bug fix. When fixing a bug, please add a test reproducing the issue to prevent regressions.

---

### 8. Linting and Formatting

Refine uses [Biome](https://biomejs.dev/) for fast linting & formatting across JavaScript, TypeScript, and JSON files, [Prettier](https://prettier.io/) for Markdown files, and [Syncpack](https://github.com/JamieMason/syncpack) for `package.json` dependency consistency.

```bash
# Check code for linting and formatting issues
pnpm lint

# Automatically fix linting and formatting issues
pnpm lint:fix

# Check consistency of package.json dependencies across the monorepo
pnpm sp
```

> [!TIP]
> Install the [Biome VS Code extension](https://marketplace.visualstudio.com/items?itemName=biomejs.biome) and enable "Format on Save" to automatically format your code according to project rules.

---

### 9. Conventional Commit Guidelines

Refine follows the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification. This keeps git history clean and enables automated changelog generation.

#### Commit Format

```
<type>(<scope>): <description>
```

#### Commit Types

| Type       | Description                                                   |
| :--------- | :------------------------------------------------------------ |
| `feat`     | A new feature or API addition                                 |
| `fix`      | A bug fix                                                     |
| `docs`     | Documentation changes only                                    |
| `style`    | Formatting, missing semicolons, whitespace changes            |
| `refactor` | Code change that neither fixes a bug nor adds a feature       |
| `perf`     | Performance improvements                                      |
| `test`     | Adding or updating tests                                      |
| `build`    | Changes that affect the build system or external dependencies |
| `ci`       | Changes to CI configuration files and scripts                 |
| `chore`    | Routine tasks, maintenance, tooling updates                   |

#### Scope Examples

Scopes correspond to the package or area modified: `core`, `antd`, `mui`, `mantine`, `chakra-ui`, `simple-rest`, `graphql`, `supabase`, `react-router`, `nextjs-router`, `cli`, `devtools`, `docs`, etc.

#### Examples of Good Commit Messages

```
feat(core): add autoSave support to useForm hook
fix(antd): resolve dropdown menu flicker on select filter
docs(contributing): make contribution guide beginner-friendly
test(simple-rest): add unit tests for custom header handling
```

> [!NOTE]
> [Commitlint](https://commitlint.js.org/) will check your commit message automatically via Husky git hooks. If a commit message fails the format, your commit will be blocked until corrected.

---

### 10. Creating a Changeset

Refine uses [Changesets](https://github.com/changesets/changesets) to manage package versioning and changelogs.

#### When is a Changeset Required?

- **REQUIRED**: Any bugfix, feature, performance improvement, or breaking change in `packages/*` or `create-refine-app`.
- **NOT REQUIRED**: Changes only affecting `documentation/`, examples, internal CI/tooling, or typo fixes.

#### How to Create a Changeset

Run the interactive changeset CLI from the repository root:

```bash
pnpm changeset
```

Follow the interactive prompts:

1. **Select Package(s)**: Use the arrow keys and <kbd>Space</kbd> to select the package(s) you modified, then press <kbd>Enter</kbd>.
2. **Select Version Bump Type**:
   - `patch`: For backwards-compatible bug fixes and internal improvements.
   - `minor`: For new backwards-compatible features and APIs.
   - `major`: For breaking changes (usually coordinated with maintainers).
3. **Write a Description**: Provide a clear, human-readable summary of the change and include the related issue number (`#1234`).

#### Changeset File Example

The command will generate a markdown file in `.changeset/<random-name>.md`:

```markdown
---
"@refinedev/antd": patch
---

fix: prevent select dropdown flickering during search #1234

Resolved an issue where searching inside Select caused the dropdown menu to flicker and close unexpectedly.

Fixes #1234
```

Add and commit the generated changeset file with your branch:

```bash
git add .changeset/
git commit -m "chore(changeset): add changeset for antd fix"
```

---

### 11. Opening a Pull Request

Once you have implemented your changes, added tests, formatted the code, and created a changeset (if applicable), you are ready to open a Pull Request!

1. **Push your branch to your GitHub fork**:
   ```bash
   git push -u origin fix/issue-description
   ```
2. **Open the Pull Request**:
   - Go to your fork on GitHub or visit the [Refine Pull Requests](https://github.com/refinedev/refine/pulls) page.
   - Click **New pull request** or **Compare & pull request**.
   - Ensure the base repository is `refinedev/refine` and the base branch is `main`.
3. **Fill in the Pull Request Template**:
   - Complete the checklist (commit convention, tests, docs, changeset).
   - Describe the **Current Behavior** and the **New Behavior**.
   - Reference the issue(s) being resolved (e.g. `Fixes #1234`).
4. **Wait for CI Checks**:
   - Automated GitHub Actions will run the test suite, Biome lint checks, and verify your changeset.
   - If any CI checks fail, click on "Details" to see the logs, fix the issues locally, and push additional commits to your branch.
5. **Code Review**:
   - Maintainers will review your PR and may suggest improvements.
   - Feel free to ask questions or discuss feedback directly in the PR comments.

---

## Release Cycle

- **Monthly Releases**: Refine releases a new version on a monthly schedule.
- **Milestones**: Approved and merged PRs are tagged with a milestone tracking the upcoming monthly release.
- **Patch Releases**: Critical bug fixes may be released out-of-band as patch versions.
- When your PR is approved and ready, maintainers will label it as `pr-ready` and merge it into `main`.

---

## Troubleshooting & FAQs

<details>
<summary><b>pnpm install fails or complains about node_modules/lockfiles</b></summary>

Clean your local build cache and re-install all packages:

```bash
pnpm coffee
# or manually:
pnpm nuke
pnpm install
```

</details>

<details>
<summary><b>My commit was rejected by Commitlint</b></summary>

Your commit message must adhere to Conventional Commits format (`type(scope): description`). You can update your last commit message with:

```bash
git commit --amend -m "fix(core): correct state update in useTable"
```

</details>

<details>
<summary><b>Changeset bot warns that a changeset is missing on my PR</b></summary>

If your PR modifies any package under `packages/`, run:

```bash
pnpm changeset
```

Follow the prompts, stage the generated file in `.changeset/`, commit, and push to your branch.
</details>

<details>
<summary><b>Biome lint or format fails on CI</b></summary>

Run the following command locally to automatically format and fix lint issues:

```bash
pnpm lint:fix
```

Then commit and push your updated files.
</details>

<details>
<summary><b>How do I update my branch with the latest upstream changes?</b></summary>

To sync your branch with upstream `main`:

```bash
git fetch upstream
git rebase upstream/main
# or git merge upstream/main
git push origin <your-branch-name> --force-with-lease
```

</details>

---

## Getting Help & Community

Need help or want to discuss ideas before coding? We are here for you!

- 💬 **Discord**: Join our friendly [Discord Community](https://discord.gg/refine) and chat in `#contributing`.
- 💡 **GitHub Discussions**: Share ideas and ask questions on [GitHub Discussions](https://github.com/refinedev/refine/discussions).
- 🐛 **GitHub Issues**: Report bugs or browse open tasks on [GitHub Issues](https://github.com/refinedev/refine/issues).
- 🌐 **Documentation**: Read the full docs at [refine.dev](https://refine.dev).

Thank you for contributing to make Refine even better! 🚀
