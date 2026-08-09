# Chapter 2: Small-grain Classes for Reusability

## Core Idea
Create very small, focused classes for fundamental data types (Quantity, Amount, Date, PersonName). These "small-grain" classes maximize reusability across systems and generations of software, while providing consistent behavior for operations that recur throughout the codebase.

## Frameworks Introduced
- **Small-Grain Classes (小粒クラス)**: Minimal, hyper-reusable classes representing atomic data values with units or meaning (e.g., Amount, Quantity, Date, Email). These are designed to be attributes of larger classes, not standalone domain entities.
- **Numeric Type Hierarchy**: A layered structure where specialized numeric classes (Amount, Quantity) inherit from a general Number class, adding domain-specific operations like "return string with currency symbol" or "calculate tax."
- **Data Boundaries**: Identifying where one logical data group ends and another begins. A Quantity includes a numeric value *and* a unit (e.g., "3個", "10kg").

## Key Concepts
- **Number (基数)**: Base class for all numeric values in the system, handling precision (via BigDecimal), arithmetic, and string formatting with commas
- **Quantity (数量)**: Number + Unit; represents "3 pieces", "10 kg", "43 square meters"
- **Amount (金額)**: A currency-aware numeric class representing monetary values
- **Currency (通貨)**: Identifies which country's currency an amount uses; holds exchange rate tables
- **JapaneseYenAmount (円貨金額)**: Specialized for Japanese yen; includes tax calculation (internal, external, tax-only variants)
- **ForeignAmount (外貨金額)**: Handles non-yen currencies with exchange rates
- **TaxRate & TaxRateTable**: Encapsulate tax rate lookups by date, abstracting tax law changes
- **Date (日付)**: Wraps date values with formatting for display (e.g., "2012年03月02日")
- **PersonName (氏名)**: Given name + Family name, with methods to return full-name strings

## Mental Models
- Think of small-grain classes as *reusable utilities that carry behavior*. Instead of calling StringUtilities.formatAmount(value) from everywhere, embed that logic in Amount.getFormattedDisplay(), so changes propagate automatically.
- Use small-grain classes when the same operation appears in multiple larger classes. If both PersonName and Address format two strings with a space, create a small-grain StringFormatter and use it from both.
- Design small-grain classes for *long-term stability*. A Date class written correctly will work across projects, decades, and platforms because it embodies date semantics, not business processes.

## Anti-patterns
- **Primitive Types Everywhere**: Using plain int, String, or double throughout your codebase. This scatters formatting and validation logic (how to display an amount? how to add two quantities?) across many methods.
- **Ignoring the Unit**: Storing Quantity as just a number without tracking its unit. Confusing "3" (of what? pieces? kilograms?) leads to calculation errors.
- **Creating Small-Grain Classes Too Late**: Waiting until duplicate formatting code appears in five places. Design small-grain classes during analysis, when you identify fundamental data concepts.

## Worked Example
Consider a system managing orders. Initially, you might store prices as double: `double unitPrice = 99.99;` and format them inline:
```
String display = "$" + String.format("%.2f", unitPrice);
```

Now tax rules change, and you need to recalculate and display tax. You add this code to OrderLine, Receipt, Invoice, and Report classes. When currency changes from USD to EUR, you need to update string formatting in all four places.

Instead, design Amount and Currency classes:
```
class Amount {
  private BigDecimal value;
  private Currency currency;
  
  String getFormattedDisplay() {
    return currency.getSymbol() + formatWithCommas(value);
  }
  
  Amount addTax(TaxRate rate) {
    BigDecimal taxed = value.multiply(rate.getRate());
    return new Amount(value.add(taxed), currency);
  }
}
```

Now OrderLine, Receipt, Invoice, and Report all use `amount.getFormattedDisplay()` and `amount.addTax()`. Change the currency symbol or tax calculation once in Amount, and all clients see the change instantly.

## Key Takeaways
1. Small-grain classes are investments in maintainability: one change to Amount.getFormattedDisplay() fixes formatting across the entire system
2. Numeric types warrant a hierarchy: Number → Quantity/Amount → specialized types (JapaneseYenAmount, ForeignAmount)
3. Small-grain classes carry both data and the operations that recur across larger classes
4. Date, PersonName, Quantity, Amount—these are the backbone of reusable design; identify them early during analysis
5. A well-designed small-grain class survives system rewrites and organizational changes

## Connects To
- **Ch 1**: Small-grain classes exemplify data-structure-based design; they prove that organizing by data creates reusable components
- **Ch 3**: Functions are often extracted to handle operations across multiple small-grain classes (e.g., string formatting utilities for multiple small types)
- **Ch 4**: Small-grain classes typically don't participate in complex hierarchies; they are frequently composed into larger classes
- **Ch 5**: Small-grain classes usually implement simple role interfaces (Numeric, Displayable) to enable polymorphic display or calculation
