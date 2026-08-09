# Chapter 4: Method Implementation Location (2 Principles Only)

## Core Idea
When a method touches attributes from multiple classes, placement is governed by a single question: do the classes have a hierarchy? If yes, put the method in the top class. If no, create a mediator class. Only two principles exist because all complex relationships reduce to binary decisions.

## Frameworks Introduced
- **Principle 1 – Hierarchical Containment**: When classes have a header-detail relationship (Order contains OrderLines), implement methods in the topmost class, delegating to lower classes to avoid exposing internal data structures.
- **Principle 2 – Mediator Pattern**: When classes are peers (Receipt and Inventory), create a third class (ReceiptInventory) to coordinate their interaction, preventing tight coupling when either class changes.

## Key Concepts
- **Header-Detail Relationship**: A parent class holds or contains instances of child classes. Orders contain OrderLines; OrderLines contain OrderItems. Operations affecting the hierarchy belong in the parent.
- **Delegation**: The parent method calls child methods without exposing the child's internal structure (e.g., how OrderLines are stored: array, List, Map).
- **Mediator/Coordinator**: A third class that knows about both parties and acts as their intermediary, preventing direct coupling.
- **Loose Coupling**: Designing so a change in one class doesn't force changes in peer classes.
- **Tight Coupling**: Two classes calling each other's methods directly; changes ripple between them.
- **Sequence Diagram**: A tool to verify method placement is correct by tracing the flow of calls from the top class downward.

## Mental Models
- Think of header-detail as a *tree structure*. Order is the root; OrderLines are branches; OrderItems are leaves. Operations should flow from root downward, not sideways.
- Think of peer classes as *independent domains* that need an intermediary to collaborate. Receipt domain (receiving goods) and Inventory domain (tracking stock) are separate concerns; ReceiptInventory orchestrates them.
- Use sequence diagrams to *validate* method placement: if calls jump sideways or loop unexpectedly, reconsider which class should own the method.

## Anti-patterns
- **Exposing Internal Structure**: Order.calculateTotal() loops through OrderLines' internal data structure (array, List) directly instead of delegating to OrderLines. When storage changes, Order breaks.
- **Direct Peer Coupling**: Receipt.recordInInventory() directly calls Inventory.addStock() with detailed Receipt knowledge. When Inventory's interface changes, Receipt breaks.
- **Method in Wrong Hierarchy Level**: Implementing Order.getTotalQuantity() in OrderItem instead of Order or OrderLines. Requires clients to iterate and aggregate themselves.
- **Mediator as Mere Conduit**: Creating ReceiptInventory but only forwarding calls with no logic. Real mediators hold the state and logic that connects both parties.

## Worked Example
An order system has Order (header) and OrderLine (detail) classes. You need a method to return the total order amount. Where should it live?

**Wrong (exposes internals):**
```
class Order {
  OrderLine[] lines;
  Amount getTotalAmount() {
    Amount total = new Amount(0);
    for (OrderLine line : lines) {  // Exposing how lines are stored
      total.add(line.getAmount());
    }
    return total;
  }
}
```

If lines change from array to Map, this breaks.

**Correct (delegates):**
```
class Order {
  OrderLines orderLines;  // Encapsulate structure
  Amount getTotalAmount() {
    return orderLines.getTotalAmount();  // Delegate
  }
}

class OrderLines {
  private OrderLine[] items;  // Internal structure hidden
  Amount getTotalAmount() {
    Amount total = new Amount(0);
    for (OrderLine line : items) {
      total.add(line.getAmount());
    }
    return total;
  }
}
```

Now if OrderLines' storage changes, only OrderLines changes; Order is unaffected.

**For peer classes (Receipt and Inventory):**
Without Mediator:
```
class Receipt {
  void recordInInventory(Inventory inv) {
    for (ReceiptLine line : lines) {
      inv.addStock(line.getProduct(), line.getQuantity(), line.getLocation());
    }
  }
}
```

Receipt now knows Inventory's interface. If Inventory changes, Receipt breaks.

**With Mediator (Correct):**
```
class ReceiptInventory {  // Mediator
  Receipt receipt;
  Inventory inventory;
  
  void record() {
    for (ReceiptLine line : receipt.getLines()) {
      InventoryItem item = new InventoryItem(
        line.getProduct(), line.getQuantity(), line.getLocation(), receipt.getDate()
      );
      inventory.add(item);
    }
  }
}
```

Now Receipt and Inventory are independent. If Inventory's interface changes, only ReceiptInventory updates.

## Key Takeaways
1. Two principles govern all method placement: hierarchy and peer relationships
2. In hierarchies, methods live in the topmost class and delegate downward; never expose child structure
3. For peer classes, create a mediator to coordinate; this keeps changes localized and prevents ripple effects
4. Use sequence diagrams to verify your method placement; unexpected paths indicate wrong placement
5. "Lowest common ancestor" thinking helps: the method should live where both data structures are most naturally accessible

## Connects To
- **Ch 1**: These principles exemplify data-structure thinking; method placement is derived from how data is organized, not from business logic
- **Ch 2**: Small-grain classes rarely participate in complex hierarchies; they are typically composed, not hierarchically structured
- **Ch 3**: Functions exist at a lower level; method placement rules apply to domain and application classes
- **Ch 5**: Interfaces define roles; a mediator often implements an interface to act as a coordinator between peers
