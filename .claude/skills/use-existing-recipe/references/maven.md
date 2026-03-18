# Maven Recipes (Portable Guide)

## Use This For
- Running existing OpenRewrite recipes in Maven builds.
- Applying dependency, plugin, and POM migration recipes.

## Discover Recipes Online
- Maven recipe docs: https://docs.openrewrite.org/recipes/maven
- General recipe catalog: https://docs.openrewrite.org/recipes

## Minimal Execution Flow
1. Add the OpenRewrite Maven plugin to `pom.xml`.
2. Add the recipe dependency set containing your target recipe (if required).
3. Set the active recipe(s).
4. Run:
- `mvn rewrite:discover`
- `mvn rewrite:dryRun`
- `mvn rewrite:run`

## Typical Recipe Names
- `org.openrewrite.maven.UpgradeDependencyVersion`
- `org.openrewrite.maven.ChangeDependencyGroupIdAndArtifactId`
- `org.openrewrite.maven.RemoveRedundantDependencyVersions`

## Option Wiring Example
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: com.example.UpgradeGuava
displayName: Upgrade Guava
description: Upgrade Guava in Maven projects.
recipeList:
  - org.openrewrite.maven.UpgradeDependencyVersion:
      groupId: com.google.guava
      artifactId: guava
      newVersion: 33.x
```

## Notes
- Prefer `rewrite:dryRun` before `rewrite:run` in CI pipelines.
- Keep recipe options explicit for deterministic behavior.
