# UI In Maui

Use c# code for creating dynamic elements however consider uysing static elements of the UI using XAML. This allows the positioning of UI elements at RunTime. 

## Things to Note

There are a collection of things to know that you might be interested in:
- InitializeComponent() is a command that you use in the page constructor to read the page's XAML and load the page. You can only call it if you're using CAML.
    - This does things like initialising fields, so you should place anything that deals with the elements after this call.
        - The exception to this is the ContentPage calss. You can access all properties of a page prior to execution. But you cannot update these properties until InitializeComponent().

## Advantages of XAML

XAML allows you to easily seperate the UI elemetns from your logic and behaviour. Similarly to how HTML and JS live in their own seperate boxes. 

XAML lets you declaratively describe an app's UI. These elements directly represent instantiated objects, which you can manipulate with code behind c#.

## Types in XAML

The .NET MAUI parser parses XAML and instantiates each element as a .NET type. These generally live in the MS.MAUI.Dependancies / Extensions packages. 

When you want to use a library in MAUI, you must call it in the Content Page element using the xmlns attribute. 

To instantiate a type, you use the Object Element Syntax. This kinda looks like HTML.

When you are passing attributes to an element you must use a string, as that is the only thing that xml accepts. If you want to change that you must use a type converter. For most things that MAUI does, it will do this automatically. Such as converting a colour string "Black" into the appropriate colour code to be rendered.

You can also create your own custom types / objects using an element based syntax. This is called the property element form. This involves seperating the setter into a parent-child form, andexpressing the property as an element of the type. Say you were using a guesture recognising element to do something, this is a complex object with its own distinct properties. Typically these must be assigned to ensure property functionality. You can assign these within the existing elements by passing them into the element, such as this:

`<Label Text="Username" TextColor="Black" FontSize="42" FontAttributes="Bold,Italic">
    <Label.GestureRecognizers>
        <TapGestureRecognizer NumberOfTapsRequired="2" />
    </Label.GestureRecognizers>
</Label>`

## Event Handling

Generally, to handle events, you assign a name to the XAML element using the `x:Name` attribute. This does two things:

- it generates a private field to the code behind file, and 
- Makes the element known to XAML through this name. 

This must conform to naming conventions, which is CapitalWords. This also must be unique and other things. 

Most elements have events that they control or are controlled by. These are generally hard set, such as a button having clicked, Pressed, Released. Entry element has TextChanged. You can manipulate these events using XAML tags by creating a fairly simple method that says something like OnClicked... These elements must obey the following:

- Must be void,
- Must take two params,
    - An Object sender and EventArgs.
- They are usually private and can be async.

### Event Subscribers

If you would like to pipe the event handling to a code behind, you should use an event subscriber call. This is done using a `+=` indicator. You might write this for example:

`Counter.Clicked += OnCounterClicked;` and then later implement it using `private void OnCounterClicked(object sender, EventArgs e) {}`

You can also use this when subscribing to multiple things.
