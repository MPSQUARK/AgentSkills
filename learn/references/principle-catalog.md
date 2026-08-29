# Principle Catalog

Category prompts for learn capture and `code-review` systemic pass. Categories only — no incident logs. New categories need user approval via `learn`.

| Category | Ask |
|---|---|
| **Reuse & discovery** | Did I search for existing solutions before writing new code? |
| **Concurrency & shared state** | Is mutable scratch/cache shared across threads or calls without an established pattern? |
| **Algorithmic correctness** | Is the algorithm correct for this domain, including edge cases and numerical stability? |
| **Structure & boundaries** | One responsibility per unit; justified file/class splits; right abstraction level? |
| **Review discipline** | Would I approve this in a strict review? Did I check adjacent/similar code? |
| **Invariants & contracts** | Are pre/postconditions explicit? Is threading/ownership/reentrancy documented where non-obvious? |
| **API consistency** | Does new API match sibling patterns (naming, errors, symmetry)? Is public surface minimal? |
| **Edge cases & failure modes** | Empty, zero, null, overflow, cancellation, errors — or explicitly out of scope with reason? |
