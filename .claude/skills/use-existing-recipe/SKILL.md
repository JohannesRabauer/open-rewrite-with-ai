---
name: use-existing-recipe
description: 'Use existing OpenRewrite recipes with an online-first workflow. Use for recipe discovery, YAML composition, and execution on Java, Maven, Docker, and Gradle projects.'
argument-hint: 'Technology and goal, for example: gradle + upgrade slf4j recipe'
---

# Use Existing OpenRewrite Recipe

## When To Use
- You want to apply an existing recipe instead of writing a new one.
- You need a portable workflow that works even when the source repository is not available.
- You are composing recipes in YAML and want confidence checks.

## Inputs
- Goal of the refactor.
- Technology area: Java, Maven, Docker, or Gradle.
- Candidate recipe names or package area.
- Build tool: Maven or Gradle.

## Procedure
1. Discover candidate recipes online from the OpenRewrite docs catalog.
2. Pick the technology reference document in this skill.
3. Compose or enable the recipe in YAML using `recipeList` and optional `preconditions`.
4. If options are required, pass them by nested mapping under the recipe name.
5. Run a dry run first, then apply the recipe.
6. Validate with targeted tests for your project.

## Technology References
- Java: [Java guide](./references/java.md)
- Maven: [Maven guide](./references/maven.md)
- Docker: [Docker guide](./references/docker.md)
- Gradle: [Gradle guide](./references/gradle.md)

## Online Discovery
- Recipe catalog: https://docs.openrewrite.org/recipes
- YAML recipe format: https://docs.openrewrite.org/reference/yaml-format-reference

### YAML Option Wiring Pattern
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: org.openrewrite.recipes.MyComposite
displayName: My composite
description: Compose existing recipes for a focused modernization.
recipeList:
  - org.openrewrite.java.RemoveUnusedImports
  - org.openrewrite.java.UseStaticImport:
      methodPattern: java.util.Collections *(..)
```

## Java-Focused Execution Commands
- Gradle dry run and apply: `gradle rewriteDryRun` then `gradle rewriteRun`
- Maven dry run and apply: `mvn rewrite:dryRun` then `mvn rewrite:run`

## Pitfalls
- Do not create a new imperative recipe when composition in YAML is sufficient.
- Do not run large recipe sets at once without a dry run.
- Avoid vague option values; prefer explicit option wiring.
- Validate on a small scope before organization-wide rollout.

## Verification Checklist
- Recipe name resolves and exists.
- YAML syntax is valid (`type`, `name`, `recipeList`).
- Required options are supplied with correct keys.
- Dry run output is reviewed before applying changes.
- At least one focused test demonstrates expected before/after behavior.
