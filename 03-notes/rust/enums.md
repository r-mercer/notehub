# Enums

Enums work how you would think, some cool things are though that you can pass enum object to a function so that it will accept anu value. 

## Enum members

We can also put data directly into each enum 'variant'. This is done by initing an enum with each limb of the enum having a data type. We can then attach data to that enum.

This is effectively an automatic constructor function defined as a result of defining an enum.

## Structs vs Enums

I am not totally sure on the use case of a struct versus an enum. Structs let you do a whole bunch of shit, but enums seem to let you do just as many things?

## Nulls or Option Enums

Option<T> is a generic enum that effectively implements a way to handle nulls. This seperation of nullable types means that you can express a val as a null, but if you want to actually use that value, you have to write code to handle it being null and then turning it into hte concrete object you want. 

To determine if you are null or not, you would generally run either Some(t) or None. Usually with a match expression.

## Match

Match is effecively a switch statement, but with a fat arrow.

You can also use matching to parse the value of a nessted enum and then handle or use that in some way.

If you are doing a match to nullcheck, you call match on the argument passed in, then write:
`None => None,`
`Some(i) => Some(i + 1)`

I don't really konw why you need to use a different variable, but this will return none if the val is none and `i + 1` if the var is an int.

Matches must be exaustive, else they won't compile. There is no default in this switch case.

Oh, yes there is. It's `other`, which can just go at the end and be a default. Other will however bind the val compared to itself. If you want to just break at the default val and not bind it, you can just use an underscore. `_ => something()`

## If Let

`if let` allows you to combine if and let statements into a streamline way to handle vals that match a pattern. I think I will need to have a go at actually doing this, but effectively, it can be thought of as syntactic sugar on a match statement (without the requirement to check everything).

There is also a plain old if statement, and that works how you would think it does.
