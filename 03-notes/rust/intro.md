# Rust Intro

## Data Types

Consts should be in UPPERS
Lets are immutable by default, unless mut is passed

### Shadowing

Shadowing lets you change the value of a variable within a different scope. This is shadowing as opposed to actually overwriting that var because it is only within the narrower scope. 

You don't need to call mut on a shadowed var (only let) because you're not actually modifying the variable, you're just redeclaring it. 

Shadowing is cool to use if you're converting data types and don't want to create new names every time.

## Data Types

Apparently Rust loves themselves a tuple, which is good because I also like a tuple

Arrays in Rust live on the stack. This means that they must have a fixed length and can probably be thought of closer to ref types in c sharp. If you would like to use a variable length array, you can use a vector, which is another compound type exposed by the standard lib.

## Expressions vs Statements

Functions are made up of statements, and may end in an expression. Expressions evaluate and return a value. Statements are instructions that do not return a value. Think Void keyword.

Rust is apparently an expression based language, but not sure what the impact of that is.

## Return Statements

If you want a non-void function, just stick the return type between the params and the curly braces after a skinny arrow. fn name() -> return {}

You canthen include a return statment if you're feeling zesty, but it is mostly implicit.

Implicit return is aided by the difference between an expresison and a statement. Statements have semi-colons, where as expressions do not. If you include a semi colon, you have made a statement and not evluated a value. Ommitting the semi colon will denote an expression, which will evaluate and then you'll be off to the races.

## Logic Flow

Seems pretty standard. You can name your loops though, so that you can break the non-innermost loop. This is done by adding a name before the loop statement prefixed with a single apostrophe.
