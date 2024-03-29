# Generator sort

[![Build Status](https://travis-ci.org/mattiash/generator-sort.svg?branch=master)](https://travis-ci.org/mattiash/generator-sort) [![Coverage Status](https://coveralls.io/repos/github/mattiash/generator-sort/badge.svg?branch=master)](https://coveralls.io/github/mattiash/generator-sort?branch=master)

Sorting on multiple criteria with generator functions

## Background

With the [Array.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) method in javascript,
it is possible to sort an array of anything based on a sorting function.
For example, if you have an array of objects like this:

```javascript

let characters = [
    {
        firstName: 'Luke',
        lastName: 'Skywalker',
    },
    {
        firstName: 'Han',
        lastName: 'Solo'
    }
    {
        firstName: 'Anakin',
        lastName: 'Skywalker'
    }
]
```

And you want to sort them based on lastName first and then firstName,
you have to write a sort-function like this:

```javascript
characters.sort( (a,b) => {
    if(a.lastName > b.lastName) {
        return 1
    }
    else if(a.lastName < b.lastName) {
        return -1
    }
    else {
        if(a.firstName > b.firstName) {
            return 1
        }
        else if(a.firstName < b.firstName) {
            return -1
        }
        else {
            return 0
        }
    }
})
```

Writing these sorting functions is not particularly difficult,
but they can become quite long and rarely look elegant.

## Sorting with generator functions

With the generator-sort module, the same sort can be written as

```javascript

import {sortFunction, compareStrings} from 'generator-sort'

characters.sort(sortFunction(function*(oA, oB) {
    yield compareStrings(oA.lastName, oB.lastName)
    yield compareStrings(oA.firstName, oB.lastName)
}))
```

The `sortFunction` function takes a generator function as its only argument.
The generator function will be called for each pair of object that shall be compared.
The function shall yield numbers.
If it yields a positive number,
oA is larger than oB.
If it yields a negative number,
oB is larger than oA.
If it yields 0,
it will be asked to yield another number until it yields a non-zero
number or is done (i.e. returns).

This makes it easy to write the comparison as a series of yield statements.

## Typescript

The module is written in typescript and comes with complete typings,
but can be used in plain javascript as well.