---
name: write-declarative-recipe
description: 'Author declarative OpenRewrite recipes in YAML. Use for composing existing recipes, preconditions, options wiring, and Java-centric validation.'
argument-hint: 'Recipe intent and target composition, for example: cleanup Java recipe tests and imports'
---

# Write Declarative OpenRewrite Recipe

## When To Use
- You can solve the task by composing existing recipes.
- You need preconditions and sequencing without custom visitor logic.
- You want a low-maintenance, YAML-first recipe definition.

## Decision Rule
- Choose declarative when you can express behavior with `recipeList`, options, and preconditions.
- Choose imperative only if you need custom AST logic or scan/edit coordination not achievable by composition.

## Procedure
1. Create a YAML recipe using `specs.openrewrite.org/v1beta/recipe`.
2. Use a fully qualified `name` and concise `displayName` and `description`.
3. Add `preconditions` only when you need to narrow applicability.
4. Add `recipeList` entries in deterministic order.
5. Pass options using nested mappings under each recipe item.
6. Test via `recipeFromYaml(...)` and `rewriteRun(...)` in focused unit tests.

## Minimal Template
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: org.openrewrite.recipes.MyDeclarativeRecipe
displayName: My declarative recipe
description: Compose existing recipes to apply a focused transformation.
preconditions:
  - org.openrewrite.FindSourceFiles:
      filePattern: "**/*.java"
recipeList:
  - org.openrewrite.java.RemoveUnusedImports
  - org.openrewrite.java.UseStaticImport:
      methodPattern: java.util.Collections *(..)
```

## Test Pattern In This Repo
Use the same pattern as `rewrite-core/src/test/java/org/openrewrite/config/DeclarativeRecipeTest.java`:

```java
rewriteRun(
  spec -> spec.recipeFromYaml("""
    type: specs.openrewrite.org/v1beta/recipe
    name: org.openrewrite.PreconditionTest
    description: Test.
    recipeList:
      - org.openrewrite.text.ChangeText:
          toText: 2
    """, "org.openrewrite.PreconditionTest"),
  text("1", "2")
);
```

## Important Behavior Notes
- Declarative recipes are initialized and executed through `DeclarativeRecipe` (`rewrite-core/src/main/java/org/openrewrite/config/DeclarativeRecipe.java`).
- Preconditions and nested scanning recipes are managed by the framework; keep YAML composition simple and explicit.
- Declarative orchestration is scan-first and composition-oriented; prefer imperative Java recipes when you need custom visitor-level control.
- Ensure names and descriptions follow `doc/adr/0002-recipe-naming.md`.

## Pitfalls
- Invalid or missing recipe names in `recipeList` cause initialization validation failures.
- Overly broad preconditions can hide expected transformations.
- Using declarative YAML for logic that needs custom AST inspection can lead to fragile compositions.

## Verification Checklist
- YAML contains required fields (`type`, `name`, `recipeList`).
- Recipe names in `recipeList` are valid and resolvable.
- Options map exactly to recipe option keys.
- A focused `recipeFromYaml` test verifies before/after behavior.
