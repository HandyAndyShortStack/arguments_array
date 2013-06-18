arguments_array
===============

(javascript)
My quest to make ``arguments`` inherit from ``Array.prototype``.

## Why

In some of my coursework for Code Fellows, I was asked to write a javascript function that would concatenate two strings passed as arguments, separating them with a space.  Simple, right?

```javascript
(function joinArguments() {
  return arguments.join(' ');
})('foo', 'bar');
```

Nope!

```
TypeError: Object #<Object> has no method 'join'
```

``arguments`` looks like a javascript "array" in that it is an object with numbered indecies and it has a ``length`` property, but that is where the similarity ends.  There doesn't seem to be a good reason for arguments not being a proper array.  I am going to assume that this is just javascript being broken and not a design feature.  So rather than rewrite my beautiful ``joinArguments`` function, I have resolved to find a concise way of making ``Array.prototype`` methods available to the ``arguments`` object.

I have to check one thing first: is arguments writable?
```javascript
(function() { return Object.getOwnPropertyDescriptor(arguments, '0').writable; })('value');
// => true
```
Okay, good.


## First Attempt

I'm too ashamed to show this here.  Suffice it to say that it used Object.create in the worst way imaginable.

## Second Attempt

```javascript
(function joinArguments() {
  var _arguments = [];
  for (var i = 0; i < arguments.length; i += 1) {
    _arguments.push(arguments[i]);
  }
  arguments = _arguments;
  return arguments.join(' ');
})('foo', 'bar');
```

I am just iterating over the arguments object and pushing its contents into a proper array.  This works, but there sure is a lot of typing involved.  I can't be bothered to type that out every time I want to use an array method on ``arguments``.  I can do better.

### Refinement

```javascript
(function joinArguments() {
  var _arguments = [];
  for(i in arguments) { _arguments.push(arguments[i]); }
  arguments = _arguments;
  return arguments.join(' ');
})('foo', 'bar');
```

For in loops are not used much in javascript, but this seems like a safe place to do so.  a call to ``arguments.hasOwnProperty`` does not seem necessary, as ``arguments.prototype === undefined // => true``.  This is still a lot to type though.

## Googling

Google led me to this page:
http://stackoverflow.com/questions/960866/converting-the-arguments-object-to-an-array-in-javascript, where ``arguments = Array.prototype.slice.call(arguments, 0);`` is suggested.  A one liner!  Here is my function using this suggestion:

```javascript
(function joinArguments() {
  arguments = Array.prototype.slice.call(arguments, 0);
  return arguments.join(' ');
})('foo', 'bar');
```

This is far easier to type than my previous attempts.  It works by borrowing a method from ``Array.prototype`` that will return a new array.  ``Array.prototype.slice`` doesn't seem to care that ``arguments`` is not a descendant ``Array.prototype``.  I think I can do better though.

## Final Attempt

```javascript
(function joinArguments() {
  arguments = Array.apply(null, arguments);
  return arguments.join(' ');
})('foo', 'bar');
```

I am happy with this.  One way to create a new array is by just calling ``Array`` with the contents of the array as arguments.  In order to reformulate this function call to accept an array-like object in place of an explicit list of arguments, I used ``Function.prototype.apply`` and passed ``null`` in as the ``this`` value.  

## Conclusions

I have no idea why I did this, but now I know a bit more about prototypal inheritance than I did before.  ``arguments`` really should inherit from ``Array.prototype``.
