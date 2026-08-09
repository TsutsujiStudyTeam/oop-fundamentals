# Patterns & Techniques: OOP Design

## UML Class Relationships

### Association (関連)
**When to use**: When two classes are independent and need bidirectional reference
- Independent objects that collaborate
- Parts can be used in different contexts (e.g., tires across multiple bikes)

**How**: Draw a line with arrows on both ends; include cardinality
- Specify how many instances can relate (1:1, 1:many, many:many)

**Why it matters**: The cardinality IS the model — without it, you lose business context

---

## Realization / Interface Implementation (実現)

### Interface-based Design
**When to use**: When multiple classes need to fulfill the same "role"
- Define behavior contract without prescribing implementation
- Support multiple types through a common interface

**How**: 
1. Define interface with method signatures only
2. Multiple classes implement the same interface
3. Clients code against the interface, not concrete classes

**Example**: Vehicle interface → Car and Airplane both implement it
- Interface defines "role" (what it does)
- Concrete classes implement "how it does it"

**Trade-offs**: 
- Adds abstraction layer
- Enables flexibility and polymorphism
- Prevents coupling to concrete implementations

---

## Generalization / Inheritance (汎化)

### When to Use Inheritance
**Pattern**: "is a" relationship for true specialization
- Wagon and Sedan "is a" Automobile
- Child class inherits all properties of parent

**How**: 
1. Extract common properties into parent class
2. Child inherits and adds specialization
3. Can override parent behavior where needed

**Anti-pattern**: Using inheritance for reuse when composition would be better
- Inheritance creates tight coupling
- Can't change parent without affecting all children
- "Favor composition over inheritance"

---

## What NOT to Use: Aggregation (集約)

**Why aggregation fails**: 
- Placebo effect (Jim Rumbaugh, one of UML's founders, said so)
- Creates false distinction (diamond points to "owner" in has-a relationships)
- In practice, related classes refer to each other bidirectionally
- Cannot meaningfully distinguish which direction the "ownership" goes

**Better alternative**: Use Association instead
- Simpler, clearer semantics
- No false distinction of ownership

---

## Design Principle: Start with Data Structure, Not Behavior

**Pattern**: Data-first design approach
**When to use**: Always — it's the foundation of proper OOP

**How**:
1. Customer requests behavior (features, processing)
2. Identify the DATA structure needed to support that behavior
3. Define classes that encapsulate related data and methods
4. Methods operate on the class's own data, not external

**Why it works**: 
- Behavior changes frequently
- Data structure changes are rare
- When you get data structure right, behavior flows from it

**Anti-pattern**: Behavior-first design
- Results in generic utility classes with static methods
- Creates separation between data and processing
- Reverts to procedural programming despite OOP language

---

## Principle: Small-grain Classes for Reusability

**Pattern**: Design classes to be reusable across contexts
**When to use**: When maximizing code reuse and reducing duplication

**How**: Make classes small and focused
- One primary responsibility
- Can be used independently or combined
- Think of as "building blocks"

**Example**: Instead of building application-specific classes, build small generic classes that can be reused in multiple applications

**Trade-off**: Requires more thought upfront but pays off in maintenance and reuse

---

## Principle: Method Implementation Location (2 Rules Only)

### Rule 1: Place Method in the Class That Owns the Data
**When to apply**: For any method that needs data
- If data is in Class A, method goes in Class A
- This keeps data and behavior together (cohesion)

### Rule 2: Only 2 Cases for Where Data Belongs
1. **Private instance data**: Only the class can access
2. **Shared data structure**: Accessed by multiple classes
   - NOT private methods — use public/protected

**Trade-off**: 
- Breaks traditional encapsulation (privateMethod anti-pattern)
- Gains: higher cohesion, simpler code, true object-orientation

**Anti-pattern**: Private methods that hide data
- Data is hidden but behavior is exposed
- Creates accidental proceduralism within an OOP context

---

## Functions vs. Methods: Clear Boundaries

**Rule**: Use functions only for stateless operations
**When to use functions**: 
- No access to instance state needed
- Pure calculation or transformation
- Utility operations

**When to use methods**: Everything else

**Anti-pattern**: Utility classes with static functions
- "Forbidden!" according to this design philosophy
- Forces thinking about relationships and data structure
- Prevents lazy procedural thinking

---

## Interface Hierarchies vs. Simple Interfaces

**Pattern**: Use interfaces to represent roles and contracts
**How to design**: 
- One interface per "role" or behavior contract
- Classes implement one or more interfaces as needed
- Allows fine-grained polymorphism

**Why not aggregation**: 
- Interfaces already capture "has-a" through method contracts
- No need for separate aggregation symbol
- Cleaner model
