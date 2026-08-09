# Chapter 1: What is OOP?

## Core Idea
Object-oriented design is fundamentally about understanding the principles underlying design patterns, not memorizing patterns themselves. OOP's real power lies in achieving maintainability and reusability by organizing code around data structures, not processes.

## Frameworks Introduced
- **Principles vs. Patterns**: Design patterns are like chess openings (defined best practices), but they emerge from deeper principles. Understanding the principles lets you design correctly without memorizing patterns.
- **Data Structure as the Center**: Classes exist to unify data structures with the operations that work on them. The data structure is what distinguishes one class from another—not the business processes they handle.

## Key Concepts
- **Encapsulation**: Bundling data (attributes) with the methods that operate on them; avoiding scattered processing
- **Polymorphism**: Ability to call methods with the same name but different implementations across different classes
- **Inheritance**: Reusing data structures from parent classes in child classes (is-a relationship)
- **Data Structure Focus**: The fundamental characteristic that determines whether two classes should be separate
- **Impedance Mismatch**: The tension between OOP design and relational databases, which can pull developers back toward procedural code
- **Class Diagram Relations**: Association (independent mutual reference), Realization (interface → class), Generalization (is-a inheritance)
- **Responsibility vs. Role**: Responsibility describes what a class *does*, but multiple different classes can share the same responsibility; roles are interfaces, not classes

## Mental Models
- Use classes to organize around *data* when that data has boundaries. Think of what information needs to be grouped together.
- Think of interfaces as *roles* that multiple classes can play. A Person can take the role of Employee, Customer, or GymMember—but the person is the real entity.
- Use inheritance when data structures have an is-a relationship: a Wagon *is a* Vehicle (shares Vehicle's data structure plus adds more).

## Anti-patterns
- **Focusing on Responsibility Over Data**: Extracting classes based on "what the customer needs" (processing) instead of identifying distinct data structures. This leads back to procedural code.
- **Confusion of Role and Class**: Treating business responsibilities as classes. Employee and GymMember are roles the same person plays, not separate data structures.
- **Inheritance Without Data Extension**: Using inheritance to add only methods, not attributes. This violates the principle that data structure is what makes classes distinct.

## Worked Example
Consider a system where you notice a requirement: "The system must manage employees and gym members." You might initially create separate Employee and GymMember classes inheriting from Person, with shared methods like eat(). But when you ask "what data distinguishes a Person as an Employee vs. a GymMember?"—there's no answer. Both are roles the same person plays. The correct design makes Person the class (with data like name, birthDate) and creates Employee and GymMember *interfaces* that Person can implement. This way, the same person object can fulfill either role without inheriting conflicting behaviors from a shared parent.

## Key Takeaways
1. Data structure defines whether classes should be separate—not the business processes they perform
2. Classes must unify data with the operations on that data; separating them leads to scattered, unmaintainable code
3. Responsibilities and roles belong in interfaces, not classes
4. OOP requires designing around the *state* a system needs to track, which initially feels harder than following requirements, but pays off in maintenance and reuse
5. Understand the *why* behind patterns so you can apply principles correctly in novel situations

## Connects To
- **Ch 2**: Small-grain classes extend this principle by showing which data structures are worth creating as separate classes
- **Ch 3**: Functions support small-grain classes when they perform operations that span multiple small data types
- **Ch 4**: Method placement rules ensure operations land in the right class when multiple classes are involved
- **Ch 5**: Interfaces enable polymorphism by defining roles abstractly before committing to specific class implementations
