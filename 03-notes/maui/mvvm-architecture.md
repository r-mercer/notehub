# MVVM Architecture Notes

There are three parts of the MVVM patter, the model the view and the view model. 

In this pattern, the view sends data bindings and commands to the view model, which updates the underlying model. The model is also able to sent notifications to the view model, which is able to send notifications to the view. 

At a high level, the view 'knows about' the view model and the view model 'knows about' model. But the inverse is not true, the model should not be aware of the view model or the view and the view model should operate completely independantly and happily of the view.

## Views and ViewModels

When following MVVM, the ViewModel should not know anything about the view. However, the view is perfectly capable of knowing everything about the ViewModel.

You may call a method from the ViewModel at any time, in response to an event or as you please. But The View holds the reference to the ViewModel, so if you make a timer and call a method to update the data, you will not violate MVVM principals.


