# Single Responsibility Principle

> There is one and only one reason to change a class. More specifically, this principle states that every module or class should have responsibility over a single part of the functionality. Think of your classes as Actors in your software, and what the responsibility of each is.

This means that we should start to think small. Each complex problem cannot easily be solved as a whole. It is much easier to first divide the problem in smaller sub-problems. Each sub-problem has a reduced complexity compared to the overall problem and can then be tackled separately.

## Impact on Code

1. Easier to understand
1. Less error prone
1. More robust
1. Easier to test
1. More maintainable and extendable

## Example

Consider this representation of a Patient.

```js
    // keep
```

Would you consider this class to be complying with the Single Responsibility Principle? Is there only one reason for this class to change?

To answer this question, you must ask yourself what the purpose of the Patient class is. At the most basic, it is intended to be a representation of a person who needs medical care. It should also provide accessors (getters) and mutators (setters) to change its current state.

Given that initial intention, is the class definition above adhering to the SRP? Obviously not, since it's also performing operations involving admitting and discharging the patient from a hospital, something that a Patient is not responsible for. 

Who is responsible for that? That would be a Doctor.

What else is a Doctor responsible for? Perhaps diagnosing a patient with an ailment and providing treatments. How does a doctor record that information? Certainly not by tattooing it onto the patient, so having the Patient class store a collection of ailments and treatments violates the SRP as well.

Let's add some code to assign the proper responsibility to the proper actor in this scenario. We have two obvious actors - the Patient and the Doctor.

> **patient.js**

```js
    // keep
```

> **doctor.js**

```js
    // keep
```

This is a good start. Some basic properties are defined for each actor and they can be retrieved and modified. You'll notice that I modifed the `Patient` class to only retain a set of chronic illnesses since those are long-term, or permanent state modifications of a doctor's patient.

Ok, time to start adding responsibilities to the doctor.

```js
function diagnose_patient () {
    ...
}
```

So... what do I do here? I need a patient to diagnose.

```js
function diagnose_patient (patient) {
    ...
}
```

Then I need the ailment to assign to the patient.

```js
function diagnose_patient (patient, ailment) {
    patient.diagnose(ailment)
}
```

Now, a sample set of implementation logic.

```js
    // keep
```

Now we have the appropriate actor diagnosing a patient.

Next, we need to address the `admit_to_hospital`, and `discharge_from_hospital` methods that were on the original `Patient` class. What actor would be responsible for those actions?

Right, a doctor also does that. Since the set of hospitals no longer is maintained in the state of the `Patient` class, how would we admit a patient to one?

I think we now need a `Hospital` class.

```js
    // keep
```

Do you remember the term for relating individual actors to each other without creating a dependent relationship? Right, it's aggregation.

```js
    // keep
```

Now we can add the responsibility to the doctor.

```js
    // keep
```

See if you can think of other actors involved in the process of routine health care: visiting a primary physician, making an appoinment, filling prescriptions for treatment, etc. Using the SRP, define the responsibilities of each actor and how the relationships between each one could be defined.
