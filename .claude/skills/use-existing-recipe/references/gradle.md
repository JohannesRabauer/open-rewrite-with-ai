# Gradle Recipes (Portable Guide)

## Use This For
- Applying existing recipes to Gradle build files and dependency declarations.
- Standardizing plugin versions and build conventions.

## Discover Recipes Online
- Gradle recipe docs: https://docs.openrewrite.org/recipes/gradle
- General recipe catalog: https://docs.openrewrite.org/recipes

## Minimal Execution Flow
1. Add/configure OpenRewrite Gradle plugin.
2. Add recipe dependencies if the recipe is not bundled already.
3. Set active recipe(s).
4. Run:
- `gradle rewriteDiscover`
- `gradle rewriteDryRun`
- `gradle rewriteRun`

## Typical Recipe Names
- `org.openrewrite.gradle.UpgradeDependencyVersion`
- `org.openrewrite.gradle.UpdateJavaCompatibility`
- `org.openrewrite.gradle.ChangePlugin`

## Option Wiring Example
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: com.example.GradleDependencyUpgrade
displayName: Gradle dependency upgrade
description: Upgrade a Gradle dependency version.
recipeList:
  - org.openrewrite.gradle.UpgradeDependencyVersion:
      groupId: org.slf4j
      artifactId: slf4j-api
      newVersion: 2.x
```

## Notes
- Use `rewriteDryRun` to review proposed changes before applying.
- Keep recipe selection small and targeted per run for easier review.
