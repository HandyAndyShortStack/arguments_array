arguments_array
===============

(javascript)
My quest to make ``arguments`` inherit from ``Array.prototype``.

## Why

In some of my coursework for Code Fellows, I was asked to write a javascript function that would concatenate two strings passed as arguments.  Simple, right?

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

This works, but there sure is a lot of typing involved
