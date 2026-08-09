# Chapter 5: Interfaces as Polymorphism

## Core Idea
Interfaces define roles (responsibilities) that multiple concrete classes can fulfill. But interfaces must be discovered by first examining concrete classes—design goes from concrete to abstract, while implementation goes from abstract to concrete.

## Frameworks Introduced
- **Polymorphism**: The ability to write code that treats objects via a common interface, allowing different classes to respond to the same method call in their own way.
- **Interface as Role**: An interface is not a class; it defines a contract ("what can you do?") without dictating *what you are*. Multiple classes play the same role.
- **Concrete-to-Abstract Design**: Identify concrete classes first (Vehicle, Bicycle, Motorcycle), then abstract their common role (Vehicle Interface).
- **Abstract-to-Concrete Implementation**: Once an interface is defined, implement classes that fulfill it.

## Key Concepts
- **Role/Responsibility**: A contract specifying method signatures (name, return type, parameters) that a class must implement
- **Interface (Java/C#)**: A language construct that enforces a contract; classes declare they implement it and provide implementations
- **Abstract Method**: A method declared but not implemented in a parent class; subclasses must implement it
- **Override/Implementation**: A concrete class provides the specific logic for a role's method
- **Generalization/Abstraction**: The process of extracting common behavior from multiple concrete classes to define a shared role
- **is-a Relationship**: Inheritance; Wagon is-a Vehicle. Vehicle's data structure is inherited and extended.
- **can-do Relationship**: Interface; Vehicle can-transport-people. Many unrelated classes can implement this role

## Mental Models
- Think of interfaces as *social roles*: a Person can be an Employee (at work), a Customer (at a store), or a Patient (at a hospital). The person is the entity; the roles are interfaces.
- Think of inheritance as *data sharing*: Wagon inherits from Vehicle because both have wheels, engine, passengers—shared data. Wagon extends this with cargo space.
- Think of design as *concrete first*: Line up three transport classes (Car, Bicycle, Airplane). Ask "what do they all do?" Answer: "transport people" → Vehicle interface. Never invent an interface from thin air.

## Anti-patterns
- **Interface Before Classes**: Designing a Vehicle interface before any transport classes exist. The interface will miss essential operations or include irrelevant ones.
- **Inheritance Without Data Extension**: Vehicle parent class with only methods, no data attributes. Child classes (Wagon, Sedan) add attributes but don't share the parent's structure—violates is-a logic.
- **Confusing Role and Data**: Treating Employee as a class when it's a role. A Person *is* the data; Employee is a role the person plays. Person is the class, Employee is an interface.
- **Over-Proliferation of Interfaces**: Creating a new interface for each class method. Interfaces should group related responsibilities, not one method per interface.

## Worked Example
A system needs to handle different transportation modes: cars, bicycles, and planes. If you start with interfaces:

**Wrong (interface-first):**
```
interface Vehicle {
  void startEngine();  // Bicycle has no engine!
  void fillGasTank();  // Plane uses jet fuel!
  double getMaxSpeed();
}
```

This interface doesn't fit all cases.

**Correct (concrete-first):**
1. Write concrete classes:
```
class Car {
  Engine engine;
  double maxSpeed;
  void startEngine() { engine.start(); }
}

class Bicycle {
  double maxSpeed;
  void pedal() { /* ... */ }
}

class Airplane {
  Turbine turbine;
  double maxSpeed;
  void startTurbine() { turbine.start(); }
}
```

2. Observe: All three move people. Some have engines, some don't. Generalize:
```
interface Vehicle {
  double getMaxSpeed();
  void beginOperation();  // Generic: start engine, pedal, or turbine
}
```

3. Implement:
```
class Car implements Vehicle {
  public double getMaxSpeed() { return maxSpeed; }
  public void beginOperation() { startEngine(); }
}

class Bicycle implements Vehicle {
  public double getMaxSpeed() { return maxSpeed; }
  public void beginOperation() { pedal(); }
}
```

Now code can treat all vehicles uniformly: `vehicle.beginOperation()` works for car, bicycle, or plane.

**Inheritance Example:**
Two vehicle classes differ in data: Car has fuelTank, Bicycle doesn't. Both have engine. Create an abstract Vehicle parent:
```
abstract class Vehicle {
  Engine engine;
  double maxSpeed;
  abstract void operate();  // Subclasses define how
}

class Car extends Vehicle {
  Fuel fuelTank;
  void operate() { /* start engine, run */ }
}

class Bicycle extends Vehicle {
  // No fuelTank; uses engine differently
  void operate() { /* pedal, engine assists */ }
}
```

But wait—Bicycle doesn't need an engine! This violates is-a: a Bicycle is-a Vehicle, but not all vehicles have engines. Better design:
```
class Vehicle {
  double maxSpeed;
}

class MotorVehicle extends Vehicle {
  Engine engine;
}

class Car extends MotorVehicle { }
class Airplane extends MotorVehicle { }
class Bicycle extends Vehicle { }
```

Now inheritance respects data structure: MotorVehicle adds Engine; non-motor vehicles skip it.

## Key Takeaways
1. Interfaces are discovered, not invented: examine concrete classes, identify shared operations, then abstract the interface
2. Design flows concrete → abstract (find common behavior), but implementation flows abstract → concrete (use the interface)
3. Multiple unrelated classes can implement the same interface (polymorphism); only classes sharing data should use inheritance
4. Inheritance is for data structure sharing (is-a); interfaces are for role definition (can-do)
5. When in doubt, start with concrete classes and their data; the interface will emerge naturally

## Connects To
- **Ch 1**: Interfaces solve the "role vs. class" problem by letting one entity (a class with data) play multiple roles (implement multiple interfaces)
- **Ch 2**: Small-grain classes often implement simple interfaces (Displayable, Numeric) enabling polymorphic formatting and calculation
- **Ch 3**: Functions abstract operations; interfaces abstract behaviors. Together, they enable flexible, extensible designs
- **Ch 4**: Method placement rules apply within class hierarchies; interfaces enable cross-cutting polymorphic behavior
