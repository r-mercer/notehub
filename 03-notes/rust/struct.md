# Structs

A struct in Rust is kinda like a javascript object, or a c# class.

They group things like a tuple, then each 'thing' has a name, called a field.

You first define an abstract version, then instantiate one by providing data. Doesn't look like there is a 'new' call anywhere though. 

If you create a fuction to init a struct object, you don't have to write param = argument, if the field and the param have the same name, then they will automatically be copied in.

If you want to init an object and copy in data from another object, you can just make the changes you want to make and then write `..OtherObject` 

But if you are referencing data from another object, the same rules apply as normal ownership. If the data type does not implement copy, then the reference to the first object will be lost if applied to the second object. 

You can also name a tuple, which makes it a 'tuple struct'. Has the benefit of making a special tuple, without actually needing to create a new struct every time. This is done by defining a struct, passing the datatypes and giving it a name. Like so: `struct Colour(i32, i32, i32)`.

## Example

The example given compares the use of tuples and structs in the following. First it supposes that you write a program that calcs the area of a rectangle. First the area function is passed two params, length and width.

Then they add structure by passing a single tuple, dimensions. This is better because of the structure, but can be a bit harder to pass because the arms are not labeled. 

They then make the data / function more meaningful by labelling the data / creating a struct called Rectangle, with a width and height field. Notably though, they borrow rectangle (create and pass a ref to the function) so that main can maintain ownership.

## Debug printing

If you want to print things from the debug output, you can decorate your struct with the 'outer attribute' `#[derive(Debug)]`, just stick that at the top of your initial struct declaration and you can print it out with the printlin! macro no worries.

if you are thinking about console.writeline, then you probably actually want to use dbg! macro.

## Methods

Methods are functions that live in structs (or enums or traits). They must be passed `self` as a first param which represents the instance of the struct that the method is being called on.

You don't define this within the actual struct itself, but rather you add an `impl` or implementation block, which extends the struct. 

You can use methods to create your own getters. If you mark the field as private and the method as public, then you have made a fancy read only access getter. 
