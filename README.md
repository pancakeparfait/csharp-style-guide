# C# Style Guide

## Purpose

## Credits
This style guide is unabashedly modeled from [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md).

## Table of Contents

  1. [Single Responsibility](#single-responsibility)
  1. [Services](#services)
  1. [Processors](#processors)
  1. [Controllers](#controllers)
  1. [Naming](#naming)
  1. [Unit Testing](#unit-testing)
  1. [Integration Testing](#integration-testing)
  1. [Comments](#comments)

## Single Responsibility

### Rule of 1
###### [Style [SC001](#style-sc001)]

  - Define 1 component per file, recommended to be less than 400 lines of code.

  *Why?*: One component per file promotes easier unit testing and mocking.

  *Why?*: One component per file makes it far easier to read, maintain, and avoid collisions with teams in source control.

  *Why?*: One component per file avoids hidden bugs that often arise when combining components in a file where they may share variables, create unwanted closures, or unwanted coupling with dependencies.

  ```csharp
  /* avoid */
  
  // AvengersController.cs
  public class AvengersController : Controller { }

  public class AvengersService : ServiceBase { }
  ```

  The same components are now separated into their own files.

  ```csharp
  /* recommended */

  // AvengersService.cs
  public class AvengersService : ServiceBase { }
  ```

  ```csharp
  /* recommended */

  // AvengersController.cs
  public class AvengersController : Controller { }
  ```

  ```csharp
  /* recommended */

  // Avengers.aspx.cs
  public class Avengers : Page { }
  ```
  
**[Back to top](#table-of-contents)**

### Small Functions
###### [Style [SC002](#style-sc002)]

  - Define small functions, no more than 75 LOC (less is better).

  *Why?*: Small functions are easier to test, especially when they do one thing and serve one purpose.

  *Why?*: Small functions promote reuse.

  *Why?*: Small functions are easier to read.

  *Why?*: Small functions are easier to maintain.

  *Why?*: Small functions help avoid hidden bugs that come with large functions that share variables with external scope, create unwanted closures, or unwanted coupling with dependencies.

**[Back to top](#table-of-contents)**

## Unit Testing
Unit testing helps maintain clean code, as such I included some of my recommendations for unit testing foundations with links for more information.

### Write Tests with Stories
###### [Style [SC060](#style-sc060)]


**[Back to top](#table-of-contents)**

### Testing Libraries
###### [Style [SC061](#style-sc061)]

* Use [MSTest](https://msdn.microsoft.com/en-us/library/hh694602.aspx) attributes.

  *Why?*: MSTest attributes are supported by the native Test Explorer and Visual Studio Team Services build process, as well as ReSharper's Test Runner.

* Use [Moq](https://github.com/Moq/moq4/wiki/Quickstart) for new development. Supplement with [Microsoft Fakes](https://msdn.microsoft.com/en-us/library/hh549175.aspx) to aid in covering otherwise untestable legacy code.

  *Why?*: Moq is a robust library used by much of the .NET community. Fakes is available to [users with Visual Studio Enterprise licenses](https://www.visualstudio.com/vs/compare/) and can shim otherwise untestable static methods. Both are stable, well maintained, and provide robust testing features.

**[Back to top](#table-of-contents)**
  
### Test Runner
###### [Style [SC062](#style-sc062)]

* Use ReSharper Test Runner.

  *Why?*: ReSharper Test Runner has an incredibly user-friendly interface that includes a Projects and Namespaces tree grouping of unit tests that is easy to navigate.
  
  *Why?*: The Cover Tests option offers a visual aid (Toggle Code Highlighting) to determine whether unit tests cover all implementation logic.

**[Back to top](#table-of-contents)**

### One Test Class Per Function
###### [Style [SC063](#style-sc063)]

* Create one test class per function under test.

  *Why?*: This adheres to SRP. One class tests one function.
  
  *Why?*: Test classes tend to be much bigger than their class under test, so this keeps LOC per file lower.

* Name the test class the same as the function under test.

  *Why?*: This adds clarity to which function is being tested.
  
  *Why?*: A solution-wide search for the function by name will also discover its test class.

```csharp
/* class under test */
public ArPaymentService : ServiceBase
{
    /* function under test */
    public ArPayment GetArPaymentById(int id) {
        // ...
    }
}
```

```csharp
/* single test class */
[TestClass]
public class GetArPaymentById : ArPaymentServiceTestsBase
{
    // ...
}
```

**[Back to top](#table-of-contents)**

### Tests Base
###### [Style [SC064](#style-sc064)]

* Create one abstract base class for each Test Class Per Function to inherit.

  *Why?*: This reduces repetition in test arrangement.
  
  *Why?*: Future refactors to the class under test dependencies can be maintained in one place.

* Expose a protected member which is the concrete class under test (```ArPaymentService``` in the example).

  *Why?*:

* Expose a protected Mock of each dependency (```IAxisContext``` in the example).
* Instantiate each

```csharp
[TestClass]
public abstract class ArPaymentServiceTestsBase
{
    protected ArPaymentService Service;

    protected Mock<IAxisContext> Context;

    [TestInitialize]
    public virtual void Arrange()
    {
        Context = new Mock<IAxisContext>();

        Service = new ArPaymentService(
            Context.Object);
    }
}
```

**[Back to top](#table-of-contents)**
