```markdown
# oxc Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill provides a comprehensive guide to contributing to the `oxc` repository, a Rust-based project focused on linting, transformation, and conformance tooling for JavaScript and TypeScript. It covers the project's coding conventions, commit patterns, and detailed step-by-step workflows for common maintenance and feature tasks, including dependency updates, linter rule development, and test management.

## Coding Conventions

- **Language:** Rust (with some TypeScript/JavaScript for plugin/test integration)
- **Framework:** Native Rust (no external web framework)
- **Commit Messages:** Follows [Conventional Commits](https://www.conventionalcommits.org/)  
  - **Prefixes:** `fix`, `refactor`, `chore`, `feat`, `test`, `docs`, `perf`, `release`
  - **Example:**  
    ```
    feat(linter): add no-console rule
    fix(transformer): correct context handling for async functions
    ```
- **File Naming:**  
  - Snake case for Rust: `my_module.rs`
  - TypeScript/JavaScript: `my_plugin.ts`, `rule_tester.test.ts`
- **Import Style:** Relative imports are preferred.
  - Example (Rust):  
    ```rust
    mod utils;
    use crate::context::Context;
    ```
  - Example (TypeScript):  
    ```typescript
    import { fix } from './plugins/fix';
    ```
- **Export Style:** Mixed (Rust uses `pub` for public items, TypeScript uses `export`)
  - Example (Rust):  
    ```rust
    pub fn run_linter() { ... }
    ```
  - Example (TypeScript):  
    ```typescript
    export function runLinter() { ... }
    ```

## Workflows

### Update Rust Crate Dependency
**Trigger:** When a new version of a Rust crate is released and the project wants to stay up to date.  
**Command:** `/update-crate`

1. Update the version of the crate in `Cargo.toml` (or let the lockfile update it implicitly).
2. Regenerate `Cargo.lock`:
    ```sh
    cargo update
    ```
3. Commit the updated `Cargo.lock` (and possibly `Cargo.toml`).
4. Include changelog or release notes in the commit message.

### Update npm Package Dependency
**Trigger:** When a new version of an npm package is released and the project wants to stay up to date.  
**Command:** `/update-npm`

1. Update the version(s) in `package.json` (or let the lockfile update it implicitly).
2. Regenerate `pnpm-lock.yaml`:
    ```sh
    pnpm install
    ```
3. Commit the updated `pnpm-lock.yaml` (and possibly `package.json`).
4. Include changelog or release notes in the commit message.

### Update GitHub Actions Workflow
**Trigger:** When a new version of a GitHub Action is released or workflow improvements are needed.  
**Command:** `/update-actions`

1. Update version(s) of actions in `.github/workflows/*.yml`.
2. Commit the updated workflow file(s).
3. Include changelog or release notes in the commit message.

### Add or Update Linter Rule
**Trigger:** When adding a new linter rule or improving/fixing an existing one.  
**Command:** `/new-linter-rule`

1. Edit or create the rule implementation in `crates/oxc_linter/src/rules/**/rule_name.rs`.
2. Update or add the corresponding snapshot in `crates/oxc_linter/src/snapshots/rule_name.snap`.
3. Optionally update documentation or config enforcement.
4. Optionally update utils or helpers if needed.
5. Example (Rust rule skeleton):
    ```rust
    pub struct NoConsoleRule;

    impl LintRule for NoConsoleRule {
        // implementation...
    }
    ```

### Fix Linter Plugin JS Fix/Suggestion
**Trigger:** When plugin fix/suggestion logic needs to be aligned with ESLint or bugs are found in fix application.  
**Command:** `/fix-plugin-fix`

1. Edit `apps/oxlint/src-js/plugins/fix.ts` or `report.ts`.
2. Edit `apps/oxlint/src-js/package/rule_tester.ts`.
3. Edit `apps/oxlint/src/js_plugins/fix.rs` (Rust side).
4. Update or add test cases in `apps/oxlint/test/fixtures/` or `test/rule_tester.test.ts`.
5. Update conformance snapshots if needed.

### Refactor Shared Context or Traverse Logic
**Trigger:** When context/state/traverse logic needs to be improved, deduplicated, or made more efficient.  
**Command:** `/refactor-traverse`

1. Edit context/state/traverse files in `crates/oxc_transformer` or `crates/oxc_minifier`.
2. Update all affected transform or minifier modules to use the new context/state structure.
3. Remove or consolidate redundant code.
4. Run and update tests as needed.

### Update Conformance Tests or Snapshots
**Trigger:** When rule/plugin logic changes or ESLint version is bumped, requiring new test baselines.  
**Command:** `/update-conformance`

1. Update `apps/oxlint/conformance/snapshots/*.md`.
2. Update or add test fixtures in `apps/oxlint/test/fixtures/**`.
3. Update test logic in `apps/oxlint/conformance/src/groups/*.ts`.
4. Commit all updated snapshots and fixtures.

## Testing Patterns

- **Framework:** Jest (for JS/TS integration and plugin testing)
- **Test File Pattern:** `*.test.ts`
- **Example:**
    ```typescript
    // apps/oxlint/test/rule_tester.test.ts
    import { runRuleTester } from '../src-js/package/rule_tester';

    test('no-console rule', () => {
      expect(runRuleTester('no-console', 'console.log("test")')).toMatchSnapshot();
    });
    ```
- **Rust:** Uses standard Rust test modules:
    ```rust
    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn test_no_console() {
            // test logic...
        }
    }
    ```

## Commands

| Command             | Purpose                                                         |
|---------------------|-----------------------------------------------------------------|
| /update-crate       | Update Rust crate dependencies                                  |
| /update-npm         | Update npm package dependencies                                 |
| /update-actions     | Update GitHub Actions workflow files                            |
| /new-linter-rule    | Add or update a linter rule                                     |
| /fix-plugin-fix     | Fix or improve linter plugin fix/suggestion logic               |
| /refactor-traverse  | Refactor shared context or traversal logic                      |
| /update-conformance | Update conformance test snapshots or fixtures                   |
```
