# Development Standards and Guidelines (C#)

## About the Guide
This guide covers structuring and naming of the Apptis/Project libraries in such a manner that other developers/teams can easily determine whether or not an object that provides desired functionality already exists, easily locate objects slated for modification, and decide where to include new development.  The guide also covers software coding standards; and although the primary focus will be on programs written in C #, many of the rules and principles are useful and apply to programs written in other languages.  Finally, the guide will briefly cover embedded XML documentation and includes a more extensive elaboration on usage in [Appendix A] (https://github.com/CA-CST-SII/Software-Standards/blob/master/C%23_Standards.md#appendix-a---xml-documentation-tags) - taken from a white paper article written by Anson Horton from the .NET Framework community website. 

## Purpose
Project specific C# coding standards are maintained by the lead developer for each project.

It is expected that all new C# code will adhere to these standards; existing code can be retrofitted to meet the standards to whatever degree possible, as modifications to that code are required. 

## Library Organization
### Library Taxonomy
The taxonomy presented in this section is a partial library layout with some explanation.  

Structure and usage will be discussed in the subsequent sections.  
* Apptis
  * Common – Houses the company’s core objects that are not “Account” specific (e.g., the Data Access Layer) and may be shared across multiple projects/accounts.
    * DataAccess
      * DBConnection.cs
      * DBConnectionFactory.cs
      * …
    * Hardware
      * Factory
        * …
    * Helper
      * NumericString.cs
      * XMLHelper.cs
      * ParameterizedString.cs
      * TokenValue.cs
      * …
    * MoneyManagement
      * MoneyManager.cs
    * SecurityManagement
      * SecurityManager.cs
    * UI
      * Web
        * … 
      * Windows
        * DesignSuite
    * Utils
      * Settings
  * Passport – Houses account specific common objects	
    * FEPClient
  * PATS
    * …
  * PIERS
    * …
  * PRISM
    * …
  * TDIS
    * Common – Houses project specific common objects
      * DataAdapter
      * DataFormatter
      * Passport
      * Project specific business entity objects.
      * PassportManagement
      * Project specific business entity management objects.
    * Support
      * SecurityManagement
      * SettingsManagement
    * Workflow
      * Adjudication
      * SignatureService
      * QualityControl

###Library Structure###
The library directory should be structured and named in a manner that makes it intuitive for a developer to easily check for existing functionality.  In general namespaces should match with the corresponding code directory structure (e.g., Apptis.TDIS.Common.Passport would align with the Apptis/TDIS/Common/Passport path).  This makes it easier to map namespaces to the directory layout, and makes it a relatively trivial matter for a developer to look for the appropriate “using” directive in a code file and locate the underlying functionality.  

However, this is not to say that every folder MUST map to a namespace.  If you’re creating a sub-folder within a project for organizational purposes, it is recommended that the objects within these folders share the same root namespace as the project (e.g., items within the Apptis/TDIS/Common/Passport/Applicant folder would lie within the Apptis.TDIS.Common.Passport namespace).  Again, this greatly simplifies library inclusion with the “using” directive and may vastly reduce the impact of a library consolidation and re-organization.

###Library Usage###
The first place a Team/Developer should look for developing a NEW system or Project is in the Apptis.Common section of the library.  If there is no existing functionality for the GENERAL type of development they are performing and it is conceivable that other systems may need the functionality at some future point, it should be implemented within the Apptis.Common section for other Projects to use.  Likewise, this may also be a good time to investigate whether other Developers/Teams have implemented functionality similar to what is needed, and if so, considering abstracting it out into the Apptis.Common section of the library.  

When adding/extending functionality to an existing Project, a developer should be able to look in the module folder (i.e., look in Apptis.TDIS.ModuleName if modifying an existing module) to see how current functionality has been implemented, and look to the Project and Apptis Common folders for existing business or non-domain specific objects.  If the folder structure is laid out in a sensible fashion and the underlying objects carry with them meaningful names, it should be relatively easy to determine whether or not there are existing pieces to the puzzle they are working on.  Again this is also a good time to look for existing functionality that is not in the Common area and re-factor and move it out if possible.  

##Programming Standards##
###About Code Uniformity###
Whereas most developers are familiar with writing code that a compiler can interpret, this section may be read as a guide that focuses on helping developers adopt code Naming Conventions, Structure, and Documentation/Comments that will help other developers understand the purpose of existing code.  Typically it is much simpler for a developer to familiarize their self with new code that has naming conventions and formatting similar to their own.  

###Naming Conventions###
####Naming Overview####
In general, put every class in a separate file and name the file like the class name.  An exception to this would be private inner classes.  Class names typically describe the business entity that the class represents or the function or set of functionality the class provides.  This convention makes things much easier to follow and significantly lends to self-documenting code.  Class and file names should NOT include the company and/or project name (e.g. GDSSecurity, TDISPassport, etc.).  Although this may have made some small amount of sense for COM objects, the combination of appropriate module name selection and the namespace paradigm used throughout the .NET framework make this unnecessary.   An obvious exception to this would be Bridge or Adapter classes where the name denotes the systems being bridged (e.g., TDISFEPAdapter).

For classes/files/methods keep your source code short, divide your code up into methods that are named such that their purpose is clear.  Unusually long classes/methods are sometimes cumbersome to name (not to mention read through) as they often perform multiple semi-atomic tasks.  This usually a good sign that some method and/or class refactoring is in order.

####Hungarian Notation####
Generally, the use of Hungarian notation in the naming objects/variables is considered poor practices in terms of modern programming standards.  

Hungarian notation is a defined set of prefixes and suffixes, which are applied to names to reflect the type of the variable.  This style of naming was widely used in early Windows programming, but now is obsolete or at least should be considered deprecated.  Using Hungarian notation is not allowed if you follow this guide.

Instead modern standards call for names that denote usage rather than type.  In other words a good variable name describes the semantic. 

One exception to this rule is UI code. All fields and variable names that contain UI elements such as a button should be suffixed with their type name without abbreviations. For example:

```C#
System.Windows.Forms.Button cancelButton;
System.Windows.Forms.TextBox firstNameTextBox;
```

#### Capitalization Styles
##### *Pascal Casing*
This convention capitalizes the first character of each word (e.g., 

<code>StandardsAndGuidelines</code>).

##### *Camel Casing*
This convention capitalizes the first character of each word except the first one (e.g. 

<code>standardsAndGuidelines</code>).

##### *Upper case*
Only use all upper case for identifiers if it consists of an abbreviation or acronym that is only a few characters long.  Longer identifiers should use Pascal Casing instead.

For Example:
```C#
public class Math
{
    public const PI # ...
    public const E # ...
    public const FeigenBaumNumber # ...
}
```

####Naming Guidelines####
##### *Class Naming Guidelines*
* Class names must be nouns or noun phrases.
* Use Pascal Casing.
* Do not include any underscore.
* Do not use any class prefix.

##### *Interface Naming Guidelines* 
* Name interfaces with nouns or noun phrases or adjectives describing behavior. (Example IComponent or IEnumberable)
* Use Pascal Casing.
* Do not include any underscore.
* Use I as prefix for the name, it is followed by a capital letter (first char of the interface name)

##### *Enum Naming Guidelines* 
* Use Pascal Casing for enum value names and enum type names.
* Don’t prefix (or suffix) an enum type or enum values.
* Do not include any underscore.
* Use singular names for enums.

##### *Static and Const Field Names* 
* Name static fields with nouns, noun phrases or abbreviations for nouns
* Use Pascal Casing.

##### *Non- const Field /Member Variable Names* 
* Use descriptive names that, if done properly should be enough to indicate the variable meaning/usage and provide insight into the underlying type.
* Use an underscore “_” as a prefix for private and protected class member variables (declared at the top of the class above any constructors) so they can be easily identified and located.
*Use Camel Casing after the underscore.

##### *Method Names*#####
* Name methods with verbs or verb phrases.
* Use Pascal Casing.
* Do not include any underscore.

##### *Method Parameters and Local Variables*#####
* Use descriptive names that, if done properly should be enough to indicate the variable meaning/usage and provide insight into the underlying type.
* Use Camel Casing.

##### *Property Names*#####
* Name properties using nouns or noun phrases.
* Use Pascal Casing.
* Do not include any underscore.
* Consider naming a property with the same name as the underlying class variable (without the underscore prefix).

##### *Event Names*#####
* Name event handlers with the EventHandler suffix.
* Use two parameters named sender and e.
* Use Pascal Casing.
* Do not include any underscore.
* Name event argument classes with the EventArgs suffix.
* Name event names that have a concept of pre and post using the present and past tense.
* Consider naming events using a verb.  Consider using the “On” prefix, e.g., OnStart.

##### *Namespace Names*#####
* Use Pascal Casing.

##### *Private/Public Fields Names*#####
* Do not include any underscore.

##### *Controls Names*#####
* Use naming conventions (Exmaple btn* for buttons, chk* for check boxes, cmb* for combo boxes)
* Do not include any underscore.

##### *Exception Names*#####
* Use Pascal Casing.
* End with X.

#####*Capitalization summary*#####

<table>
<caption>style#“caption-side:bottom;”|<em>Table 4.2.4.10 Capitalization Summary</em></caption>
<thead>
<tr class="header">
<th align="left"><p>Type</p></th>
<th align="left"><p>Case</p></th>
<th align="left"><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Class/Struct</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>Interface</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"><p>Start with I</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Enum values</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>Enum type</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>Events</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>Exception class</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"><p>End with Exception</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Public Fields</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>Methods</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>Namespace</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>Property</p></td>
<td align="left"><p>Pascal Casing</p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>Protected/Private Fields</p></td>
<td align="left"><p>Camel Casing</p></td>
<td align="left"><p>Start with “_” for class level</p></td>
</tr>
<tr class="even">
<td align="left"><p>Parameters/Local Variables</p></td>
<td align="left"><p>Camel Casing</p></td>
<td align="left"></td>
</tr>
</tbody>
</table>

###Code Structure###
####Code Separation####
#####*Separation Overview*#####
Imagine trying to read a book or a magazine article with no spaces between the words or any 
indentation and/or blank lines to set-off or separate paragraphs.  It becomes readily 
apparent just how powerful these simple formatting tools are in contributing to the flow and 
readability of the text.  Similarly, the proper usage of indentation, blank lines, and white 
spaces will vastly improve the flow and readability of source code. 

#####*Indentation*#####
An indentation standard using spaces never was achieved.  Some people like 2 spaces; some 
prefer 4 and others 8, or even more spaces.  For this reason it is better use tabs and we 
define the Tab as the standard indentation character.  Tab characters have some advantages:

* Everyone can set their own preferred indentation level
* It is only 1 keystroke and not 2, 4, or 8…and will therefore reduce typing (even with “smart 
indenting” you have to set the indentation manually sometimes, or take it back or whatever).
* If you want to increase the indentation (or decrease), mark one block and increase the 
indent level with Tab with Shift-Tab you decrease the indentation. This is true for almost 
any text editor.

Indentation should be used in the following manner:

* Subordinate scopes/clauses should be indented one level in from the next higher scope (see 
Code Examples).
  * The class, interface, struct, enum, and delegate declarations should be one level in from 
the namespace declaration.
  * Constructors, Fields, Properties, Methods, etc. should be one level in from the class 
declaration.
  * Source code within Constructors, Properties, Methods, etc. should be indented one level.
  * Subordinate statements within conditional and flow statements should be indented one level 
(see Formatting Conditional/Flow Statements).
* If a declaration or statement continues onto subsequent lines the “wrapped” portion should 
be indented (see Wrapping Lines).

#####*Blank Lines*#####
Blank lines used within and between Methods, Properties, etc. improve readability.  They set 
off blocks of code that are logically related.

Two blank lines should always be used between:

* Logical sections of a source file (e.g., between code regions).
* Class and interface definitions (use one class/interface per file to prevent this case).

One blank line should always be used between:

* Methods
* Properties
* Local variables in a method and its first statement
* Logical sections inside a method to improve readability

#####*Spacing*#####
The following spacing rules should be followed:
* No space between a method name and the opening parenthesis "(".
* No space between the parentheses for methods with no params in the signature.
* All items in a Class declaration should be separated by spaces (see Class, Interface, and 

Method Declarations).
* All parameters/expressions within parentheses should be separated by spaces located after 

the opening parenthesis, after the commas or semicolons, and before the closing parenthesis.
* Nested parentheses should be separated by spaces from the outer parentheses as well as the 
contained expression.
*No spaces should be used within parentheses that denote an explicit object cast.
*Surround operators with single spaces (except unary operators like increment or logical 
not).

Method example:

Use:
```C#
TestMethod();
```
Or
```C#
TestMethod( paramName1, paramName2, paramName3 );
```

Don't use: 
```C#
TestMethod(paramName1,paramName2,paramName3);
```
Or
```C#
TestMethod(paramName1, paramName2, paramName3);
```

Flow example:

Use:
```C#
for( int loopIndex # 0; loopIndex < 10; loopIndex++ )
```

Don't use:
```C#
for(int loopIndex#0; loopIndex<10; loopIndex++)
```
Or
```C#
for(int loopIndex#0;loopIndex<10;loopIndex++)
```

Nested parentheses example:

Use:
```C#
if( ( ( printStatus & PrintError ) !# PrintError ) ||
    ( ( printStatus & PrintBypass ) ## PrintBypass ) )
{
	…
}
```

Don’t use:
```C#
if( ( (printStatus & PrintError) !# PrintError ) ||
    ( (printStatus & PrintBypass) ## PrintBypass ) )
{
	…
}
```
	Or
```C#
if( ((printStatus & PrintError) !# PrintError) ||
    ((printStatus & PrintBypass) ## PrintBypass) )
{
	…
}
```

Explicit cast example:
```C#
double someDouble # 1234.7;
int someInt;
```
Use:
```C#
someInt # (int) someDouble;
```
Or
```C#
someInt # (int)someDouble;
```

Don't use:
```C#
someInt # ( int )someDouble;
```

Operator example:

Use:
```C#
prevVariable # curVariable;
```

Don't use:
```C#
prevVariable#curVariable;
```

####Wrapping Lines####
When an expression will not fit on a single line, break it up according to these general 
principles:

* Break after a comma.
* Break after an operator.
* Prefer higher-level breaks to lower-level breaks.

Example of breaking up method calls:
```C#
MethodCall( variableName1, variableName2, variableName3,
	variableName4, variableName5 );
```
Or
```C#
MethodCall( variableName1, variableName2, variableName3,
            variableName4, variableName5 );
```

Examples of breaking an arithmetic expression:

Use:
```C#
someResult # a * b / ( c - g + f ) + 
4 * z;
```
Or
```C#
someResult # a * b / ( c - g + f ) + 
      4 * z;
```

Don’t use:
```C#
someResult # a * b / ( c - g +
f ) + 4 * z;
```
The first is preferred since the break occurs outside the parenthesized expression (i.e., 
flows and reads better with the order of operations in mind).  


NOTE: If you prefer to use indentation that lines variables and/or the right hand portion of 
expressions up with those on the previous line, then you MUST use tabs only to where the 
previous line began and spaces for the rest of the indentation.  Otherwise, formatting will 
be disrupted for developers using tab lengths that differ from your own.  

Examples of using tab/char combinations to preserve alignment:
```C#
>MethodCall( variableName1, variableName2, variableName3,
>............variableName4, variableName5, variableName6 );

>var # a * b / ( c - g + f ) +
>......4 * z;
```
Where `'>'` are tab chars and `'.'` are spaces (the spaces after the tab char must begin under 
the first char of the previous line).  For this reason it may be easier to simply indent with 
an additional tab rather than align continuations of variable lists and expressions.

####Declarations####
#####*Number of Declarations per Line*#####
One declaration per line is recommended since it allows for commenting should the variable 
name not suffice.  In other words,

```C#
int floorNumber; 	
int totalFloors; // The total number of floors in the building
```

Do not put more than one variable on a line unless their usage is closely related.  Do not 
put variables of different types on the same line when declaring them.  Example:

```C#
int a, b; // What is 'a'? What does 'b' stand for?
```
The above example also demonstrates the drawbacks of non-obvious variable names.  
#####*Avoid declaring public Fields*#####
Public Fields (defined in a class) should not be used. Public Fields can be accessed by any other Class, therefore its value can be modified at any time, without control by the Class itself.  In addition, direct use of Public Fields does not let Field definition evolve without requiring updates to all Objects referencing it.  This goes against OO Encapsulation concepts.

#####*Initialization*#####
Wherever possible try to initialize local variables as soon as they are declared. 

For example:
```C#
string mLastName # string.Empty;
```
Or
```C#
string lastName # curEmployee.LastName;
```
Or
```C#
double hoursWorked # timePeriod.Hours;
```

Note: If you initialize a dialog try to use the using statement:
```C#
using( OpenFileDialog openFileDialog # new OpenFileDialog() )
{
...
}
```

#####*Class, Interface, and Method Declarations*#####
* The opening brace "{" appears in the next line after the declaration statement.
* The closing brace "}" starts a line by itself indented to match its corresponding opening 
brace.

For example:
```C#
class BoundedCounter : CounterBase, ICounter
 {
  int _upperBound;
  int _lowerBound;
  int _countStart;
		
 public MySample( int countStart, int upperBound, 
  int lowerBound )
 {
  _countStart # countStart;
  _upperBound # upperBound;
  _lowerBound # lowerBound;
 }

 void Increment()
 {
	if( _countStart < upperBound )
	{
          CounterBase.Increment();
	}
 }

 void Decrement()
 {
	if( _countStart > lowerBound )
	{
          // Call the base class decrement
			}
        }

 void EmptyMethod()
 {
	// How’d I end up here?
 }
}
```
For a brace placement example see the Brace Example.

####Formatting Conditional/Flow Statements####
#####*Formatting if, if-else, if else-if else Statements*#####
The if, if-else and else-if else statements should be formatted as follows:
```C#
if( condition )
{
 DoSomething();
 ...
}

if( condition )
{
 DoSomething();
 ...
}
else
{
 DoSomethingOther();
 ...
}

if( condition )
{
 DoSomething();
 ...
}
else if( condition )
{
 DoSomethingOther();
 ...
}
else
{
 DoSomethingOtherAgain();
 ...
}
```
Note: Generally use brackets even if there is only one statement in condition.

#####*Formatting for / foreach Statements*#####
A for statement should have following format:
```C#
for( int loopIndex # 0; loopIndex < 5; ++ loopIndex )
{
 ...
}
```
Or single lined (consider using a while statement instead):
```C#
for( [initialization expression]; [loop condition]; 
[update expression] );
```
A foreach should look like:
```C#
foreach( int i in IntList )
{
 ...
}
```
Note: Generally use brackets even if there is only one statement in the loop.

#####*Formatting  while/do-while Statements*#####
A while statement should be written as follows:
```C#
while( condition )
{
 ...
}
```
Note: Generally use brackets even if there is only one statement in the loop.

An empty while should have the following form:
```C#
while( condition );
```
A do-while statement should have the following form:
```C#
do
{
 ...
} 
while( condition );
```

#####*Formatting switch Statements*#####
A switch statement should be of following form:
```C#
switch( condition )
{
 case A:
  ...
  break;
 case B:
  ...
  break;
 default:
  ...
  break;
}
```

#####*try-catch Statements*#####
A try-catch statement should follow this form:
```C#
try
{
 ...
}
catch( Exception ) 
{
  // We don't care if it fails because we're shutting down anyway.
}
```
NOTE:  If you plan on catching and ignoring the error, a good practice is to place a comment 
in the catch block explaining why.

Or
```C#
try
{
 ...
}
catch( Exception e )
{
 ...
}
```
Or 
```C#
try
{
 ...
}
catch( Exception e )
{
 ...
}
finally
{
...
}
```
####Implementing Structure Standards####
#####*Within Visual Studio*#####
For the most part In Visual Studio 2003 these code formatting practices must be adopted and 
adhered to via the developer’s own diligence.

In Visual Studio 2005 the structure rules described above, along with additional layout and 
formatting rules, must be configured by using the C# Code Options depicted in the screen 
shots below.


TODO:  Screen Shots to be added.


##Best Practices##
###Visibility###
Do not make any instance or class variable public make them private or protected.   Instead, 
use properties if you need to expose a class variable.  You may use public static fields (or 
const) as an exception to this rule, but it should not be the rule.

###Avoid large Classes - too many Constructors and Methods###
Classes should have less than X Constructors and Methods.

###Avoid large Methods - too many Lines of Codes###
Methods should not have more than X lines of code.  Large methods are more difficult to understand, and are a sign of a bad modularity of the code.

###Avoid Classes with a High Lack of Cohesion###
Lack of cohesion implies Classes should probably be split into two or more sub/classes.  Cohesiveness of Methods within a Class is desirable, since it promotes encapsulation.  Low cohesion increases complexity, thereby increasing the likelihood of errors during the development process.  Avoid Classes with a High Lack of Cohesion in Methods (LCOM > X). LCOM is an indicator of a Class whose methods only a few of its fields. 

###Avoid Classes with High Coupling Between Objects###
The Coupling Between Object(CBO) is equal to the fan-out of a Class, that is, the number of other Classes that are referenced through one of its methods or one of its fields. Excessive coupling between objects is detrimental to modular design and prevents reuse.  The larger the number of couples, the higher the sensitivity to changes in other parts of the design and therefore the more difficult the maintenance.  High CBO numbers might indicate that a class has too many responsibilities. Such a class is potential candidate for a refactoring where the class would delegate some the responsibilities to other classes or new classes (Extract Class, Extract Method refactoring). This will increase modularity and reusability.  When refactoring with architecture in mind, the CBO metric can be used to check classes running on the application client that have high coupling.  These classes are then good candidate for a refactoring towards the Session Facade pattern.

###Avoid High Response for a Class###
Avoid High Response for a Class (RFC > X).  RFC is the total number of local methods and remote methods called by methods in the Class.  If a large number of methods can be invoked in response to a message, the testing and debugging of the Class becomes more complicated since it requires a greater level of understanding required on the part of the tester. Reduce the number of local methods and remote methods called by methods in the Class.  Reduce the number of local methods and remote methods called by methods in the Class.

###Avoid Classes with High Weighted Methods per Class###
The Weighted Methods per Class metric is defined as the sum of all the Classes method's cyclomatic complexity.  The number of methods and complexity of methods is an indicator of how much time and effort is required to develop and maintain the object.  For maintainability reasons, High Weighted Methods per Class should not be too high.  Reduce the number  - by splitting the Class in two or moving method to a component Class - or the Cyclomatic Complexity of the method of the non compliant Class.

###Avoid Classes with a High Depth of Inheritance Tree###
The inheritance tree should have at most X levels.  Depth of Inheritance Tree (DIT) is the maximum length of a path from a class to a root class in the inheritance structure of a system. DIT measures how many super-classes can affect a class.  Changing a class requires prior understanding, which, in turn, is more complicated for classes with many methods. Classes that are deep down in the classes hierarchy potentially inherit many methods from super-classes. Moreover, the definitions of inherited methods are not local to the class making it even harder to understand it.  Complete testing requires coverage of all execution paths. The number of possible execution paths of a class increases with the number of methods and their control flow complexity. Classes that are deep down in the classes hierarchy potentially inherit many methods from super-classes. Due to late binding, super-class methods need to be tested again in the sub-class context. This makes it potentially harder to test classes deep down in the classes hierarchy.

###Avoid Classes with a High Public Data Ratio###
Public Data Ratio is the percentage of public fields among all fields. Properties are not considered as fields.  Public fields are accessible by any other Class, therefore their values can be modified at any time, without control by the Class itself. This goes against OO encapsulation concepts.  It is necessary to change the field visibility to private and provide appropriate accessors.  

###Avoid Classes with a High Number Of Children###
High Number Of Children (NOC) is the number of immediate <Sub-Classes> of the Class.  Depth is generally better than breadth in class hierarchy, since it promotes reuse of methods through inheritance.  NOC measures the potential influence a Class has on the design.  Classes with large number of children require more intensive testing as through inheritance an implementation error can potentially lead to many regression bugs. Technical or framework classes which are evolving and will not be changed often should not be concerned by this rule.

###Avoid Artifacts with High Cyclomatic Complexity###
Cyclomatic Complexity is a measure of the complexity of the control structure of an Artifact.  It is the number of linearly independent paths and therefore, the minimum number of independent paths when executing the software.  The effort and time for diagnosis of deficiencies or causes of failures, or for identification of parts to be modified is directly related to the number of execution paths, i.e. the complexity of the control flow.  Analyzability declines with increasing Cyclomatic Complexity. Each modification must be correct for all execution paths.  Cyclomatic Complexity computes the number of the linearly independent paths, a lower bound of all execution paths ignoring multiple iterations.  Changeability declines with increasing Cyclomatic Complexity.  Complete testing requires coverage of all execution paths.  Cyclomatic Complexity computes the number of the linearly independent paths.  Review the design of the Artifact to reduce number of independent paths. E.g.: Reduce the number of conditional statements.

###Avoid Artifacts with High Depth of Code###
Depth of Code is measured as the maximum number of nested control statements in an artifact.  Artifact that contains an IF statement which contains a While loop which itself contains another IF statement will have a Depth of Code of 3 (at least).\nAvoid Artifacts with Depth of Code (DoC) greater than X.  Complex Artifacts are difficult to maintain. Keeping Artifacts small and simple ensures a good readability of the code.  Review the design of the Artifact to reduce the Depth of Code.

###Avoid Artifacts with lines longer than 80 characters###
For better readability and portability (printers, terminals, IDE), Artifacts should not have lines longer than 80 characters.  Consider splitting the line.

###Avoid artifacts with too many parameters###
Avoid artifacts with more than X parameters.  For maintainability and readability reasons, artifacts should not have too many parameters.  Review the design of the artifacts to reduce number of parameters.

###Avoid Artifacts with High Essential Complexity###
Avoid Artifacts with High Essential Complexity (EC greater than X). Essential Complexity measures the number of non-structured independent paths.  Non-structured paths are paths of the control flow graph in which an instruction that interrupts the flow is present.  Essential Complexity equals the minimum number of tests to check the Artifact's non-structured behavior. High EC means more testing and higher risk of errors.  Review the design of the Artifact to reduce the number of non-structured independent paths. E.g.: Reduce the number of BREAK or GOTO statements.

###Avoid large number of String concatenation###
String concatenation resolved at runtime is much slower than using StringBuilder.  Use StringBuilder and StringBuilder.Append() method instead.

###Avoid String concatenation in loops###
When placed in a loop, String concatenation results in the creation and garbage collection of large numbers of temporary objects.  This both consumes memory and can dramatically slow the program execution.  It is recommended to create a StringBuilder before entering the loop, and append to it within the loop, thus reducing the overhead.  It is recommended to create a StringBuilder before entering the loop, and append to it within the loop, thus reducing the overhead. 

###Avoid the use of 'is' inside loops###
The run-time type checking is a time expensive operation and as such should be avoided within loops.  In a more general matter, the use of is operator, run-time type checking might indicate a misuse of Object Oriented programming.  In deed, it is always recommended to design classes and interfaces so client code do not need to use 'is' operator and down-casting. The recommended way is to prefer polymorphism over 'is' operator and down-casting.  Prefer polymorphism  over 'is' operator and down-casting.  It is always recommended to design classes and interfaces so client code do not need to use 'is' operator and down-casting.  The recommended way is to prefer polymorphism over  'is' operator and down-casting.

###Avoid instantiations inside loops###
One of the fundamental OO performance management principles is this: Avoid excessive object creation.  This doesn't mean that you should give up the benefits of object-oriented programming by not creating any objects, but you should be wary of object creation inside of tight loops when executing performance-critical code.  Object creation is expensive enough that you should avoid unnecessarily creating temporary or intermediate objects in situations where performance is an issue.  

###User Interface elements must not use directly the database###
User Interface classes are classes belonging to User Interface namespaces such as WinForms or namespaces used for web pages implementation.  Direct access to the database from the User Interface does not respect the multi-layer architecture principles making the application more difficult to change.  Furthermore, accessing database elements directly from the User Interface prevents access control at the database level. E.g.: use of non-optimized query against the database and can be the source of performance.

###Avoid Namespaces with High Efferent Coupling (CE)###
CE(also known as Outgoing Dependencies or the Number of Types outside a namespace that Types of the namespace Depend on) indicates the number of other namespaces that classes and interfaces in the analyzed namespace depend upon. This is an indicator of the namespace's independence.  Excessive coupling is detrimental to modular design since classes are not independent. A large efferent coupling indicates that a class is unfocussed and may also indicate that it is unstable, since it depends on the stability of all the types to which it is coupled. This prevents reuse since a high coupling possibly indicates a namespace is poorly designed and difficult to understand/maintain. Extracting classes from the original class so the class is decomposed into smaller classes can reduce efferent coupling, this improves modularity and promotes encapsulation.

###Avoid namespaces with High Afferent Coupling (CA)###
Afferent Coupling (also known as Incoming Dependencies and Number of Types outside a namespace that Depend on Types of the namespace) indicates The number of other namespaces that depend upon classes within the analyzed namespace. Afferent Coupling is a time consuming determination of couplings between namespaces, hence showing which namespaces that depend upon each other.  The number of namespaces that depend upon the analyzed namespace is an indication of the analyzed namespace's level of responsibility.  In order to improve modularity and promote encapsulation, inter-object class couples should be kept to a minimum.  If the namespace is relatively abstract then a large number of incoming dependencies is acceptable but the larger the number of couples, the higher the sensitivity to changes in other parts of the design, and therefore maintenance is difficult.  Excessive coupling between concrete object classes is detrimental to modular design and prevents reuse.  If a namespace is highly abstract then it should be very stable.  If the namespace is highly concrete (un-abstract), then it would be acceptably unstable as it already has reached its maximum specialization.  If a category is to be stable, it should also consist of abstract classes so that it can be extended.  Stable categories that are extensible are flexible and do not constrain the design.

###Call 'base.Dispose()' or 'MyBase.Finalize()' in the "finally" block of 'Dispose(bool)' methods###
If a type inherits from a disposable type, it must call the Dispose method of the base type from within its own Dispose method in order to make sure all allocated resources are released properly and timely.  Failing to do so can provoke a resource leak that will  lead to serious application availability and stability issues.  This applies only for Dispose(bool) method defined in classes which implement the IDisposable interface. 

###Dispose() methods should call GC.SuppressFinalize###
Because the cleanup code executed at dispose-time is a superset of the code executed at the finalize-time, there is no need to call the finalize-time code during object finalization after the object has been disposed. Moreover, keeping objects that don't need to be finalized in the finalization queue has a cost associated with it.  This is why the Dispose() method should call GC.SuppressFinalize, which removes the object from the finalization queue and thus prevents unnecessary finalization.

###Declare as static all Methods not using Instance Fields###
If a method doesn't use an instance field or const, it should be declared static except if the method is overriden or override another method.  When an object is created, Memory is allocated to all the fields.  All super class fields are also allocated memory.  All sub class fields, super class fields are initialized.  The constructor is invoked.  Using a static avoid to create an object that takes resources when it is unnecessary.

###Provide a private default Constructor for utility Classes###
Utility classes must have a private default constructor, but must not have other constructors. Default constructors are constructors without any parameters.  Utility classes are static classes: all their fields and methods (excluding constructors) are static (this excludes classes marked as static as this case is handled by the compiler).  Utility classes are not meant to be instantiated because all the functionalities that they provide are accessible without instantiating the class. Instantiating these classes means that the developer has effectively misued the class.  It also consumes memory unnecessarily. Providing a private default constructor ensures that the class is not instantiated.  Add a private default constructor to ensure that the class can't be instantiated.

###Avoid cyclical calls and inheritances between namespaces content###
When two namespaces refer to each other through a call, the result is a circular dependency. Neither namespaces can function without the other, and so neither is reusable without the other.  In some cases redesign may eliminate these dependencies. When circular references are necessary, redesign it to ensure reusability.  The same problem happen when some classes from a namespace A inherit from classes of a namespace B and other classes from namespace B inherit from other classes from namespace A.  This rule can be extended to circular dependencies for more than 2 namespaces (for example a namespace A call a namespace B that call a namespace C, that call namespace.  If there are circular relationships among namespaces, the partitioning is not clear and should be redesigned.

###Avoid calling properties that clone values in loops###
It is about a property accessed inside an iteration statement when the property returns a cloned object.  In this case, multiple identical objects are created.  If this is not the intent, access the property outside the iteration statement. Otherwise, multiple unnecessary objects are created and afterward, these objects must be garbage collected.  This degrades performance, especially in compact iteration statements.  Note that it is possible that such a construct is intended, and in that case this violation is perfectly acceptable.  Whenever possible assign the property to a local variable outside the iteration statement and use the local variable inside the iteration statement.

###Avoid Artifacts with High Integration Complexity###
Integration Complexity measures the number of independent integration paths. Integration paths are paths of the control flow graph in which another object is invoked

###Avoid Interface implementation on Structures###
Interfaces should not be implemented on Structures.  C## allows structs to implement interfaces. However this language feature can produce unexpected results, as structs are value types while interfaces require reference types to interact with. This means that calls to interface methods via implicit boxing operations will modify a copy rather than the original object. When implicit boxing occurs, a copy of the struct is placed on the heap and calls to the interface methods are executed on this copy via a reference.  All changes to the state of the struct object are discarded after that call.  Declare the type as a class rather than a struct.  This is better Object Oriented practice as classes can hide their implementation details using efficient class property encapsulation.  Create a wrapper class that contains the struct as a member - for example a property (for efficient encapsulation), and use the wrapper class to implement the interface

###Avoid unreferenced Interfaces/Classes###
All Interfaces and Classes should be referenced.

###Avoid using String.Empty for empty string tests###
String.Empty should not be used for empty string tests.  If you want to test for empty strings, test the length (compare with 0) of the string instead.  This is over twice as quick. Also as String objects can be null, you can use String.IsNullOrEmpty instead - however, do not compare it to true (ie not if (StringIsNullOrEmpty (myString)  == true), just use: if (StringIsNullOrEmpty (myString)).

###Data Access must be based on Stored Procedure Calls###
By using Stored Procedures the database engine is more able to optimize the access plan and to reuse them. It also limit the parsing phase of the SQL order. This generally result in better performance.  From a security point of view, it is generally safer to use SP rather than dynamic SQL as this limit the risk of having SQL-injection. Transform the SQL into a SP and use parameters. Then call the SP.  Do not transform the SQL in a SP that in turn uses dynamic SQL (e.g. @exec or EXECUTE_IMMEDIATE) as this deny all the benefits of the change.

###Avoid direct access to Database Tables###
Direct access to database Table prevents the control at the database level of accesses. E.g.: use of non-optimized query against the database. Use Stored Procedures instead.

###No embedded, user-facing strings###
No UI elements (Labels, Drop Downs, Text Boxes, Error Messages, etc.) should use text that is 
embedded in the application.  In .Net much of this can be overcome by enabling localization, 
and the remainder should rely on some construct that can display the appropriate text from a 
persistent store such as the Notification layer.

###Avoid Superclass knowing Subclass### 
A Superclass is not allowed to have knowledge of one of its Subclasses.  The Superclass has knowledge of the Subclass if the Superclass directly calls a Subclass-method, uses a Subclass-attribute or refers to the name of the Subclass.  Referencing down the inheritance tree is against Object-Oriented practices. This is an indication of a poor class design and class inheritance.  These practices increase the complexity of the code and decrease maintainability.

###Avoid Artifacts with High Fan-In###
The Fan-In of an Artifact is the number of other Artifacts that are referencing it. When computing the Fan-In of an Artifact, multiple accesses to it from the same Artifact are counted as one access.  The higher the number of reference to an Artifact, the more difficult the maintenance as all referencing Artifacts will have to be updated or tested. Reduce the number of references to the Artifact.

###Avoid having Classes implementing too many Interfaces###
Avoid Classes implementing more than X Interfaces.

###No 'magic' Numbers###
Don’t use magic numbers, i.e. place constant numerical values directly into the source code. 
Replacing these later on in case of changes (say, your application can now handle 32767 users 
instead of the 255 hard-coded into your code in 50 lines scattered throughout your 25000 LOC) 
is error-prone, not productive, and an all around bad programming practice.  Instead declare 
a const variable which contains the number:
```C#
public class MyMath
{
  public const double PI # 3.14159...
}
```
##Code Examples##
###Brace Placement Example###
```C#
namespace ShowMeTheBracket
{
  public enum Test
  {
    TestMe,
    TestYou
  }

  public class TestMeClass
  {
    Test _test;

    public Test Test
    {
     get
     {
       return _test;
     }

      set
     {
       _test # value;
     }
    }

  void DoSomething()
  {
    if( _test ## Test.TestMe )
    {
      //...stuff gets done
    }
    else
    {
      //...other stuff gets done
    }
   }
 }
}
```
Brackets should begin on a new line after:
* Namespace declarations
* Class/Interface/Struct declarations
* Method declarations
* Looping statements with multiple subordinate statements.
* Conditional statements with multiple subordinate statements.

###Variable Naming Example###

Instead of:
```C#
for( int i # 1; i < num; i++ )
{
  meetsCriteria[i] # true;
}
			
for( int i # 2; i < num / 2; i++ )
{
  int j # i + i;
  while( j <# num )
  {
    meetsCriteria[j] # false;
    j +# i;
  }
}
	
for( int i # 0; i < num; i++ )
{
  if( meetsCriteria[i] )
  {
    Console.WriteLine( i + " meets criteria" );
  }
}
```

Try intelligent naming:
```C#
for( int primeCandidate # 1; primeCandidate < candidateLimit; 
    primeCandidate++ )
{
    _isPrime[primeCandidate] # true;
}

for( int factor # 2; factor < candidateLimit / 2; factor++ )
{
    int factorableNumber # factor + factor;

    while( factorableNumber <# candidateLimit )
    {
      _isPrime[factorableNumber] # false;
      factorableNumber +# factor;
    }
}

for( int primeCandidate # 0; primeCandidate < candidateLimit; 
    primeCandidate++ )
{
    if( _isPrime[primeCandidate] )
    {
      Console.WriteLine( primeCandidate + " is prime." );
    }
}
```
Note: Indexer variables are generally named i,j,k, etc.  But in cases like this, it may make 
sense to reconsider this rule.  In general, when the same counters or indexers are reused or 
can provide insight into the functionality, give them meaningful names.

##Embedded Comments and Documentation##
###Avoid uncommented Methods and Methods with a very low comments/code ratio###
Methods should have comments. Methods should have at least a ratio comment/code > X%.

###Avoid Classes with a very low comment/code ratio###
Classes should have at least a ratio comment/code > X%.

###Avoid Interfaces with a very low comment/code ratio###
Interfaces should have at least a ratio comment/code > X%.

###Block Comments###
Block comments should not be used above Constructors, Methods, and Properties.  Instead use 
the `///` XML comments discussed in a later section to give C # standard descriptions.  For the 
most part, appropriately named classes, methods, properties, and variables should make the 
code self-explanatory and limit in-line comments to the occasional, single-line `//` comments.  

However, complex algorithms/business rules occasionally do require more verbose in-line 
commenting.  In these cases if you wish to use block comments you should use the following 
style:
```C#
/* Line 1
 * Line 2
 * Line 3 
 */
```
Lining up the asterisks and closing the comment block on a separate line will visually set 
the comment block off from code for the (human) reader.   Comment blocks are also useful for 
“commenting out” sections of code during development `/` debugging; however, functionality 
being deprecated should not be “commented out” and left in the source files.  Instead, rely 
on the source repository to retain historical functionality.

###Single-Line Comments###
Single-line `//` comments should be used where subtle clarification is needed.  A rule of thumb 
is that the length of a comment should not exceed the length of the code being explained; 
and, with the exception of complex algorithms/business rules, the need for long comments is 
generally an indication of poorly named or structured source code.  Single-line comments may 
also be used to “comment out” code during development `/` debugging but, again, “commented out” 
code should not be left in the source files.

When single-line comments are used as in-line, code documentation they must be indented to 
the same level as the code they are clarifying.  When using single-line comments to "comment 
out" code, place the `//` comment marks at the beginning of the line to enhance the visibility 
of commented out code.  

###XML Documentation Overview###
In the .NET framework, Microsoft has introduced a documentation generation system based on 
XML comments. These comments are formally single line C# comments containing XML tags. They 
follow this pattern for single line comments:
```C#
/// <summary>
/// This class...
/// </summary>
```
Multi-line XML comments follow this pattern:
```C#
/// <exception cref#”BogusException”>
/// This exception gets thrown as soon as a
/// Bogus flag gets set.
/// </exception>
```
All lines must be preceded by three slashes to be accepted as XML comment lines.
Tags fall into two categories:

* Documentation items
* Formatting/Referencing

The first category contains tags like `<summary>`, `<param>` or `<exception>`.  These 
represent items that are the elements of a program's API, which must be documented for the 
program to be useful to other programmers.  These tags usually have attributes such as name 
or cref as demonstrated in the multi-line example above.  The compiler checks these 
attributes, so they should be valid.

The latter category governs the layout of the documentation, using tags such as `<code>`, 
`<list>` or` <para>`.

For a more complete explanation of XML comments see the Microsoft .NET framework SDK 
documentation.

####XML Documentation Tag Usage####
For all classes, types, enums, and class members, a `<summary>` tag must be used, regardless of whether they are public, protected, or private.  This can be simply done by positioning the cursor on an empty line above the statement you wish to comment and typing ‘`///`’.  The VS .NET editor will automatically turn this into a correctly structured `<summary>` block, and will add `<param>` tags also if the statement is a method and has parameters.

The `<remarks>` tag should be used in addition to the `<summary>` tag for any code that does not allow for brief description.  This can be combined with the `<example>` tag to demonstrate usage if helpful.

Any method that includes parameters in its signature must have a `<param>` tag for each parameter that describes their purpose.  

Any method that throws an exception must have an `<exception>` tag to describe the exception that can be thrown and why it would be thrown.  The cref attribute of this tag WILL be evaluated by the compiler, so should refer to the actual Exception class that will be thrown.

Any method with a return value must have a `<remarks>` tag describing the return value.

Once XML documentation has been created, it is only truly useful if transforms are applied to make it readable.  A popular open source application called NDOC does a very good job of this.  NDOC can generate HTML or Windows based .chm help files. As of the writing of this guide NDOC is available in at the DevSystemSoftware/ndoc share.

More information on XML Documentation Tags can be found in [Appendix A] (https://github.com/CA-CST-SII/Software-Standards/blob/master/C%23_Standards.md#appendix-a---xml-documentation-tags).

####Implementing XML Documentation####
#####*Within Visual Studio*#####
In Visual Studio 2003 consider using the latest 2003 compatible GhostDoc Add-In (1.3.0 as of this writing).

In Visual Studio 2005 consider using the latest 2005 compatible GhostDoc Add-In (1.9.2 as of this writing).

There also exists Add-Ins for previewing your comments, such as QuickDocViewer. 
# Children Pages# 
# Appendix A - XML Documentation Tags

A list of recommended XML tags, with examples, includes:

##Tag: `<c>`
This tag provides a mechanism to indicate that a fragment of text within a description should be set a special font such as that used for a block of code. (For lines of actual code, use `<code>`) 

Syntax:
```C#
<c>text to be set like code</c>
```
Example:
```C#
/// <remarks>Class <c>Point</c> models a point in a two-d
/// plane.</remarks>
public class Point 
{
// …
}
```

##Tag: `<code>`
This tag is used to set one or more lines of source code or program output in some special font. (For small code fragments in narrative, use `<c>`.)

Syntax:
```C#
<code>source code or program output</code>
```
Example:
```C#
/// <summary>This method changes the point's location by
/// the given x- and y-offsets.
/// <example>For example:
/// <code>
///	Point p = new Point( 3, 5 );
///	p.Translate( -1, 3 );
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>
public void Translate( int xOr, int yOr ) 
{
X += xOr;
Y += yOr;
}	

```
Tag: `<example>`
This tag allows example code within a comment, to specify how a method or other library member may be used. Ordinarily, this would also involve use of the tag <code> as well.

Syntax:
```C#
<example>description</example>
```
Example:

See `<code>` for an example.


Tag: `<exception>`
This tag provides a way to document the exceptions a method can throw.

Syntax:
```C#
<exception cref="member">description</exception>
```
cref="member" – The name of a member. The documentation generator checks that the given member exists and translates member to the canonical element name in the documentation file. 
description – A description of the circumstances in which the exception is thrown. 

Example:
```C#
public class DataBaseOperations
{
	/// <exception cref="MasterFileFormatCorruptException"> 
	/// </exception>
	/// <exception cref="MasterFileLockedOpenException"> 
	/// </exception>
	public static void ReadRecord( int flag ) 
	{
		if( flag == 1 )
			throw new MasterFileFormatCorruptException();
		else if( flag == 2 )
			throw new MasterFileLockedOpenException();
		// …
	} 
}

```
Tag: `<list>`
This tag is used to create a list or table of items. It may contain a `<listheader>` block to define the heading row of either a table or definition list. (When defining a table, only an entry for term in the heading need be supplied.)

Each item in the list is specified with an <item> block. When creating a definition list, both term and description must be specified. However, for a table, bulleted list, or numbered list, only description need be specified.

Syntax:
```C#
<list type="bullet" | "number" | "table">
	<listheader>
		<term>term</term>
		<description>description</description>
	</listheader>
	<item>
		<term>term</term>
		<description>description</description>
	</item>
	…
	<item>
		<term>term</term>
		<description>description</description>
	</item>
</list>
```
term - The term to define.
description  - The definition of the term.

Either an item in a bullet or numbered list, or the definition of a term. 

Example:
```C#
public class MyClass
{
	/// <remarks>Here is an example of a bulleted list:
	/// <list type="bullet">
	/// <item>
	/// <description>Item 1.</description>
	/// </item>
	/// <item>
	/// <description>Item 2.</description>
	/// </item>
	/// </list>
	/// </remarks>
	public static void Main() 
	{
		// …
	}
}
```
Tag: `<para>`
This tag is for use inside other tags, such as `<remarks>` or `<returns>`, and permits structure to be added to text.

Syntax:

`<para>content</para>`

content - The text of the paragraph. 

Example:
```C#
/// <summary>This is the entry point of the Point class 
/// testing program.
/// <para>This program tests each method and operator, 
/// and is intended to be run after any non-trivial 
/// maintenance has been performed on the Point 
/// class.
/// </para>
/// </summary>
public static void Main() 
{
	// …
}
```
Tag: `<param>`
This tag is used to describe a parameter for a method, constructor, or indexer.

Syntax:

`<param name="name">description</param>`

name - The name of the parameter.
description - A description of the parameter. 

Example:
```C#
/// <summary>This method changes the point's location to
/// the given coordinates.
/// </summary>
/// <param><c>xOr</c> is the new x-coordinate.</param>
/// <param><c>yOr</c> is the new y-coordinate.</param>
public void Move( int xOr, int yOr ) 
{
	X = xOr;
	Y = yOr;
}
```

Tag: `<paramref>`
This tag is used to indicate that a word is a parameter. The documentation file can be processed to format this parameter in some distinct way.

Syntax:

`<paramref name="name"/>`

name - The name of the parameter.

Example:
```C#
/// <summary>This constructor initializes the new Point to
///	(<paramref name="xOr"/>, <paramref name="yOr"/>).
/// </summary>
/// <param><c>xOr</c> is the new Point's x-coordinate.</param>
/// <param><c>yOr</c> is the new Point's y-coordinate.</param>
public Point( int xOr, int yOr ) 
{
	X = xOr;
	Y = yOr;
}
```

Tag: `<permission>`
This tag allows the security accessibility of a member to be documented. 

Syntax:

`<permission cref="member">description</permission>`
 
cref="member" - The name of a member. The documentation generator checks that the given code element exists and translates member to the canonical element name in the documentation file.
description - A description of the access to the member. 


Example:
```C#
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>
public static void Test() 
{
	// …
}
```

Tag: `<remarks>`
This tag is used to specify overview information about a type. (Use `<summary>` to describe the members of a type.)

Syntax:

`<remarks>description</remarks>`

description - The text of the remarks. 

Example:
```C#
/// <remarks>Class <c>Point</c> models a point in a two-dimensional 
/// plane.</remarks>
public class Point 
{
	// …
}

```
Tag: `<returns>`
This tag is used to describe the return value of a method.

Syntax:

`<returns>description</returns>`

description -  A description of the return value. 

Example:
```C#
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form 
/// (x,y), without any leading, training, or embedded 
/// whitespace.
/// </returns>
public override string ToString() 
{
	return "(" + X + "," + Y + ")";
}

```
Tag: `<see>`
This tag allows a link to be specified within text. (Use `<seealso>` to indicate text that is to appear in a See Also section.)

Syntax:

`<see cref="member"/>`

cref="member" - The name of a member. The documentation generator checks that the given code element exists and passes member to the element name in the documentation file.

Example:
```C#
/// <summary>This method changes the point's location to
/// the given coordinates.
/// </summary>
/// <see cref="Translate"/>
public void Move( int xOr, int yOr ) 
{
	X = xOr;
	Y = yOr;
}
/// <summary>This method changes the point's location by
/// the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate( int xOr, int yOr ) 
{
	X += xOr;
	Y += yOr;
}
```

Tag: `<seealso>`
This tag allows an entry to be generated for the See Also section. (Use `<see>` to specify a link from within text.)

Syntax:

`<seealso cref="member"/>`

cref="member" - The name of a member. The documentation generator checks that the given code element exists and passes member to the element name in the documentation file.

Example:
```C#
/// <summary>This method determines whether two Points have the 
/// same location.
/// </summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals( object o ) 
{
	// …
}
```

Tag: `<summary>`
This tag can be used to describe a member for a type. (Use `<remarks>` to describe the type itself.)

Syntax:

`<summary>description</summary>`

description - A summary of the member. 

Example:
```C#
/// <summary>This constructor initializes the new Point to 
/// ( 0, 0 ).
/// </summary>
public Point() : this( 0, 0 ) 
{
}
```

Tag: `<value>`
This tag allows a property to be described.

Syntax:

`<value>property description</value>`

property description - A description for the property. 

Example:
```C#
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
	get { return x; }
	set { x = value; }
}
```

Tag: `<include>`
This tag is used to reference external files. The file attribute is the name of the file using relative or fully qualified paths. The include file itself in an XML document that holds XML Comments. The path attribute is an XPath statement that points to the parent element of the XML comments in the external document.

Example:
```C#
/// <include file=’MyXMLCommentFile.xml’
/// path=’doc/members/member[@name=”T:MyExampleClass”]/*’/>
public class MyExampleClass
{

	/// <include file=’MyXMLCommentFile.xml’
	/// path=’doc/members/member[@name=”M:MyExampleMethod”]/*’/>
	public string MyExampleMethod( string returnThis )
	{
		return returnThis;
	}

}
```
