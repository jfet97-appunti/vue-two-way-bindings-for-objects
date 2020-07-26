## Description

This solution uses the `sync` feature onto the child component, passing down the object.\
The object is cloned using a get-set computed property, but all the changes are not made directly on something. Instead we deal only with events
inside the child component that will be captured, providing a new value for a certain property that is merged with the current clone of the
original object to form a new clone.\
The computed setter is going to handle the emission of the updated object.

## When to use

Use it when there is not much to update, the updates are connected to few events and there is not the need of directly change something inside the child
(e.g. with a v-model)

### Notes

Change branch to see all the ways!
