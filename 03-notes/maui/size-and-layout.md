# Layout 

## Layout Panels

A layout panel is a container that holds a collection of child views and determines their size and position. These recalculate when the view window changes. This ensures that you make consistent views across devices. 

The most common options are: 

- StackLayout
- Absolute Layout
- Grid
- FlexLayout

## Sizing

You can configure the size of elements using the Height/Width Request() property. This allows an element to specify a preferred size but its true size is only allocated at run time, hence the application does not need to listen to the element.

You set size values using double values like 100. However these values do not really have any meaning, they are only translated to units once they hit the device OS. 

Given the weirdness of diamensions, it can be slightly weird to know exactly how big a thing is. This is where the View vase class comes in, and defines two properties called Width and Height. These represent the rendered diamonsons of a view. 

## Position

To set positions within a view, you can use the VerticalOptions and HorizontalOptions. Using these properties is more annoying becauethey're of the type layout options.

### Layout Options

A LayoutOption is a tyhpe that encapsulates Alignment and Expands. 

#### Alignment

The LayoutAlignment optoins specify how things fill a space. There are the following keywords which all sorta do what you'd expect:

- Start
- Center
- End
- Fill

Then there's also expands which doesn't do anything anymore.

## StackLayout

 Container that organises its children either left to right or top to bottom. Controlled through the organisation property.

- Has a list of `children` that it inherits from its base class `Layout<t>`. 
    - Derives from View as do other components so they nest nicely.
- This and horizontal layouts are the preferred options because they fast as fuck.
- Elements are ordered relative to their order in the parent element.
- Control spacing between objects with the spacing attribute.
- Changing the orientation of the list will cause it to change up the Height/Width Request option that is used.


## Grid Layout

Can be tedious and inefficient to code a grid using several Stack Layouts nested together. Should actually use a Grid:

- Grids are composed of rows and cols, you place things in the intersection of the two.
- Within a grid you have collections of Row Definitions and ColumnDefinitions.
    - These are objects that define the shape of your grid and represent those cols / rows within the grid.
- They have the standard height / length, but also GridLength:
    - Can be Absolute, Auto or Star
- Absolute is simple.
    - Though should be noted that if you use a numeric val for your Definition, it will be set to absolute automatically.
- Auto sizes things to fit.
- Star makes things fit proportionately. Using the total available space for each row or col.
    - To do this, it scans the non-star elements, then distributes that space across each star sized element.
        - You can also include a value multiplier to add further specificity.

A set of GridDefinitions will look like this: 

`<Grid>
    <Grid.RowDefinitions>
        <RowDefinition />
        <RowDefinition />
        <RowDefinition />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>
    ...
</Grid>`

When adding views to a Grid, observe these guidelines:

- Cols and Rows are 0 indexed from the top left corner.
- You specify both col and row to add the view to the grid.
- Best way to insert a view into a grid is to use the attached property.
    - This is a property defined in one class but set on objects of another type. 
        - Can be thought of as key value pairs. 
    - When you add to a grid, the grid will check the collection to find a matching grid coords, then add it to the grid.
- Span properties will let you move across multiple cols / rows.
