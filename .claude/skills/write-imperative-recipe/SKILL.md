---
name: write-imperative-recipe
description: 'Author imperative OpenRewrite recipes in Java. Use for custom AST transformations, Recipe/Visitor implementation, option validation, and RewriteTest coverage.'
argument-hint: 'Target language, transformation goal, and matching strategy'
---

# Write Imperative OpenRewrite Recipe

## When To Use
- You need custom AST traversal or transformation logic.
- Existing recipes cannot be composed to achieve the behavior.
- You need precise matching, validation, or type-aware edits.

## Inputs
- Language and module (`rewrite-java` for Java recipes).
- Target AST elements and matching strategy.
- Optional parameters exposed as recipe options.

## Procedure
1. Create a recipe class extending `Recipe` in the appropriate module package.
2. Add recipe options with `@Option` and clear metadata.
3. Implement `getVisitor()` and optionally preconditions with `Preconditions.check(...)`.
4. Add `validate()` when option constraints are non-trivial.
5. Keep transformations type-safe and return unchanged trees when no match applies.
6. Add a focused `RewriteTest` class with representative before/after coverage.

## Java Reference Patterns In This Repo
- Parameterized recipe with options and preconditions:
  `rewrite-java/src/main/java/org/openrewrite/java/ChangeType.java`
- Recipe with explicit `validate()` and method-pattern checks:
  `rewrite-java/src/main/java/org/openrewrite/java/ChangeMethodName.java`
- RewriteTest pattern:
  `rewrite-java-test/src/test/java/org/openrewrite/java/ChangeTypeTest.java`

## Imperative Recipe Skeleton
```java
@Value
@EqualsAndHashCode(callSuper = false)
public class MyRecipe extends Recipe {

    @Option(displayName = "Target type", description = "Type to update.", example = "com.example.Legacy")
    String targetType;

    String displayName = "My recipe";
    String description = "Apply a custom Java transformation.";

    @Override
    public Validated<Object> validate() {
        return super.validate();
    }

    @Override
    public TreeVisitor<?, ExecutionContext> getVisitor() {
        return new JavaIsoVisitor<ExecutionContext>() {
            // implement specific visit methods
        };
    }
}
```

## Test Skeleton
```java
class MyRecipeTest implements RewriteTest {
    @Override
    public void defaults(RecipeSpec spec) {
        spec.recipe(new MyRecipe("com.example.Legacy"));
    }

    @Test
    void rewritesExpectedCode() {
        rewriteRun(
          java("class A {}", "class A {}")
        );
    }
}
```

## Java-Focused Commands
- `./gradlew :rewrite-java:test`
- `./gradlew :rewrite-java-test:test --tests "org.openrewrite.java.ChangeTypeTest"`
- `./gradlew test --tests "*MyRecipeTest"`

## Pitfalls
- Missing `@Option` metadata reduces recipe discoverability.
- Overly broad visitors can create unintended edits.
- Skipping targeted tests makes regressions hard to detect.

## Verification Checklist
- Recipe has clear `displayName` and `description`.
- Options are validated when needed.
- Visitor only changes intended nodes.
- Tests cover positive and negative cases.
