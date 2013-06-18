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

``arguments`` looks like a javascript "array" in that it is an object with numbered indicies and it has a ``length`` property, but that is where the similarity ends.  There doesn't seem to be a good reason for arguments not being a proper array.  I am going to assume that this is just javascript being broken and not a design feature.  So rather than rewrite my beautiful ``joinArguments`` function, I have decided find a concise way to make ``Array.prototype`` methods available to the ``arguments`` object.  

## First Attempt

