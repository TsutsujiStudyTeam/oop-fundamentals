# Chapter 3: Using Functions Effectively

## Core Idea
Functions and utility classes are not forbidden, but they must serve small-grain classes—never application-level or domain classes. Functions abstract operations common to multiple data types, ensuring a single-point-of-change when behavior evolves.

## Frameworks Introduced
- **Function Extraction Rule**: Extract a function only when the same operation appears in multiple small-grain classes, and the operation is implementation-level (not business logic).
- **Abstraction Layering**: Keep functions as a service layer for small-grain classes; forbid their direct use from application code. This ensures changes propagate uniformly.

## Key Concepts
- **String Formatting Function**: A utility method like `joinWithSpace(String a, String b)` used by multiple small classes to format paired data
- **Utility Class Danger**: Data-less classes like StringUtilities or ConverterUtils invite misuse when called from application logic, bypassing the cohesive design of small-grain classes
- **Single Point of Change**: When a formatting rule changes (e.g., space between fields becomes 2 spaces instead of 1), modify the function once, and all small-grain classes using it change together
- **Platform Abstraction**: Functions can wrap language-specific concerns (e.g., BigDecimal handling in Java) to provide a consistent interface

## Mental Models
- Think of functions as *shared scaffolding for small-grain classes*, not general-purpose utilities. PersonName and Address both format paired data, so they both use joinWithSpace().
- Use functions when the same *implementation pattern* (not business rule) repeats. Formatting rules are implementation; business rules are not.
- Enforce a rule: *functions may only be called from small-grain classes, never from application code*. This prevents the system from devolving into scattered procedural logic.

## Anti-patterns
- **Calling Utility Functions from Application Code**: Allowing Receipt or Invoice to call StringUtilities.joinWithSpace() directly. Now if formatting changes, you must update Receipt and Invoice as well as Amount and Date.
- **Creating "Generic" Utility Classes**: Building a Converter or Helper class that aggregates unrelated operations. These become dumping grounds for any function that doesn't fit elsewhere.
- **Premature Function Extraction**: Extracting a function before seeing it used in at least two small-grain classes. Premature abstraction creates maintenance debt.

## Worked Example
You have PersonName and Address classes:
```
class PersonName {
  String familyName, givenName;
  String getFullName() { return familyName + " " + givenName; }
}

class Address {
  String zipCode, location;
  String getAddress() { return zipCode + " " + location; }
}
```

Both methods do the same thing: join two strings with a space. Now a requirement arrives: PersonName should use *two* spaces, but Address keeps one space.

If formatting is duplicated inline, you change PersonName and Address separately—and any other class doing similar formatting. If you extracted a function early:
```
class StringFunctions {
  static String joinWithSpace(String a, String b, int spaceCount) {
    StringBuffer result = new StringBuffer(a);
    for (int i = 0; i < spaceCount; i++) result.append(" ");
    result.append(b);
    return result.toString();
  }
}

class PersonName {
  String getFullName() { return StringFunctions.joinWithSpace(familyName, givenName, 2); }
}

class Address {
  String getAddress() { return StringFunctions.joinWithSpace(zipCode, location, 1); }
}
```

Change once, everywhere updates. And since PersonName and Address are the only callers, the function's design remains lean.

## Key Takeaways
1. Extract functions only when the same operation appears in multiple small-grain classes
2. Functions must be called *only* from small-grain classes, never from application, screen, or batch code
3. Functions are implementation-level abstractions, not business abstractions
4. A function is justified when it achieves "single point of change" for logic that spans multiple small types
5. Experienced designers should review extracted functions; casual extraction leads to scattered procedural design

## Connects To
- **Ch 1**: Functions support the data-structure principle by allowing small-grain classes to delegate common operations
- **Ch 2**: Functions exist *because* small-grain classes multiply; without them, you'd repeat formatting logic everywhere
- **Ch 4**: Method placement rules ensure functions remain at the small-grain level; application-level methods never delegate to functions
- **Ch 5**: Functions can be wrapped in interface-based polymorphic designs when multiple implementations of the same operation are needed
