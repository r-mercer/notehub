# Swift Notes

## Navigation Link

Steps for Resolution of this Issue:

- Stick Everything in a NavigationStack
- wiggle things around a little bit
- hope that hidden() and isActive() are the same thing...

Also then just use this as a template:

`                    NavigationLink(
                        destination: ,
                        label: {
                            Text("Trigger").hidden()
                        }
                    ).isDetailLink(false).disabled(self. )`

I am honestly still not totally sure what on earth I am doing, but I am slowly wrapping my head around the syntax.

Each navigation stack is effectively a div or horizontal stack that you can use as a window to display more / different content. Similar prehaps to maui views. 

Not totally sure about the idiosyncracies of the .navigationDestination or w/e, but I do agree that the new syntax does actually make more sense.

## Things to Confirm

I should confirm the following on Monday:

1. What on earth I have done wrong with the search function thing in both the driver licence and name / dob search
    1.1 What does this look like in the original / master branch version?
2. Do I need the `isDetailLink(false)` now? Or is it vestigial??
3. I should run the app and click around on the different buttons whilst actually watching the output logs
