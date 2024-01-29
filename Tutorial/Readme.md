# Tutorial

### Overview

This tutorial provides a step-by-step approach to understand different techniques and how to use the Roslyn API to generate objects in .NET applications.



This tutorial is laid out with folders which are label in order of importance and what will be discussed.  Each step is then built upon by the next, so it would be wise to step through each one the first time through this tutorial.

Each folder has an additional Readme.md file that discusses in greater detail the purpose of the step. Additionally, you can use a tool like WinMerge to diff the folders in order to see the important changes.

The source code in each step should be examined to fully understand what is being done.

</br>

### Requirements
The majority of this tutorial will apply to either Visual Studio or VS Code.
However, I'm not a VS Code guy so you may have to do some research on your own as to how to proceed.

The 2 areas that may be a problem are
- **Tooling in general** - In a minute, you will see an option you will need for Visual Studio.  VS Code may have a similar requirement.
- **Step 4 - Debugging** - This may only be available in Visual Studio.

These are the minimum requirements given the implementation, using method **ForAttributeWithMetadataName**, that I will be showing.

|              | Version                         |
| ------------ | ------------------------------  |
| Roslyn       | 4.3.1                           | 
| .NET SDK     | 6.0.400                         | 
| C#           | 10.0                            | 
| Visual Studio| 17.3 (.NET 6) / 17.8 (.NET 8)   | 
| C# VS Code   | 1.25.0                          | 

After upgrading Visual Studio to at least 17.3, you will have to add an additional feature through the installer.
Individual Components -> Compilers, build tools, and runtimes -> .NET Compiler Platform SDK

</br>

### Background on Source Generators
A Source Generator is code that runs during the compilation of your code and produces code to add additional or changes functionality. This is done by examining your code 

- namespaces
- classes
- properties / fields
- attributes
- interfaces
- etc.

</br>

### What we will be building
The output of the Source Generator that I will be discussing will build Object Mothers.  Martin Fowler describes them as ***"An object mother is a kind of class used in testing to help create example objects that you use for testing."***

The output will allow you to create one or more methods that you can then use repeatedly for multiple tests.


````
public static AddressBuilder Typical()
{
    return new AddressBuilder()
        .WithAddress1(GetRandomValue.String(50))
        .SetDefaultAddress2()
        .WithCity(GetRandomValue.String(50))
        .WithState(StateBuilder.Typical().Build())
        .WithPostalCode(GetRandomValue.String(50))
        ;
}
````