```mermaid
flowchart TD
    A[Legacy Code] -->|generate with AI| B[OpenRewrite Recipes]
    B -->|apply recipes| D[Modified Code]
    D -->|review changes
     generate with AI| B
```