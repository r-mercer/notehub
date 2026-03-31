# Platform Implementations

## Handlers

Honestly don't worry about it

## Compile time Bindings?

In the code behind you can use
`#IF WINDOWS`
and that will endure that the logic that has failed the test will not be compiled

## Runtime

write up some notes on that link: https://blog.ewers-peters.de/platform-specific-xaml-in-net-maui

`if (DeviceInfo.Platform == DevicePlatform.Android)
{
    VerticalLayout.Add(new Android.ViewAndroid());
}
else if (DeviceInfo.Platform == DevicePlatform.iOS)
{
    VerticalLayout.Add(new iOS.ViewiOS());
}`

## How to get around disctinct name requirements

If you create a placeholder map componenet (grid or similar) then you can simply add your platorm specific implementation to this componenet at compile time. It is more annoying to name it this way, but you can still add a Map property and then access it that way.

## Handlers (Additional Notes)

What on earth is the point of using empty handlers?

## Device Detection (Additional Notes)

The Device class is a utility class that provides device specific info regarding where your app is being run. The most important property for this is called `DeviceInfo.Platform`. This will return a string indicating the platform your app is currently being run on.

You can query the above and then use standard conditional logic to manipulate it.

You can do the equivalent thing in XAML using the OnPlatform extension. Which allows you to detect the platform at runtime. It looks kinda like this:
```xml
<VerticalStackLayout>  
    <VerticalStackLayout.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="30,60,30,30" />
        </OnPlatform>
    </VerticalStackLayout.Padding>
    <!--XAML for other controls goes here -->
</VerticalStackLayout>
```

> [!NOTE]
> You can use TypeArguments attribute to specify which argument, because out of the box, OnPlatform is generic!
