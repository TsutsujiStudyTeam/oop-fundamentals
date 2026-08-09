# Cheatsheet: OOP Fundamentals Quick Reference

## Core Decision Tree: Is This Proper OOP?

```
Does your code focus on DATA STRUCTURE?
  ├─ YES → Good OOP foundation
  └─ NO → Ask "What data does this behavior need?"

Is the method in the class that OWNS that data?
  ├─ YES → Method location is correct
  └─ NO → Move method to data owner (Rule 1)

Are you using utility classes or static methods extensively?
  ├─ YES → You're doing procedural programming in an OOP language
  └─ NO → Good — you're thinking about object relationships

Can you change requirements in ONE PLACE and fix all occurrences?
  ├─ YES → Good maintainability and reusability
  └─ NO → Your data structure needs rethinking
```

---

## 2 Principles for Method Implementation Location

| Situation | Where Method Goes | Why |
|-----------|------------------|-----|
| Method needs instance data | In the class that owns the data | Encapsulation + cohesion |
| Multiple classes need same data | Public/protected data, all classes can access | Avoid private methods hiding data |
| No instance data needed | As a static function, not a utility method | Keep it separate from objects |

**Key rule**: There are only 2 valid locations for data:
1. Private instance data (only that class accesses)
2. Shared data structure (multiple classes access via public/protected)

---

## When to Use Each UML Relationship

| Relationship | Symbol | When to Use | Example |
|---|---|---|---|
| **Association** | Line with arrows both ways | Independent classes that reference each other | Bike body ↔ Tires |
| **Realization** | Dashed line → interface | Class implements an interface | Car implements Vehicle interface |
| **Generalization** | Triangle pointing up | "is a" inheritance | Sedan is a Automobile |
| **Aggregation** | Diamond at owner | ❌ DON'T USE — causes confusion | (avoid this) |

**Remember**: Don't use aggregation. It's a "placebo effect." Use association instead.

---

## Inheritance vs. Composition vs. Interface

| Pattern | Usage | Trade-off | Example |
|---------|-------|-----------|---------|
| **Inheritance** | True "is a" relationship | Creates tight coupling, hard to change parent | Wagon is a Automobile |
| **Composition** | "has a" relationship, object contains another | More flexible, but more boilerplate | Car has Engine |
| **Interface** | Contractual relationship, defines role | No implementation, multiple can be implemented | Vehicle (interface) |

**Rule**: Prefer composition over inheritance unless true "is a" relationship exists

---

## Why OOP Is Hard: 3 Core Barriers

| Problem | Why It's Hard | Solution |
|---------|---------------|----------|
| **Customer requests behavior, not data structures** | You have to think about data structure that enables the behavior | Ask: "What data must exist for this behavior?" |
| **OOP languages let you write procedurally** | Java/C++ syntax supports static methods and procedural style | Discipline: ban utility classes, ban private methods, ban static functions for business logic |
| **RDBs are fundamentally mismatched with OOP** | Impedance mismatch: tables ≠ objects | Use proper O/R mapper or embrace mismatch in design; don't let it force procedural code |

---

## Anti-patterns to Avoid

### 1. Encapsulation via Private Methods (❌ Don't)
**Problem**: 
```
class Account {
  private data
  private calculateInterest() { ... }  // Hidden
}
```
Hides both data AND behavior. Results in procedural code in OOP context.

**Fix**: Make data public/protected, put methods in right class

---

### 2. Utility Classes with Static Methods (❌ Don't)
**Problem**: 
```
class StringUtils {
  static String capitalize(String s) { ... }
}
```
Forces procedural thinking, breaks object relationships, hides data dependencies

**Fix**: Ask what object should own this behavior

---

### 3. Inheritance for Reuse (❌ Don't)
**Problem**: 
```
class Employee extends Person { }  // Wrong: just for reuse
```
Creates tight coupling, hard to change, breaks Liskov substitution

**Fix**: Use composition or extract true "is a" relationship

---

### 4. Ignoring Data Structure (❌ Don't)
**Problem**: Focus entirely on behavior patterns
**Fix**: **Always start with data structure.** "1箇所直せばみな直る" (Fix it in one place, everything is fixed)

---

## The Core Value Proposition

**Upfront**: OOP takes more time initially (lower productivity coding)

**Long-term**: When requirements change or reusing code:
- ✓ Single point of change (fix data structure once)
- ✓ High maintainability (changes propagate correctly)
- ✓ High reusability (code works in new contexts)
- ✓ Total productivity much higher

**Break-even**: Typically by first major maintenance cycle

---

## Design Thinking: From Principle to Pattern

```
Principles (原理原則)
    ↓
    Based on fundamental OOP rules and reasoning
    ↓
Patterns (パターン)
    ↓
    Named solutions applying the principles
    ↓
Think of patterns as Go "joseki" (定石 — standard moves)
    ↓
Understanding principles lets you create new patterns
```

**Key insight**: Learn principles, not just patterns.
- Patterns are useful shortcuts
- Principles let you innovate

---

## UML Cardinality Quick Reference

| Notation | Meaning | Example |
|----------|---------|---------|
| `1` | Exactly one | 1 Bike body |
| `0..1` | Zero or one | 0..1 spare tire |
| `*` or `0..*` | Zero or many | * Tires per bike |
| `1..*` | One or many | 1..* Wheels per bike |
| `1..3` | Specific range | 1..3 headlights |

**Critical**: Always specify cardinality in association diagrams

---

## Decision Rules (When in Doubt)

1. **"Where should this method go?"** → In the class that owns the data it uses
2. **"Should I use inheritance?"** → Only if true "is a" relationship; else use composition
3. **"Should I hide this?"** → No; expose data structure, keep it consistent
4. **"Is this proper OOP?"** → Can you change requirements in one place?
5. **"Should I create a utility class?"** → No; it's forbidden. Find the right object to own the behavior.

---

## Quick Principle Checklist

- [ ] Data structure designed first (before methods)
- [ ] One primary responsibility per class (cohesion)
- [ ] Methods in class that owns the data
- [ ] Only 2 types of data: private instance or public shared
- [ ] No utility classes with static functions
- [ ] No aggregation — use association instead
- [ ] No inheritance unless true "is a"
- [ ] Can fix bugs in one place → everything fixed

**If you check all these: You're doing proper OOP**
