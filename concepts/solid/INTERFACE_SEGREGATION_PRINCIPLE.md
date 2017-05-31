# Interface Segregation Principle

> Clients should not be forced to depend upon interfaces that they do not use. When a client depends upon a class that contains interfaces that the client does not use, but that other clients do use, then that client will be affected by the changes that those other clients force upon the class.

This principle is strongly related to the [Single Responsiblity Principle](./SINGLE_RESPONSIBILITY_PRINCIPLE.md), in that deals with ensuring that your system interfaces start small and deal with only one facet of the responsibilities of your system.

## Interface-based Programming

> **Pro tip:** This is an advanced topic that requires, often, years of practical usage before it *clicks* with a developer. This is a primer, and feeling like it goes over your head is normal.

Interfaces specify the properties and methods that a derived class **must** implement. The interface itself doesn't provide any guidance on *how* they must be implemented - simply that they must.

JavaScript doesn't have any built-in implementation of interfaces.

* Classes define a blueprint for an object.
* Interfaces define an implementation contract for classes.


# References

* [Develop to interfaces, not implementations](http://stackoverflow.com/questions/2697783/what-does-program-to-interfaces-not-implementations-mean)
