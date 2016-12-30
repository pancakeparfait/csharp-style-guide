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
