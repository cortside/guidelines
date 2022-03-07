# Coding Standards

## File Naming & Conventions

* Use the standard extension for the file type (.java, .cs, .xml, .vb, etc)
* One file will contain only one class (which may contain inner classes)
* The file name will be the name of the class (cased the same)
* There will be one namespace per file

## Organization within a code file

* file level comment
* using statements
* namespace
* class level comment
* class, interface, or type definition
* class level static variables
* class static initializer
* instance variables
* constructors
* methods (public may precede private, but is not required by the standard)

Initialize all static or instance variables in line at time of declaration

## Line splits

* At around 132 columns, a long line should be broken
* The second and subsequent lines after a break should be indented two indents from the start of the first line
* Do not break lines unless line length is extended

## Indenting

* An indent will be 4 spaces
* At 8 spaces an indent will be replaced with a single tab

## Comments

* All public or protected methods should have a documentation header (/** for Java and /// for C#)
* implementation comments will be used for all difficult sections of code
(if it took you time to think about how to construct it, then it needs an implementation comment)
* If a comment is too lengthy to be placed at the end of a line, it should be given its own line at the same
indentation level as the code it is documenting and should immediately precede the code being documented

## Declarations

* One declaration per line
* Initialize when declared
* Declare in-line at first usage
* Never over-ride existing variable names
* Variable names must be descriptive unless solely used within a few lines of the declaration
* No space between name and (
* Open a block at the end of a line
* Single statement blocks will use block delimiters
* Empty blocks will close the block on the next line

## Naming

* No prefixes
* Change case between words of a multi-word variable or method name (do not use symbols between words)
* First character will be upper case for all methods, namespaces, and classes
* First character will be lower case for all variable names
* Exception:  Constants should be all upper case, using _ between words in the name

## Misc

* Curly brace open on the contruct line
* tab of 8 characters, indent of 4
* Camel casing of names (property, method, namespace)
* No hungarian notation
* Methods, classes, and properties all start with a capital letter
* No underscores in method or variable names
* Methods begin with a verb
* One class per file (except inner classes)
* Class name and filename should match
* In a service app, you should not have your whole app in main.cs
* Namespace needs to be in every file
* Imports (using) come before namespace
* Use C#
* Directory and namespace hierarchy match ( /src is omitted )
* FCL class names instead of language specific aliases (INT32 instead of INT)
* Only handle expeptions you plan to specifically deal with
* Facade adapter and errorable controller should catch all exceptions and log them
* All public methods have a comment
* Don't check in code that has warnings


# Microsoft Coding Conventions

* https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
* https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/naming-rules
* https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/general-naming-conventions
* https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines
