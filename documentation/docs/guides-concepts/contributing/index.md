---
title: "Contributing Guide | Guides Concepts in Refine"
display_title: "Contributing"
sidebar_label: "Contributing"
description: "Comprehensive step-by-step contributing guide for Refine. Learn how to set up your local environment, develop packages, run tests, format code, create changesets, and submit pull requests."
---

First off, thank you for considering contributing to Refine! 🎉

Refine is an open-source, React-based framework designed for building data-intensive enterprise applications, internal tools, admin panels, and dashboards rapidly. Community contributions are what make Refine thrive. Whether you are fixing a typo in the documentation, reporting a bug, or building a whole new feature or data provider, your help is warmly welcomed!

We follow a [Code of Conduct](https://github.com/refinedev/refine/blob/main/CODE_OF_CONDUCT.md) to ensure an inclusive and welcoming environment for everyone. Please review it before contributing.

---

## Ways to Contribute

There are many valuable ways to get involved with Refine:

- 🌟 **Star the Repository**: Show your appreciation by starring Refine on [GitHub](https://github.com/refinedev/refine).
- 📖 **Improve Documentation**: Fix errors, clarify confusing explanations, or write new guides and tutorials.
- 🐛 **Report Bugs**: Submit bug reports with clear reproduction steps on [GitHub Issues](https://github.com/refinedev/refine/issues).
- 💡 **Suggest Features & Ideas**: Share ideas for new hooks, UI integrations, or optimizations via [GitHub Discussions](https://github.com/refinedev/refine/discussions) or [Discord](https://discord.gg/refine).
- 💻 **Contribute Code**: Solve open issues or build requested features across Refine packages and examples.
- 🔌 **Share Community Integrations**: Have you built a custom data provider, auth provider, or UI integration? Share it on our [Integrations page](/core/integrations) with the community.
- 💬 **Help Fellow Developers**: Answer questions and help newcomers in our [Discord community](https://discord.gg/refine).

---

## Finding Something to Work On

If you are a first-time contributor looking for a good starting point:

1. **`good first issue`**: Check out issues tagged with [`good first issue`](https://github.com/refinedev/refine/labels/good%20first%20issue). These tasks have a well-defined scope and are beginner-friendly.
2. **`help wanted`**: Look at issues labeled [`help wanted`](https://github.com/refinedev/refine/labels/help%20wanted) for community-requested features and enhancements.
3. **Documentation Tasks**: Check open [documentation issues](https://github.com/refinedev/refine/labels/documentation) if you prefer writing docs, fixing guides, or improving code samples.

:::simple Tip for Contributors

Before starting work on an issue, please leave a comment on the issue page expressing your interest and asking to be assigned. This lets the maintainers and community know that someone is actively working on it and avoids duplicated effort.

:::

---

## Step-by-Step Contribution Guide

Here is the complete end-to-end workflow to contribute to Refine:

```
Fork & Clone ➔ Install Dependencies ➔ Create Branch ➔ Develop & Test ➔ Lint & Format ➔ Create Changeset ➔ Commit & PR
```

---

### Step 1: Prerequisites & Environment Setup

Make sure your development machine has the following tools installed:

:::simple System Requirements

- **[Node.js](https://nodejs.org/en/)**: Version `18.x` or higher (we recommend using [`nvm`](https://github.com/nvm-sh/nvm), [`fnm`](https://github.com/Schniz/fnm), or [`volta`](https://volta.sh/)).
- **[pnpm](https://pnpm.io/)**: Version `9.x` or higher (Refine uses pnpm workspaces).
  ```sh title="Terminal"
  npm install -g pnpm@latest
  ```
- **[Git](https://git-scm.com/)** and a **[GitHub](https://github.com/)** account.
- **[Visual C++ Redistributable](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)** _(Windows only)_: Required if compiling native dependencies.

:::

:::simple Recommended VS Code Extensions

- **[Biome](https://marketplace.visualstudio.com/items?itemName=biomejs.biome)**: For fast linting and formatting on save.
- **[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)**: For formatting markdown files.

:::

---

### Step 2: Fork and Clone the Repository

1. **Fork the repository**: Navigate to [github.com/refinedev/refine](https://github.com/refinedev/refine) and click the **Fork** button in the top-right corner.
2. **Clone your fork**:
   ```sh title="Terminal"
   git clone https://github.com/<YOUR-USERNAME>/refine.git
   cd refine
   ```
3. **Add the upstream remote**:
   ```sh title="Terminal"
   git remote add upstream https://github.com/refinedev/refine.git
   git fetch upstream
   ```
4. **Create a new branch**:
   Always branch off the latest `main` branch with a descriptive branch name:
   ```sh title="Terminal"
   git checkout main
   git pull upstream main
   git checkout -b fix/issue-description
   # or
   git checkout -b feat/feature-name
   ```

---

### Step 3: Install Dependencies & Monorepo Structure

Refine is structured as a monorepo powered by **pnpm workspaces**.

To install all dependencies across the entire monorepo and link packages:

```sh title="Terminal"
pnpm install
```

:::simple Fast Install Tip

If you want to skip running package build scripts during the initial install to speed up setup:

```sh title="Terminal"
pnpm install --ignore-scripts
```

:::

#### Monorepo Structure Overview

```
refine/
├── packages/              # All publishable Refine npm packages
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
│   └── ...                # Additional packages and providers
├── examples/              # 250+ example apps demonstrating integrations
│   ├── base-antd/         # Basic Ant Design starter app
│   ├── base-mui/          # Basic Material UI starter app
│   └── ...
├── documentation/         # Refine official Docusaurus documentation website
├── .changeset/            # Changeset definitions and configurations
└── .github/               # GitHub workflows, issue/PR templates
```

:::simple Resetting / Cleaning Environment

If you ever experience corrupted node_modules or cache issues, run:

```sh title="Terminal"
pnpm coffee
# or
pnpm nuke && pnpm install
```

:::

---

### Step 4: Developing Packages and Running Examples

#### Building Packages

You can build individual packages or all packages using the `--scope` flag:

```sh title="Terminal"
# Build a specific package
pnpm build --scope @refinedev/core

# Build multiple packages
pnpm build --scope @refinedev/core --scope @refinedev/antd

# Build all packages across the repository
pnpm build:all
```

#### Running in Development (Watch) Mode

To develop a package while seeing changes reflected live in an example application, start them together:

```sh title="Terminal"
pnpm dev --scope @refinedev/antd --scope base-antd
```

**How it works:**

- When you edit code in `packages/antd/src`, the package automatically recompiles.
- The `base-antd` example immediately re-bundles and hot-reloads in your browser (usually at `http://localhost:3000` or `http://localhost:5173`).

<details>
<summary><b>How to add a dependency to a package?</b></summary>

To add a new dependency to a specific package:

```sh title="Terminal"
# From the repository root:
pnpm --filter @refinedev/core add <package-name>

# Or navigate to the package folder:
cd packages/core
pnpm add <package-name>
```

</details>

---

### Step 5: Working on Documentation

Refine's documentation is built with [Docusaurus](https://docusaurus.io/).

You can run documentation scripts either from the repository root or inside the `documentation/` folder:

```sh title="Terminal"
# From the repository root:
pnpm dev:docs

# Or inside the documentation directory:
cd documentation
pnpm install
pnpm dev:docs     # Fast docs dev server (skips type and prop table generation)
pnpm dev:blog     # Start blog section only
pnpm dev          # Full documentation server (including all generations)
pnpm build        # Verify production build of the documentation
pnpm serve        # Preview the production build locally
```

#### Creating Previews and Code Samples with Sandpack

Refine uses [CodeSandbox Sandpack](https://sandpack.codesandbox.io/) to provide live editable previews in the documentation. We provide a custom `<Sandpack />` component to embed runnable Refine apps directly in doc pages.

- [Example usage in `useForm` hook documentation](/core/docs/data/hooks/use-form/#usage)
- [Source code for Sandpack example](https://github.com/refinedev/refine/blob/main/documentation/docs/data/hooks/use-form/basic-usage.tsx)

---

### Step 6: Running Tests

Refine uses [Vitest](https://vitest.dev/) and [Jest](https://jestjs.io/) with [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro/) for unit and component tests, and [Cypress](https://www.cypress.io/) for end-to-end (E2E) tests.

```sh title="Terminal"
# Run tests for a specific package
pnpm test --scope @refinedev/core

# Run tests for multiple packages
pnpm test --scope @refinedev/core --scope @refinedev/antd

# Run tests with coverage
pnpm test:coverage --scope @refinedev/core

# Run all tests across the repository
pnpm test:all
```

#### End-to-End (E2E) Tests

```sh title="Terminal"
# Open interactive Cypress UI
pnpm cypress:open

# Run Cypress headless in Chrome
pnpm cypress:run
```

:::simple Testing Guidelines

- Every bug fix should include a unit test that reproduces the bug and verifies the fix.
- Every new feature should include comprehensive tests covering typical use cases and edge cases.
- If you need guidance on writing tests, feel free to ask in our [Discord community room](https://discord.gg/refine).

:::

---

### Step 7: Linting & Formatting

Refine uses [Biome](https://biomejs.dev/) for linting & formatting across JavaScript, TypeScript, and JSON files, [Prettier](https://prettier.io/) for Markdown files, and [Syncpack](https://github.com/JamieMason/syncpack) for workspace dependency consistency.

```sh title="Terminal"
# Check for linting and formatting issues
pnpm lint

# Automatically fix linting and formatting issues
pnpm lint:fix

# Check consistency of package.json dependencies across the monorepo
pnpm sp
```

:::simple VS Code Setup

We highly recommend installing the [Biome VS Code extension](https://biomejs.dev/reference/vscode/) and enabling formatting on save to avoid unexpected CI failures.

:::

---

### Step 8: Conventional Commit Guidelines

Refine enforces the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification using [commitlint](https://commitlint.js.org/).

#### Commit Message Format

```
<type>(optional scope): <description>
```

#### Common Types

- `feat`: A new feature or capability
- `fix`: A bug fix
- `docs`: Documentation improvements or additions
- `refactor`: Code changes that neither fix a bug nor add a feature
- `test`: Adding or updating test cases
- `perf`: Code changes that improve performance
- `chore`: Tooling, workflow, or dependency updates

#### Examples

```sh
feat(core): add autoSave support to useForm
fix(antd): handle undefined value in DateField
docs(contributing): make contribution guide beginner-friendly
test(simple-rest): add unit tests for custom header handling
```

:::simple Good to know

Husky and commitlint validate your commit messages automatically when committing. If the commit message format does not follow Conventional Commits, the commit will be rejected.

:::

---

### Step 9: Creating a Changeset

Refine uses [Changesets](https://github.com/changesets/changesets) to automate semantic versioning and changelog generation.

#### When is a Changeset Required?

- **Required**: For any bug fix, feature, performance enhancement, or breaking change made inside `packages/*` or `create-refine-app`.
- **Not Required**: For pure documentation changes (`documentation/*`), examples, typos, or internal CI/tooling adjustments.

#### How to Create a Changeset

Run the Changeset CLI from the repository root:

```sh title="Terminal"
pnpm changeset
```

Follow the interactive prompts:

1. **Select Package(s)**: Use the arrow keys and <kbd>Space</kbd> to select the package(s) you modified, then press <kbd>Enter</kbd>.
2. **Select Version Bump Type**:
   - `patch`: For bug fixes, non-breaking internal changes, and minor improvements.
   - `minor`: For new features and backwards-compatible API additions.
   - `major`: For breaking changes (usually coordinated with maintainers).
3. **Write a Summary**: Write a clear, concise summary of the change and reference the relevant issue number (`#1234`).

#### Example Changeset Files

```md title=".changeset/happy-tigers-sing.md"
---
"@refinedev/core": minor
---

feat: add `autoSave` configuration to `useForm` hook #1234

Now you can enable automatic draft saving directly in `useForm` by providing the `autoSave` property.

Resolves #1234
```

or

```md title=".changeset/clever-foxes-run.md"
---
"@refinedev/antd": patch
---

fix: prevent select dropdown flickering during live search #5678

Resolved an issue where rapid typing in the search box caused the dropdown menu to flicker and close unexpectedly.

Fixes #5678
```

Stage and commit the generated changeset file with your changes:

```sh title="Terminal"
git add .changeset/
git commit -m "chore(changeset): add changeset for antd select fix"
```

---

### Step 10: Opening a Pull Request

Once your code is ready, tested, formatted, and has a changeset (if needed), submit your Pull Request!

1. **Push your branch to your GitHub fork**:
   ```sh title="Terminal"
   git push -u origin fix/issue-description
   ```
2. **Open the Pull Request**:
   - Go to the [Refine Pull Requests](https://github.com/refinedev/refine/pulls) page.
   - Click **New pull request** or **Compare & pull request**.
   - Make sure `base: main` is selected.
3. **Fill out the Pull Request Template**:
   - Check the checklist items (commit format, tests, docs, changeset).
   - Describe the **Current Behavior** and the **New Behavior**.
   - Link the relevant issue (e.g. `Fixes #1234`).
4. **CI Checks**:
   - Automated GitHub Actions will run tests, Biome linting, and verify changesets.
   - If any checks fail, review the logs, fix the issues locally, and push new commits to your branch.
5. **Review and Collaboration**:
   - Maintainers will review your PR and provide feedback.
   - Once approved, maintainers will label it as `pr-ready` and merge it into `main`!

---

## Release Cycle

Refine follows a predictable release cycle:

- **Monthly Releases**: Refine publishes regular releases every month containing all features and fixes merged during that cycle.
- **Milestones**: Approved and merged PRs are attached to a milestone tracking the upcoming monthly release.
- **Patch Releases**: Critical bug fixes are released immediately as out-of-band patch versions.

---

## Troubleshooting & Frequently Asked Questions

<details>
<summary><b>pnpm install fails or reports lockfile / workspace errors</b></summary>

Clean your local workspace artifacts and re-install:

```sh title="Terminal"
pnpm coffee
# or
pnpm nuke && pnpm install
```

</details>

<details>
<summary><b>My commit failed due to commitlint</b></summary>

Make sure your commit message follows the format `type(scope): description`. You can fix your previous commit message with:

```sh title="Terminal"
git commit --amend -m "fix(core): resolve data provider error handling"
```

</details>

<details>
<summary><b>The Changeset bot flagged that a changeset is missing on my PR</b></summary>

If your PR alters any package in `packages/`, run `pnpm changeset` locally, choose the affected package and bump level, write a description referencing your issue, and push the newly generated file in `.changeset/`.

</details>

<details>
<summary><b>Biome lint or formatting failed in CI</b></summary>

Run the automatic fix command locally:

```sh title="Terminal"
pnpm lint:fix
```

Then stage, commit, and push the formatted files.

</details>

<details>
<summary><b>How do I keep my branch up to date with upstream/main?</b></summary>

Sync your local branch with upstream changes:

```sh title="Terminal"
git fetch upstream
git rebase upstream/main
# or git merge upstream/main
git push origin <your-branch-name> --force-with-lease
```

</details>

---

## Getting Help & Community

Have questions or need assistance during your contribution journey?

- 💬 **Discord**: Join our [Discord community](https://discord.gg/refine) and chat in `#contributing`.
- 💡 **GitHub Discussions**: Propose ideas on [GitHub Discussions](https://github.com/refinedev/refine/discussions).
- 🐛 **GitHub Issues**: Report bugs and browse tasks on [GitHub Issues](https://github.com/refinedev/refine/issues).
- 🌐 **Website & Docs**: Explore documentation at [refine.dev](https://refine.dev).

Thank you for contributing to Refine! 🚀
