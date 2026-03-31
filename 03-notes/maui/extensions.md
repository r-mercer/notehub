# Extensions 

If you would like to manipulate elements, you can implement the MarkupExtension interface. This interface implements one method called ProvideValue. This method exists to return values to markup. The return can be of any type because it's an object. 

To use this, you first want to make an extension that will interact with your component, inject the interface, use the method to do things with the properties of your elements. 

to then use this, add the extension namespace to your content page tag using the weird MAUI format. 

## Static Extensions

If you want to use statics and then work with those things, you can use the StaticExtension class. To use, you create a static class, pass in the interface then set things as you would like. You might need to sprinkle in the work member, or that might actually just be a name.
