## Description

This solution uses the `sync` feature onto the child component, passing down the object.\
The object is not going to be cloned because all the changes are not made directly on something. Instead we deal only with events
inside the child component that will be captured, providing a new value for a certain property, and than the updated version is emitted.

## When to use

Use it when there is not much to update, the updates are connected to few events and there is not the need of directly change something inside the child
(e.g. with a v-model)

### Notes

Change branch to see all the ways!
