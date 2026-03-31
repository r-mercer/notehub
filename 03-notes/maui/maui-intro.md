# MAUI 
The purpose of .NET MAUI (Multi-platform Application User Interface), is to consolidate development for multiple platforms into one place.

## The MAUI Tech Stack

There are a variety of .NET frameworks for each platform. These frameworks have access to the same base class library (BCL). This provides the functionality for abstracting the underlying device away from the general business code. 

Mono is an open source implementation of the .NET runtime. This is how we get Android, iOS and MacOS deployments. Win32 does Windows.

These tools allow for an app's business code to be shared to anyplatform and for platform specific features to be implemented as required. 

## MAUI App Structure

### App.xaml

Defines the resources that the application uses. Resources are located in the Resouces folder and define app wise things like colours and default styles. Then we merge the resources together to consolidate them. 

#### App.xaml.cs

The code behind file for the app.xaml file. Defines the app class and represents your application at run time. The constructor in this class creates the first window and assigns it the main page property. This property defines the starting page for your application and enables you to override common platform neutral app lifecycle event handlers. These include Onstart, OnResume and Onsleep.

### AppShell.xaml 

This is the main application structure. Can include things like app styling, URI based navigation and options for layouts. 

### MainPage.xaml

Contains the user interface definitions. I think this is approximately equivalent to index.html. If you have multiple pages, you would add in more {name}Page.xaml and use their corresponding code behind pages to define the logic.

#### MainPage.xaml.cs 

This is the code behind for your page. This is where the logic for the various event handlers and actions triggered by page controls lives. This can include things like handlers for button presses and semantic screen reader specifies what is to be announced by a screen reader when encountered. 

### MauiProgram.cs

Each platform has a different starting point that creates and inits that pplilcation. You can find this in the Platforms folder. This is platform specific, but ultimately ends up calling the CreateMauiApp method of  the static MauiProgram class. 

You must specify which class describes your application within the UseMauiApp builder generic method. The app builder provides methods for tasks such as registering fonts, configuring services for DI, custom handlers and more. 

### Platforms/

This is a directory that sorts platform specific init code files and resources. Most of the start up process is abstracted away by the MAUI libraries, but these files provide a way for integrating your own custom code. Once init is complete, the platform specific code will then call MauiProgram.CreateMauiApp which creates andruns the App object as outlined earlier. 

## Project Resources

### .csproj File

There are several interesting parts of this file:

- The PropertyGroup first specifies the frameworks that the project targets,
- Underneath that the initial ItemGroup specifies styling for the splash screen,    
