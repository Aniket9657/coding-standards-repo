# Coding Standards Repository

## Philosophy

These standards are inspired by the coding rules used in safety-critical systems engineering at NASA.
The goal is not stylistic perfection — it is **predictability, analyzability, maintainability, and fault prevention**.

The core idea is simple:

> Code should be easy for humans to reason about and easy for tools to verify.

These rules intentionally favor:

* deterministic behavior
* explicit control flow
* bounded execution
* small logical units
* aggressive validation
* mechanical verification over subjective preference

Some rules may initially feel restrictive, but the tradeoff is improved reliability, debuggability, and long-term maintainability.

---

# Core Coding Standards

## 1. Restrict Control Flow

Avoid:

* `goto`
* `setjmp`
* `longjmp`
* recursion

### Standard

Control flow should remain simple, explicit, and statically analyzable.

### Why

Acyclic call graphs are easier to:

* verify
* debug
* reason about
* analyze with static tooling

### Preferred

* iterative logic
* explicit state machines
* structured branching

---

## 2. Every Loop Must Have a Fixed Upper Bound

### Standard

All loops must have a provable maximum iteration count.

### Why

Prevents:

* infinite loops
* runaway execution
* unbounded resource consumption

### Preferred

```c
for (int i = 0; i < MAX_RETRIES; i++)
```

### Avoid

```c
while (true)
```

unless termination is formally guaranteed and documented.

---

## 3. No Dynamic Memory Allocation After Initialization

Avoid:

* `malloc`
* `calloc`
* `realloc`
* runtime heap allocation

### Standard

Memory should be allocated during startup/initialization only.

### Why

Eliminates:

* fragmentation
* leaks
* nondeterministic allocation failures
* unpredictable latency

### Preferred

* stack allocation
* static allocation
* object pools
* fixed-size buffers

---

## 4. Keep Functions Small

### Standard

Functions should fit on a single screen/page (~60 lines maximum).

### Why

Small functions:

* reduce cognitive load
* improve readability
* isolate responsibility
* simplify testing

### Preferred

Each function should represent:

* one logical task
* one level of abstraction

---

## 5. Use Assertions Aggressively

### Standard

Every function should contain meaningful assertions validating assumptions and invariants.

### Why

Assertions:

* catch invalid states early
* expose contract violations
* improve debugging
* document intent

### Preferred

```c
assert(ptr != NULL);
assert(length < BUFFER_SIZE);
```

---

## 6. Minimize Variable Scope

### Standard

Declare variables in the narrowest possible scope.

### Why

Smaller scope:

* reduces accidental coupling
* improves readability
* simplifies fault isolation
* minimizes unintended mutation

### Preferred

```c
for (int i = 0; i < size; i++)
```

instead of:

```c
int i;
```

declared far earlier than necessary.

---

## 7. Never Ignore Return Values Implicitly

### Standard

Every non-void return value must be:

* checked
* propagated
* explicitly ignored

### Preferred

```c
result = write_data();
if (result != SUCCESS)
```

or explicitly:

```c
(void)write_data();
```

### Why

Silent failure handling is one of the most common sources of production defects.

---

## 8. Limit Preprocessor Usage

### Standard

Use the preprocessor only for:

* header inclusion
* simple constants
* minimal macros

### Avoid

* complex macros
* token pasting
* macro metaprogramming
* deep conditional compilation

### Why

Heavy preprocessor logic:

* obscures intent
* complicates tooling
* breaks debuggability
* reduces readability

---

## 9. Restrict Pointer Complexity

### Standard

Limit pointer indirection to one level whenever possible.

Avoid:

* multi-level pointer chains
* function pointers unless unavoidable

### Why

Complex pointer structures:

* increase cognitive load
* complicate data flow analysis
* increase risk of undefined behavior

### Preferred

Simple, explicit ownership and access patterns.

---

## 10. Mandatory Compilation and Static Analysis

### Standard

Code must:

* compile with all warnings enabled
* produce zero warnings
* pass static analysis checks continuously

### Why

Warnings are defects waiting to happen.

### Recommended

* daily builds
* CI enforcement
* automated linting
* static analyzers
* sanitizers

---

# Engineering Principles

## Automation Over Opinion

Prefer rules that can be:

* enforced mechanically
* verified automatically
* checked consistently

Avoid subjective standards that depend entirely on reviewer interpretation.

---

## Predictability Over Cleverness

Readable and boring code is preferable to:

* clever abstractions
* hidden behavior
* implicit control flow
* magic metaprogramming

---

## Explicitness Over Convenience

Prefer:

* explicit ownership
* explicit errors
* explicit state transitions
* explicit lifetimes

Hidden behavior increases failure risk.

---

## Safety Over Brevity

Shorter code is not always better.

Correct, analyzable, maintainable code takes priority over terseness.

---

# Practical Interpretation

These standards are intentionally strict.

Not every project requires this level of rigor, but the philosophy scales extremely well to:

* embedded systems
* backend infrastructure
* distributed systems
* finance
* medical software
* aerospace
* high-reliability systems

Even partial adoption significantly improves code quality.

---

# Recommended Team Rules

## Build Rules

* Warnings treated as errors
* Mandatory formatting
* Mandatory linting
* CI required for merge

## Review Rules

* No hidden side effects
* No unnecessary abstraction
* No unexplained complexity
* Every unsafe operation documented

## Reliability Rules

* Fail fast
* Validate assumptions
* Prefer immutable data
* Prefer deterministic behavior

---

# Summary

The purpose of these standards is simple:

> Write code that is easy to reason about, easy to verify, and difficult to misuse.

Good engineering is not about writing the smartest code.

It is about building systems that continue working correctly under pressure, failure, maintenance, and scale.
