# Liskov Substitution Principle

## Definition

> Let &#966;(&#967;) be a property provable about objects &#967; of type T. Then &#966;(&#933;) should be true for objects &#933; of type S where S is a subtype of T.

#### In Programmer Language
Functions that use references to base classes must be able to use objects of the derived class with no runtime side effects.

#### In Beginner Language
Derived classes must be substitutable for the base class without any impact on the expected functionality of the system.

## Object Behavior Implications
This principle makes clear that in object-oriented design the _IsA_ relationship pertains to behavior. Not intrinsic private behavior, but extrinsic public behavior; behavior that clients depend upon. This means that if two classes share the exact same properties and methods, that does not ensure that one is inheritable from the other.

## Example

Let's say you're building an application that displays products for people to buy. In order to represent those products, as a responsible developer, you create a class definition which defines the properties and behaviors of a product.

```js
    // Sample JavaScript code here
```

