# {{Module}} design

## 1. Summary

{{One paragraph: what this module does and what it explicitly does not do. Its conceptual role in the larger system.}}

## 2. Submodules and interactions

{{If the module has internal structure, list each submodule with a one-line role and describe how they interact. Omit this section if the module is flat.}}

### 2.1 {{Submodule}}

- Role: ...
- Interacts with: ...

## 3. Data model

{{Database schema or key persistent structures at a conceptual level — entity names, relationships, key fields. Not a full SQL DDL. Omit if the module has no persistent state.}}

## 4. Key algorithms

{{Brief pseudocode for any non-obvious algorithm. Only the ones a reviewer needs to understand the design. Omit if the module is straightforward.}}

```
{{pseudocode}}
```

## 5. Testing approach

{{How this module should be tested: what behaviors to cover, what edge cases matter, what to mock vs. use real. Conceptual — not a list of test cases.}}
