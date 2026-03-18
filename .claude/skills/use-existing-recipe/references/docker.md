# Docker Recipes (Portable Guide)

## Use This For
- Applying existing recipes to Dockerfiles and container build practices.
- Enforcing base image and Dockerfile policy updates.

## Discover Recipes Online
- Docker recipe docs: https://docs.openrewrite.org/recipes/docker
- General catalog: https://docs.openrewrite.org/recipes

## Typical Recipe Names
- `org.openrewrite.docker.ChangeBaseImage`
- `org.openrewrite.docker.UpgradeParentImage`

## Declarative Pattern
```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: com.example.DockerBaseImagePolicy
displayName: Docker base image policy
description: Standardize Docker base images.
recipeList:
  - org.openrewrite.docker.ChangeBaseImage:
      oldImageName: eclipse-temurin
      newImageName: eclipse-temurin
```

## Execution
- Gradle projects: `gradle rewriteRun` (or `./gradlew rewriteRun`)
- Maven projects: `mvn rewrite:run`

## Notes
- Validate that the target image/tag exists before applying at scale.
- Run on representative services first, then roll out broadly.
