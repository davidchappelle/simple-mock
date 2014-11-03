# simple-mock ![Project status](https://secure.travis-ci.org/jupiter/node-simple-mock.png?branch=master)

Super simple mocks, stubs, and spies with 1-step sandbox restore.

## Install

`npm install simple-mock`

## Mock

You can mock any *function* or *value* on an object and *easily* restore it.

Examples

```
simple.mock(obj, 'example', 'value'); // Replace with this value
simple.mock(obj, 'example', function() {}); // Replace with this function

simple.mock(obj, 'example') // Spy on underlying method *or* stub
simple.mock(obj, 'example').callbackWith(null, 'etc'); // Stub
simple.mock(obj, 'example').returnWith('etc'); // Stub
simple.mock(obj, 'example').throwWith(new Error()); // Stub
```

Then, to make sure all objects are back to the state the were in before your mocks:

```
simple.restore(); // Ideally called in an afterEach() block
```

`callbackWith`, `returnWith` and `throwWith` can be chained for queued behaviour, e.g.

```
simple.mock(Something.prototype, 'example')
  .callbackWith(null, 'etc')
  .callbackWith(new Error());
```

`callbackWith`, `returnWith` and `throwWith` configurations are stored on a simple array fn.actions

## Expectations

You define your expectations with your own choice of assertion library.

```
assert(fn.called);
assert.equals(fn.callCount, 3);
assert.equals(fn.lastCall.args[0], error); // First parameter of the last call
assert.equals(fn.calls[0].returned, 'etc');
assert.equals(fn.calls[1].threw, error);
```

## Standalone Stubs and Spies

If you need to create a standalone stub (stubs are also spies):

```
simple.stub().callbackWith(null, 'etc');
simple.stub().returnWith('etc');
simple.stub().throwWith(new Error());
```

Or spy on a standalone function:

```
var fn = simple.spy(function(){});

assert.equals(fn.callCount, 0);
assert.equals(fn.calls, []);
```
