# Java Recipes (Portable Guide)

## Use This For
- Applying existing Java recipes without writing custom visitors.
- Running Java modernization and cleanup recipes in Maven or Gradle projects.

## Discover Recipes Online
- Recipe catalog docs: https://docs.openrewrite.org/recipes
- Java recipe docs: https://docs.openrewrite.org/recipes/java
- Static analysis recipes: https://docs.openrewrite.org/recipes/staticanalysis

## Typical Recipe Names
- `org.openrewrite.java.RemoveUnusedImports`
- `org.openrewrite.java.format.AutoFormat`
- `org.openrewrite.staticanalysis.UseDiamondOperator`

## Declarative Composition Pattern
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: com.example.JavaCleanup
displayName: Java cleanup
description: Apply common Java cleanup recipes.
recipeList:
  - org.openrewrite.java.RemoveUnusedImports
  - org.openrewrite.staticanalysis.UseDiamondOperator
```

## Run On Gradle Project
1. Configure the OpenRewrite Gradle plugin.
2. Set `activeRecipe` to the recipe name.
3. Run `gradle rewriteRun` (or `./gradlew rewriteRun`).

## Run On Maven Project
1. Configure the OpenRewrite Maven plugin.
2. Set `activeRecipes` to the recipe name.
3. Run `mvn rewrite:run`.

## Notes
- Prefer composing existing recipes first.
- Use imperative Java recipes only if declarative composition is insufficient.
