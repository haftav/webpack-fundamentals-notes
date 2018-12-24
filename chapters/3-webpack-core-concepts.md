[Table of Contents](../README.md)

## 3. Webpack Core Concepts

### Entry

- Conceptually, the entry point is the first file to 'kick-off' your app.
- Webpack uses this as the starting point
- We can define this using an entry property in the config

### Output

- After webpack creates a bundle from our dependency graph, it needs a name for the output file and a place to put it
- The output property tells webpack _where_ and _how_ to distribute bundles

### Loaders & Rules

- How do we want to treat files that aren't JS?
    - We can define rules (rulesets) to treat files that match a certain filetype
    - These rules allow you to transform files before they're added to your dependency graph, based on the loaders provided
    - Loaders are javascript modules that take the source file and return it in a modified state
    - rulesets need (at minumum) two properties:
        - test
            - A regex expression that can be used to match certain filetype extensions
        - use
            - The loader, or loaders to apply when a filetype is matched in the test property
    - This testing of rulesets is a per-file process; it's not something that just happens in bulk
