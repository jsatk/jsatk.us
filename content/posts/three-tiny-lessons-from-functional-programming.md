+++
draft = false
date = 2018-01-17T00:00:00-00:00
title = "Three Tiny Lessons from Functional Programming"
description = ""
slug = ""
tags = []
categories = []
externalLink = ""
series = []
+++

**Originally published on the Earnest engineering blog [here](https://medium.com/earnest-engineering/three-tiny-lessons-from-functional-programming-d546092eb5d1).**

In the past two years or so the term "Functional Programming" (FP for short) has cropped up everywhere. You can't get away from it. Especially in javascript. It feels like every JS developer but me was invited to a cool FP party and all of a sudden traditional object-oriented programming (OO for short) was for dinosaurs.

Also in the past two years, more thinkpieces than I care to link to have been written stating why or why not FP is a good thing for the javascript community.

All this change and usage of new words like "monad" and "functor" can seem very daunting if you're a more traditional (read "older") javascript developer. The goal of this post is not to convenience you that FP is better than OO. It is not meant to persuade you one way or the other. Its goal is to introduce to you some of the smaller, simpler, and more powerful patterns that I've grown to love since studying and learning just exactly what FP in JS is. Even if you love OO or are stuck in a codebase that uses it, these principles apply there too. And ultimately I believe they will make your code more bulletproof.

*1. Don't mutate an object unnecessarily*

Whenever possible avoid mutating an object. Notice I didn't say "never" (and I'm sure there are some FP purists shaking their fists at me). Objects in javascript are passed by reference–meaning that if you pass around an object and various methods mutate that object it can have drastic and unwanted side-effects. I've created a trivial example of what I often see below:

```js
const transform = response => {
  if (response.verify) {
    response.verificationRequired = true;
  }

  response.amount = response.amount * 100; // I see this often for cents to dollars converion between APIs
  return response;
};
```

In the above we do some trivial checks and conversions. The reasons why aren't important. What is important is that, in this context, we have no idea where that response object came from or who's using it. Assuming it came from somewhere else in our codebase we can do the research and make sure it's okay we modify this object. But a much safer way that causes less burden on any future developers that look at this code is to use `Object.assign` (or if you're on a newer version of node or only support newer browsers you can use the `…` operator).

```js
const transform = response => {
  return Object.assign({}, response, {
    verificationRequired: Boolean(response.verify),
    amount: response.amount * 100
  });
};

const transform = response => {
  return {
    ...response,
    verificationRequired: Boolean(response.verify),
    // Since this comes *after* the original response the `amount` here will
    // overwrite the original `response.amount`.
    amount: response.amount * 100
  });
};
```

In the latter example we return an entirely new object without modifying the original object. This is paramount for ensuring our code has as little footprint as possible, doesn't cause side-effects, and is easier to understand for developers new to the codebase.

*2. Classes aren't always the answer*

Often a class is overkill. Particularly if it's for a one-time use situation. Yet, when it's what you know it can be hard to wrap your head around not using it. One of the biggest and most common "gotcha"s with using classes is the `this` keyword. The problems with `this` are many and have been written about [elsewhere](https://www.i-programmer.info/programmer-puzzles/137-javascript/1922-the-this-problem.html) so I won't elaborate on them here. But they are real and they are many.
Lets take a look at the MDN example for classes.

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  get area() {
    return this.height * this.width;
  }
}

// Pre-ES6
function Rectangle(height, width) {
  this.height = height;
  this.width = width;
}

Rectangle.prototype.getArea = function () {
  return this.height * this.width;
}
```

This can be easily re-written as a simple function that returns an object.

```js
const rectangle = (height, width) => ({
  height,
  width,
  get area() {
    return height * width;
  }
});

const myRectangle = rectangle(10, 20);
myRectangle.area // => 200
myRectangle.height // => 10
myRectangle.width // => 20
```

The result of both examples is the same, but the wrapper pattern used in the latter example requires fewer lines, doesn't require usage of (or understanding of) `this`, and lastly doesn't require the `new` keyword to invoke. Overall there are fewer concepts to keep track of and understand.

*3. `Object.freeze` is an informative little tool*

For some reason it seems that everyone forgot about or never learned about `Object.freeze`. I only started using it in the past year. But it has quickly become one of my favorite no-brainer tools. It does pretty much what it says on the tin. Any object you don't need to mutate can be "frozen" by passing it into `Object.freeze`. The `Object.freeze` method returns a frozen copy of the object passed in.

There are two gotchas with using `Object.freeze`.

1. If you eventually pass a frozen object to third-party code it's possible you'll get an error because the third-party code may attempt to modify the object.
2. `Object.freeze` is shallow. Meaning that if your object contains properties that are also objects it will not freeze those objects. The concepts of deep vs. shallow when working with objects comes up a lot. `Object.freeze` isn't unique in this regard, but it is worth noting. So if you call `Object.freeze({foo: {bar: 10}})` it will not freeze the value of foo (which is an object). See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) for more info.

I've found this to be particularly useful in my test suite. It ensures I'm not making any false assumptions about what my code does or benefiting from any side-effects unknowingly.

For example:

```js
const options = {
  async: true,
  page: 4
};

const awesomeFunction = options => {
  if (!options.offset) {
    options.offset = 10;
  }

  // do stuff with options
};

awesomeFunction(options);
someOtherFunction(options);
yetAnotherFunction(options);
```

The above code modifies `options` once it's passed into `awesomeMethod`. Let's say `options` is used for multiple methods all invoked at about the same time, depending on order of execution this can produce unwanted results since we are modifying the options object at the beginning of `awesomeMethod`. If that object were frozen we'd have gotten a javascript error (in strict mode, you're using strict mode right?!) letting us know we were attempting to modify a frozen object, which is super useful feedback for not only the original author but any new developers coming into the code.

Lets see what that would look like with `Object.freeze`.

```js
const options = Object.freeze({
  async: true,
  page: 4
});

const awesomeFunction = options => {
  const updatedOptions = Object.assign({}, options, {
    offset: options.offset || 10
  });

  // do stuff with updatedOptions
};

awesomeFunction(options);
someOtherFunction(options);
yetAnotherFunction(options);
```

Here we're combining things we learned in the first point with things we're learning here. `Object.freeze` causes JS to throw an error if we attempt to modify the object, so we then use `Object.assign` (or `...`) to create a new options object that's local to the method thus preventing any timing issues. Concepts like this can be very useful even in OO programing.

Finally, lets combine all three of the above concepts. The example here is trivial, but it gives you an idea of the simplicity and power of working without classes (when appropriate) and not mutating objects.

```js
const config = Object.freeze({
  async: true,
  page: 4
});

const getThing = config => {
  const awesomeFunction = options => {
    const updatedOptions = Object.assign({}, options, {
      offset: options.offset || 10
    });

    // do stuff with updatedOptions
  };

  return Object.freeze({
    ...config,
    awesomeFunction,
    amount: config.amount * 100
  });
};

const thing = getThing(config);
```

Combining the deceptively simple concepts from the three points listed above can radically change how confident you are in your code. If you combine `Object.freeze` with a function that returns an object (rather than a class) and `Object.assign` you can ensure that you have a configured object that is immutable and that nothing along the way mutated it due to a side-effect.

These tiny concepts have saved me from shooting myself in the foot with vanilla JS and I hope you find them useful too.
