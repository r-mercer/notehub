# Macros

Macros are a type of meta-programming. They implement the `derive` attribute which 'expand' to produce more code than actually exists on the page. 

## Differences

Macros compared to functions, can take a variable number of params, as well as other modifications that can hapen before compile type. This includes things such as implementing a trait on a given type.

The disadvantages of macros are that they effectively obfuscate a bunch of code. They are generally more difficult to parse, maintain and implmenet than other functions.
