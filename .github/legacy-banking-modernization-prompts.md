# Legacy Banking Modernization Prompts

Use these prompts as copy-paste starting points when you want the agent to modernize `legacy-banking` in a deterministic OpenRewrite workflow.

## Notes
- Use `rewrite/` as the source of existing recipes, proven visitor patterns, and test examples.
- Use `rewrite-recipe-legacy-banking/` as the place to author the final banking-specific recipes and composite recipe definitions.
- Treat the current recipes in `rewrite-recipe-legacy-banking/` as starter templates, not as the recipe catalog to reuse.
- If a useful recipe already exists in `rewrite/` or is generally available through OpenRewrite, prefer composing it instead of reimplementing it.
- If no suitable recipe exists, write a new recipe specifically for `legacy-banking` in `rewrite-recipe-legacy-banking/`.
- **Final end state**: a single Maven command in `legacy-banking` that applies the complete modernization recipe set:
  - `cd legacy-banking && mvn org.openrewrite.maven:rewrite-maven-plugin:run -Drewrite.activeRecipes=dev.rabauer.LegacyBankingModernization`
  - This requires wiring `rewrite-recipe-legacy-banking` as a plugin dependency in `legacy-banking/banking-service/pom.xml`
- The recipe project in `rewrite-recipe-legacy-banking/` should contain all needed banking-specific recipes AND a composite recipe entry point that references both reused and custom recipes.
- Preserve the intentionally legacy runtime unless explicitly told otherwise: SOAP stays SOAP, Spring XML stays unless directly targeted, and direct JDBC is not removed casually.

## Prompt 1: Discover Reusable Recipes Before Writing Any New Ones

```text
Analyze `legacy-banking` and determine which existing OpenRewrite recipes should be reused before any custom recipe is written.

Requirements:
1. Inspect the code in `legacy-banking` and the plan files in `legacy-banking/plans/`.
2. Search `rewrite/` for existing recipes and proven implementation patterns relevant to the observed legacy code.
3. Prefer real reusable recipes from `rewrite/` or generally available OpenRewrite recipes over custom implementation.
4. Identify the gaps that still require a new recipe in `rewrite-recipe-legacy-banking`.

Output:
- reusable existing recipe IDs
- what each recipe would change in `legacy-banking`
- target files or patterns
- safe to apply now or not
- missing banking-specific recipe opportunities

Do not write code yet. I want a concrete modernization inventory, not generic advice.
```

## Prompt 2: Build A Deterministic Composite Recipe For A Safe First Pass

```text
Create a deterministic first-pass modernization for `legacy-banking` that can be applied with one Maven command.

Workflow:
1. Inspect `legacy-banking`.
2. Reuse existing recipes from `rewrite/` or other general OpenRewrite recipes wherever possible.
3. In `rewrite-recipe-legacy-banking`, remove or ignore starter-template recipes that are not part of the final modernization plan.
4. Create the final composite recipe definition in `rewrite-recipe-legacy-banking` that references the selected reusable recipes.
5. Write all needed banking-specific recipes in `rewrite-recipe-legacy-banking` (do not stop at one).
6. Add tests for all new custom recipes.
7. Build `rewrite-recipe-legacy-banking` with Gradle.
8. Publish it to Maven local.
9. Wire the published artifact into `legacy-banking/banking-service/pom.xml` as a rewrite-maven-plugin dependency.
10. Run a single Maven command to apply the complete composite recipe to `legacy-banking`.
11. Summarize the resulting refactoring and provide the exact Maven command for future reruns.

Constraints:
- Prefer low-risk modernizations.
- Do not migrate SOAP to REST.
- Do not replace the full Spring XML architecture.
- The final answer must be a single rerunnable Maven command that applies all changes.
```

## Prompt 3: Write All Missing Banking-Specific Recipes

```text
Identify ALL real legacy patterns in `legacy-banking` that need modernization, and write recipes for all gaps that existing `rewrite/` recipes do not cover.

Process:
1. Inspect `legacy-banking` source code and plan files to identify all modernization opportunities.
2. For each opportunity, search `rewrite/` first for an existing recipe or implementation pattern.
3. Categorize each opportunity as:
   a. Already covered by a reusable `rewrite/` recipe
   b. Requires a new banking-specific recipe in `rewrite-recipe-legacy-banking`
4. Implement ALL missing banking-specific recipes in `rewrite-recipe-legacy-banking` (do not write just one).
5. Keep each custom recipe minimal, composable, and deterministic.
6. Add complete `RewriteTest` coverage for all new recipes.
7. Create a composite recipe in `rewrite-recipe-legacy-banking` that orchestrates all reusable and custom recipes.
8. Build and publish the recipe project with Gradle.
9. Verify that one composite recipe entry point can be used in a `rewrite.activeRecipes` Maven property.
10. Report all recipes created, all recipes reused, the composite entry point, and the anticipated code changes.

Constraints:
- Aim to write multiple recipes if the modernization requires multiple independent transformations.
- Follow patterns from `rewrite/`.
- Use declarative YAML when that is sufficient; use imperative Java only when AST-aware logic is required.
- Build in a way that the final composite recipe produces deterministic, idempotent changes.
```

## Prompt 4: Replace The Template Catalog With Real Modernization Recipes

```text
Turn `rewrite-recipe-legacy-banking` from a template starter into the real recipe project for `legacy-banking` modernization.

Please:
1. Inspect the current starter recipes in `rewrite-recipe-legacy-banking`.
2. Determine which files are just template/demo content and no longer belong in the project.
3. Remove or replace that template content with recipes that are actually relevant to `legacy-banking`.
4. Reuse existing `rewrite/` recipes by composing them whenever possible.
5. Add only the missing banking-specific recipes needed for the modernization goal.
6. Produce one or more final recipe entry points that can be rerun deterministically.
7. Build and test the recipe project.

Deliverables:
- which template recipes were removed or replaced
- which existing `rewrite/` recipes were composed
- which new banking-specific recipes were added
- which final recipe IDs should be used to refactor `legacy-banking`
```

## Prompt 5: Dry-Run The Full Modernization Plan Before Editing

```text
Prepare a dry-run modernization plan for `legacy-banking` using `rewrite/` as the recipe source and `rewrite-recipe-legacy-banking` as the final authoring repo.

Do not edit files yet. Instead:
1. Inspect `legacy-banking` and its plan files.
2. Identify reusable existing recipes from `rewrite/`.
3. Identify what must be written specifically in `rewrite-recipe-legacy-banking`.
4. Propose the final deterministic recipe structure that should exist in `rewrite-recipe-legacy-banking`.
5. Split the plan into:
  - reuse directly from existing recipes
  - implement as new banking-specific recipes
  - not worth automating

I want exact recipe ideas and target files, not a vague strategy memo.
```

## Prompt 6: End-To-End Modernization Loop With Single Maven Command

```text
Execute one complete modernization loop for `legacy-banking` that results in a single Maven command applying all changes.

The workflow must be:
1. inspect `legacy-banking` and `legacy-banking/plans/`
2. identify all modernization opportunities
3. inspect relevant recipes and patterns in `rewrite/`
4. decide what can be reused directly and what needs banking-specific recipes
5. write ALL missing banking-specific recipes in `rewrite-recipe-legacy-banking`
6. create the final composite recipe entry point in `rewrite-recipe-legacy-banking`
7. add complete tests for all custom recipes
8. compile the recipe project with Gradle
9. publish the recipe jar to Maven local
10. wire the artifact into `legacy-banking/banking-service/pom.xml` as a rewrite-maven-plugin dependency
11. run the single Maven command to apply the complete modernization to `legacy-banking`
12. summarize the code changes, build results, and provide the exact Maven command for future reruns

The final deliverable must be:
- all recipes compiled and tested
- the recipe jar published
- the Maven pom.xml configured with the plugin dependency
- a working single-command invocation that applies everything
```

## Prompt 7: Improve A Weak Or Failing Banking Recipe

```text
The current banking modernization recipe is incomplete or weak. Fix it end-to-end.

Requirements:
1. Inspect the current recipe implementation in `rewrite-recipe-legacy-banking`.
2. Compare it against relevant implementations or patterns in `rewrite/`.
3. Identify whether the problem is wrong composition, poor matching, missing test coverage, or incorrect visitor logic.
4. Fix the recipe at the root cause.
5. Keep only the recipe pieces that belong to the real `legacy-banking` modernization.
6. Rebuild and reapply the recipe to `legacy-banking`.
7. Report what changed in the recipe project and what changed in the application.

Do not make unrelated cleanups.
```

## Prompt 8: Generate Recipes Directly From The Banking Plans

```text
Use `legacy-banking/plans/` as the source of truth and generate the real modernization recipes for one documented legacy concern.

Instructions:
1. Read the relevant plan files first.
2. Determine whether existing recipes from `rewrite/` already cover part of the concern.
3. Reuse those existing recipes where possible.
4. Implement only the missing banking-specific logic in `rewrite-recipe-legacy-banking`.
5. Create a final deterministic recipe entry point in `rewrite-recipe-legacy-banking`.
6. Add tests, build, and publish locally.
7. Apply the recipe to `legacy-banking`.
8. Report how the recipe maps back to the plan and how to rerun it later.

Do not choose a modernization that replaces the runtime architecture unless I explicitly request that.
```