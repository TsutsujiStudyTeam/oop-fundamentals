---
name: oop-fundamentals
description: "Knowledge base of pragmatic Object-Oriented Programming principles from Japanese OOP framework development. Use when designing classes, understanding data structure vs. behavior, applying the 2-principle method placement rule, choosing between inheritance/composition/interfaces, or studying core OOP mental models."
---

<!-- argument-hint: [chapter number, framework name, or OOP principle] -->

# Object-Oriented Programming Fundamentals (Japanese Context)

**Source**: "本には書いていないオブジェクト指向" (Object-Oriented Programming Not in the Books)  
**Focus**: Pragmatic principles from Java framework design experience  
**Chapters**: 5 | **Generated**: 2026-08-09

## How to Use This Skill

- **Without arguments** — Load core OOP principles and decision frameworks
- **With a chapter number** — Ask for `ch01` through `ch05`; I load that chapter's frameworks and worked examples
- **With a topic** — Ask about `small-grain classes`, `method placement`, `inheritance`, or any OOP principle; I find and explain the relevant chapter
- **Browse structure** — Ask "what chapters do you have?" or "index"

When you ask about a topic not listed below, I read the relevant chapter and answer from first principles.

---

## Core OOP Principles & Frameworks

### 1. Data Structure is the Foundation
The single most important principle: **Classes exist to organize around data structures, not business processes.**

**Why this matters**: 
- Customers request *behavior*, but behavior changes frequently
- Data structures remain stable across system evolution
- When data structure is right, behavior flows from it naturally

**How to apply**:
- Before designing classes, ask: "What data must exist for this feature?"
- Group data that naturally belongs together
- Co-locate methods with the data they manipulate
- Avoid separating data from behavior (which leads back to procedural programming)

**Anti-pattern**: Behavior-first design
- "The customer needs feature X"
- → Create methods for feature X
- → Scatter those methods across utility classes
- → Result: procedural code in an OOP language

---

### 2. Principles Over Patterns
Design patterns are *derived from* underlying principles—they're useful shortcuts, not laws.

**Key metaphor**: Patterns are like Go "joseki" (定石 — standard board positions)
- Joseki are useful; experienced players memorize them
- But behind every joseki is a strategic *principle*
- Understanding the principle lets you handle novel situations

**How to apply**:
- Learn the 5 core principles (data structure, small-grain classes, method placement, etc.)
- Understand *why* each pattern works
- When facing a new design situation, apply principles to derive the right solution
- Don't memorize patterns; memorize why they exist

---

### 3. Small-Grain Classes for Reusability
Create very small, focused classes representing fundamental data types and concepts.

**Examples**: Amount, Quantity, Date, PersonName, Currency
- These classes are reusable across projects and decades
- They carry both data and operations (formatting, validation, calculation)
- They solve the "primitive type everywhere" problem

**Key benefit**: Single point of change
- Change how amounts are formatted? Modify Amount.getFormattedDisplay() once
- All 50 places that use amounts automatically see the change

**Design rule**: Extract a small-grain class when you identify a concept that appears as data in multiple larger classes

---

### 4. The 2 Principles of Method Implementation Location
**Only 2 principles govern where methods should live.** All complex design questions reduce to these binary decisions.

#### Principle 1: Hierarchical Containment
When classes have a header-detail relationship (Order contains OrderLines):
- Implement methods in the topmost class
- Delegate downward to preserve encapsulation
- Never expose child structure upward

**Why**: Changes to storage (array → List → Map) don't break parent code

#### Principle 2: Mediator Pattern for Peers
When classes are independent (Receipt and Inventory):
- Create a third class (ReceiptInventory) to coordinate
- This prevents tight coupling
- Changes to one domain don't ripple to another

**Apply these consistently and your design will be maintainable.**

---

### 5. Data Exposure, Not Method Hiding
**Controversial rule**: Make data public/protected; don't hide it with private methods.

**Rationale**:
- Private methods hide both data AND behavior
- Results in "encapsulation" that's actually just scattered procedural code
- Public data + private methods is worse than public both

**Better approach**:
- Keep only 2 locations for data: (1) private instance data OR (2) shared public/protected data
- Let multiple classes access shared data if needed
- Put methods where they naturally belong (with the data they use)

**This breaks traditional OOP dogma but produces cleaner, more maintainable code.**

---

### 6. Interfaces Define Roles, Not Hierarchies
An interface is a *role* multiple classes can play, not a data structure.

**Key difference**:
- **Inheritance** (is-a): Wagon *is a* Vehicle (shares data structure)
- **Interface** (can-do): Vehicle role — Car, Bicycle, Airplane can all play it

**Design rule**: 
- Start with concrete classes
- Observe what they have in common (roles they could play)
- Extract the interface
- Never design an interface before concrete classes exist

**Why**: Interfaces designed in isolation miss essential operations or include irrelevant ones

---

### 7. Avoid These Design Anti-patterns

| Anti-pattern | Why It Fails | Better Approach |
|---|---|---|
| **Utility classes** (`StringUtils.join()`) | Procedural thinking, breaks data cohesion | Put method in the class that owns the data |
| **Inheritance for reuse** | Tight coupling, can't change parent | Use composition or extract true "is a" |
| **Aggregation in UML** | False semantics (ownership isn't meaningful) | Use association; it's simpler and clearer |
| **Private methods hiding data** | Accidental proceduralism in OOP | Make data accessible to classes that need it |
| **Interface-first design** | Misses real requirements | Start with concrete classes; interface emerges |
| **Ignoring impedance mismatch** | Forces procedural code due to DB structure | Embrace the mismatch; don't let it dictate OOP design |

---

### 8. Why OOP Is Hard: 3 Barriers

1. **Customers request behavior, not data**
   - You must infer the data structure
   - Takes more upfront thought than procedural design
   - But pays dividends in maintenance

2. **OOP languages allow procedural coding**
   - Java/C++ syntax supports static methods, utility classes, primitives everywhere
   - Discipline required to maintain OOP discipline
   - No language enforcement; it's a design choice

3. **Relational databases don't match OOP**
   - Impedance mismatch: tables ≠ objects
   - O/R mappers can hide or exacerbate the mismatch
   - Must consciously design around this tension

---

### 9. The Core Value Proposition

| Phase | OOP | Procedural |
|-------|-----|-----------|
| **Initial coding** | Slower (more thought) | Faster (write quickly) |
| **First bug fix** | Fix in one place; all fixed | Fix multiple places |
| **Requirements change** | Extend cleanly; reuse works | Refactor everywhere |
| **Code reuse** | Classes work in new contexts | Heavy adaptation needed |
| **Long-term productivity** | ↑↑ (accumulating benefit) | ↓ (accumulating debt) |

**Break-even point**: Typically the first major maintenance cycle or new feature that requires code reuse.

---

### 10. UML Relationships at a Glance

| Relationship | When | Example |
|---|---|---|
| **Association** | Mutual reference between independent classes | Bike ↔ Tires (tires can be used elsewhere) |
| **Realization** | Class implements an interface | Car implements Vehicle role |
| **Generalization** | True "is a" with shared data | Sedan is-a Automobile |
| **Aggregation** | ❌ Don't use — it's a "placebo effect" | Use association instead |

**Critical**: Always specify cardinality in associations (1, 0..1, *, 1..*). The cardinality IS the model.

---

## Chapter Index

| Chapter | Title | Core Frameworks | Key Takeaway |
|---------|-------|---|---|
| [ch01](chapters/ch01-what-is-oop.md) | What is OOP? | Principles vs. Patterns, Data as foundation, Role vs. Class, UML relationships | Learn principles, not patterns |
| [ch02](chapters/ch02-small-grain-classes.md) | Small-Grain Classes for Reusability | Atomic data types, Numeric hierarchy, Single point of change | Design reusable foundational classes |
| [ch03](chapters/ch03-using-functions-effectively.md) | Using Functions Effectively | Function extraction rules, Implementation abstractions, Utility class dangers | Extract functions only for small-grain classes |
| [ch04](chapters/ch04-method-implementation-location.md) | Method Implementation Location (2 Principles) | Hierarchical containment, Mediator pattern, Sequence diagrams | Two principles govern all placement decisions |
| [ch05](chapters/ch05-interfaces-as-polymorphism.md) | Interfaces as Polymorphism | Concrete-to-abstract design, Roles vs. inheritance, Polymorphic behavior | Design interfaces from concrete classes |

---

## Topic Index

- **Abstraction** → ch01, ch05
- **Anti-patterns** → ch01, ch02, ch03, ch04, ch05
- **Association (UML)** → ch01
- **Behavior vs. Data** → ch01, ch02, ch03, ch04
- **Cardinality** → ch01
- **Class relationships** → ch01, ch04, ch05
- **Cohesion** → ch02, ch04
- **Composition** → ch02, ch04, ch05
- **Concrete-to-abstract** → ch05
- **Data structure design** → ch01, ch02, ch03, ch04
- **Decision trees** → cheatsheet
- **Design patterns** → ch01, ch02, ch03, ch04, ch05
- **Encapsulation** → ch01, ch03, ch04
- **Functions** → ch03
- **Generalization** → ch01, ch05
- **Impedance mismatch** → ch01, ch03
- **Inheritance** → ch01, ch04, ch05
- **Interfaces** → ch01, ch05
- **Method placement** → ch04
- **Polymorphism** → ch01, ch05
- **Principles** → ch01, ch02, ch03, ch04, ch05
- **Realization** → ch01, ch05
- **Reusability** → ch02, ch03, ch04, ch05
- **Roles** → ch01, ch05
- **Small-grain classes** → ch02, ch03, ch04, ch05
- **Utility classes** → ch01, ch03

---

## Supporting Files

- [glossary.md](glossary.md) — 25+ key terms with Japanese context and chapter references
- [patterns.md](patterns.md) — 8 design patterns and techniques with decision rules and examples
- [cheatsheet.md](cheatsheet.md) — Quick reference tables, decision trees, anti-patterns, and design checklist

---

## Quick Start: Ask Me About

**To apply OOP right now:**
- "How do I design classes for [my feature]?"
- "Should I use inheritance or interfaces here?"
- "Where should this method go?"
- "What's wrong with utility classes?"

**To understand a concept:**
- "Explain small-grain classes"
- "What's the impedance mismatch?"
- "Why is encapsulation wrong when it hides data with private methods?"

**To reference frameworks:**
- "Give me the 2 method placement principles"
- "Index" → full topic index
- "ch03" → Chapter 3: Using Functions Effectively

---

## Scope & Limits

This skill covers OOP design principles and pragmatic patterns grounded in Java framework development experience. It's best used when:
- ✓ Designing class hierarchies or deciding between inheritance/composition/interfaces
- ✓ Understanding *why* design patterns work (principles over memorization)
- ✓ Applying data-structure-first thinking to new systems
- ✓ Resolving design conflicts (method placement, data exposure, etc.)
- ✓ Learning OOP from first principles rather than pattern catalogs

Use alongside:
- Your language's standard library docs (syntax)
- Project-specific requirements (business context)
- Code review conversations (feedback and alternatives)

---

## About the Source

This skill is synthesized from "本には書いていないオブジェクト指向" (Object-Oriented Programming Not in the Books), a Japanese resource based on real Java framework development experience. The author emphasizes:
- Pragmatism over dogma
- Principles over patterns
- Data structure as the foundation
- Practical design rules learned in production systems

The material is especially valuable for those who use OOP languages daily but haven't studied OOP formally from first principles.
