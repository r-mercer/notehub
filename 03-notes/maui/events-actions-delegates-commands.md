# Maui Events, Actions, Delegates, and Commands

I am including this here as its own seperate document, because honestly these things are freaking me out and I want to work through what they are.

## Events


## Actions

## Commands
You need to pass a Command param to a button to trigger it on click.

If you particularly need to go from view to view model, you can use commands. Commands enable you to execute strings of logic without needing to get your hands dirty.

### ViewModel to View Communication

You can use `NotifyPropertyChanged` to communicate from the ViewModel back to the View. The view is presumably subscribed to this change, which is then used to call methods.

Alternatively, you can pass the ViewModel into the View when constructing it:

```csharp
// In your view
var vm = new MyViewModel();
var view = new MyView(vm);
```

This means that a new ViewModel is created with the view, so you can use the ViewModel methods from the view.

## Delegates

