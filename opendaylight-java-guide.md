# OpenDaylight Java Style Guide

# Introduction

This document serves as the definition of the OpenDaylight's projects coding standards for source code written in the Java™ programming language. All code submitted to the OpenDaylight project MUST adhere to these guidelines. These guidelines are enforced, where applicable, by the Maven Checkstyle plugin.

The guidelines laid forth in this document pertain primarily to the aesthetics of a Java source file but will expand in future to encompass additional conventions, patterns and coding standards.

## Terminology

## Guide Notes

Code used in examples is *non-normative*. The examples are therefore for illustrative purposes only and are not designed to be compiled or used in any way. Any optional formatting choices used in examples should not be interpretted as standards.

## Copyright Notice

The style and content in this document is based on the [Google Java Style](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html) and is licensed under the [Creative Commons 3.0 Attribution unported license](http://creativecommons.org/licenses/by/3.0/)

This document carries the same license

# Source File Basics

## File Name

The source file name consists of a case-sensitive name of the top-level class it contains plus the `.java` extension

## File Encoding

Source files are encoded in **UTF-8**

## Special Characters

### Whitespace Characters

Aside from the line terminator sequence `\n` the **ASCII horizontal space character (0x20)** is the only whitespace character permitted in a source file. This implies that

    - All other whitespace characters in string or character literals are escaped
    - Tab characters are **NOT** used for indentation
    - Files should use UNIX line endings `\n`
        - Use of CRLF `\r\n` is **NOT** permitted

### Special Escape Sequences

If a characted has a special escape sequence (`\b`, `\t`, `\n`, `\*`, `\\`)
 then this is to be used rather than the corresponding octal (e.g. `\012`) or Unicode (e.g `\u000a`) escapes.

 ### Non-ASCII Characters

For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent Unicode escape (e.g. `\u221e`) is used, depending only on which makes the code easier to read and understand.

> Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

| Example                                                  | Discussion                                    |
|----------------------------------------------------------|-----------------------------------------------|
| `String unitAbbrev = "μs";`                              | Best: perfectly clear even without a comment. |
| `String unitAbbrev = "\u03bcs";  // "μs"`                | Allowed, but there's no reason to do this.    |
| `String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"` | Allowed, but awkward and prone to mistakes.   |
| `String unitAbbrev = "\u03bcs";`                         | Poor: the reader has no idea what this is.    |
| `return '\ufeff' + content; // byte order mark`          | Good: use escapes for non-printable characters, and comment if necessary. |

> Tip: Never make your code less readable simply out of fear that some programs might not handle non-ASCII characters properly. If that should happen, those programs are broken and they must be fixed.

# Source File Structure

A source file consists of, **in-order**:

1. Copyright Header
2. Package Statement
3. Import Statements
4. Exactly One Top-Level Class

**Exactly one blank line seperates each section**

## Copyright Header

The copyright header appears at the top of the file. This snippet should be used:

    /**
    * Copyright (c) <Date> <Company or Individual>.  All rights reserved.
    *
    * This program and the accompanying materials are made available under the
    * terms of the Eclipse Public License v1.0 which accompanies this distribution,
    * and is available at http://www.eclipse.org/legal/epl-v10.html
    */

> Fun Fact: The Copyright statement in this file is used to grant the freedoms of the open source license. See [Copyleft](http://en.wikipedia.org/wiki/Copyleft) for further reading.

### Modifying Copyright headers

When a significant code contribution is made to a file the copyright may be altered to either:

    /**
    * Copyright (c) <Date> <Company or Individual> and others.  All rights reserved.
    <SNIP>
    */

or:

    /**
    * Copyright (c) <Date> <Company or Individual>.  All rights reserved.
    * Copyright (c) <Date> <Company or Individual>.  All rights reserved.
    <SNIP>
    */

A significant contribution is defined as the addition of more than 50% of the current line count within a source file.

> **NEVER** remove or replace the copyright header

## Package statement

This identifies the package to which a class belongs. It is not line wrapped and therefore is exempt from the column limit.

## Import Statements

### No Wildcard Imports

**Wildcard imports**, static or otherwise, are NOT used.

### No Line Wrapping

Import statements are never wrapped to the column limit

### Ordering And Spacing

Imports are divided in to the following groups and seperated by exactly one blank line.

1. Static Imports
2. `org.opendaylight` imports
3. Third party imports, on group per top-level package in ASCII sort order
    - `com`, `junit`, `org`, `sun`
4. `java` imports
5. `javax` imports

Within a group there are no blank lines, and the imported names appear in ASCII sort order. (Note: this is not the same as the import statements being in ASCII sort order; the presence of semicolons warps the result.)

## Class Declaration

### Exactly One Top-Level Class Declaration

The amount of top-level class is one. Neither two, nor three, but *one* is the number of top-level classes.

### Class Member Ordering

There is no one right way to order members within a class, but please try to do so logically. A good litmus test is to make sure you are able to explain the ordering to a colleague who is working on the same class. Having some order other than 'logical' is NOT permitted and no 'chronological' doesn't count even though it contains the word 'logical'

#### Don't split overloads

When a class has multiple constructors or when multiple methods exists with the same name then these should appear sequentially.

## Formatting

**Terminology Note:** block-like construct refers to the body of a class, method or constructor. Note that, by Section 4.8.3.1 on array initializers, any array initializer may optionally be treated as if it were a block-like construct.

### Braces

Braces are used with `if`, `else`, `for`, `do` and `while` statements, even when the body is empty or contains only a single statement.

### Non-Empty Blocks: K&R Style

Braces follow the Kernighan and Ritchie style (["Egyptian brackets"](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)) for nonempty blocks and block-like constructs:

- No line break before the opening brace.
- Line break after the opening brace.
- Line break before the closing brace.
- Line break after the closing brace if that brace terminates a statement or the body of a method, constructor or named class. For example, there is no line break after the brace if it is followed by `else` or a comma.

Example:

    return new MyClass() {
    @Override public void method() {
        if (condition()) {
          try {
            something();
          } catch (ProblemException e) {
            recover();
          }
        }
      }
    };

### Empty Blocks: Concise

An empty block or block-like construct may be closed immediately after it is opened, with no characters or line break in between (`{}`), unless it is part of a multi-block statement (one that directly contains multiple blocks: `if/else-if/else` or `try/catch/finally`).

Example:

      void doNothing() {}

### Block Indentation: 2+ Spaces

Each time a new block or block-like consttuct is openend, the indent increases by two spaces. When the block ends, the indent returns to the previous indent level. This applies to both code and comments within the block

### One Statement Per Line

Each statement is followed by a line break

### Column Limit: 80 Characters

Any line that exceeds the 80 Character limit must be wrapped with the exception of:

- `package` and `import` statements

### Line Wrapping

**Terminology Note:** When code that might otherwise legally occupy a single line is divided into multiple lines, typically to avoid overflowing the column limit, this activity is called line-wrapping.

#### Where To Break

The prime directive of line-wrapping is: prefer to break at a higher syntactic level. Also:

1. When a line is broken at a non-assignment operator the break comes before the symbol.
    - This also applies to the following "operator-like" symbols: the dot separator (`.`), the ampersand in type bounds (`<T extends Foo & Bar>`), and the pipe in catch blocks (`catch (FooException | BarException e)`).
2. When a line is broken at an assignment operator the break typically comes after the symbol, but either way is acceptable.
    - This also applies to the "assignment-operator-like" colon in an enhanced `for` ("foreach") statement.
3. A method or constructor name stays attached to the open parenthesis (`(`) that follows it.
4. A comma (`,`) stays attached to the token that precedes it.

#### Indent Continuation: 4+ Spaces

When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line.

When there are multiple continuation lines, indentation may be varied beyond +4 as desired. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.

### Whitespace

#### Vertical Whitespace

A single blank line appears:

- Between consecutive members (or initializers) of a class: fields, constructors, methods, nested classes, static initializers, instance initializers.
    - Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
- Within method bodies, as needed to create logical groupings of statements.
- Optionally before the first member or after the last member of the class (neither encouraged nor discouraged).
- As required by other sections of this document (such as Import statements)

Multiple consecutive blank lines are permitted, but never required (or encouraged).

#### Horizontal Whitespace

Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places only.

1. Separating any reserved word, such as `if`, `for` or `catch`, from an open parenthesis (`(`) that follows it on that line
2. Separating any reserved word, such as `else` or `catch`, from a closing curly brace (`}`) that precedes it on that line
3. Before any open curly brace (`{`), with two exceptions:
    - `@SomeAnnotation({a, b})` (no space is used)
    - `String[][] x = {{"foo"}};` (no space is required between `{{`, by item 8 below)
4. On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:
    - the ampersand in a conjunctive type bound: `<T extends Foo & Bar>`
    - the pipe for a catch block that handles multiple exceptions: `catch (FooException | BarException e)`
    - the colon (`:`) in an enhanced `for` ("foreach") statement
5. After `,:;` or the closing parenthesis (`)`) of a cast
6. On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.
7. Between the type and variable of a declaration: `List<String> list`
8. Optional just inside both braces of an array initializer
        `new int[] {5, 6}` and `new int[] { 5, 6 }` are both valid



<!-- unedited !-->

#### Horizontal Alignment: Never Required

**Terminology Note:** Horizontal alignment is the practice of adding a variable number of additional spaces in your code with the goal of making certain tokens appear directly below certain other tokens on previous lines.

This practice is permitted, but is never required. It is not even required to maintain horizontal alignment in places where it was already used.

Here is an example without alignment, then using alignment:


    private int x; // this is fine
    private Color color; // this too

    private int   x;      // permitted, but future edits
    private Color color;  // may leave it unaligned


>Tip: Alignment can aid readability, but it creates problems for future maintenance. Consider a future change that needs to touch just one line. This change may leave the formerly-pleasing formatting mangled, and that is allowed. More often it prompts the coder (perhaps you) to adjust whitespace on nearby lines as well, possibly triggering a cascading series of reformattings. That one-line change now has a "blast radius." This can at worst result in pointless busywork, but at best it still corrupts version history information, slows down reviewers and exacerbates merge conflicts.

### Grouping Parentheses: Recommended

Optional grouping parentheses are omitted only when author and reviewer agree that there is no reasonable chance the code will be misinterpreted without them, nor would they have made the code easier to read. It is not reasonable to assume that every reader has the entire Java operator precedence table memorized.

### Specific Constructs

#### Enum Classes

After each comma that follows an enum constant, a line-break is optional.

An enum class with no methods and no documentation on its constants may optionally be formatted as if it were an array initializer.

    private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }

Since enum classes are classes, all other rules for formatting classes apply.

#### Variable Declarations

##### One Variable Per Declaration

Every variable declaration (field or local) declares only one variable: declarations such as `int a, b;` are not used.

##### Declared When Needed, Initialized As Soon As Possible

Local variables are not habitually declared at the start of their containing block or block-like construct. Instead, local variables are declared close to the point they are first used (within reason), to minimize their scope. Local variable declarations typically have initializers, or are initialized immediately after declaration.

#### Arrays

##### Array Initializers: Can Be "block-like"

Any array initializer may optionally be formatted as if it were a "block-like construct." For example, the following are all valid (not an exhaustive list):


    new int[] {           new int[] {
      0, 1, 2, 3            0,
    }                       1,
                            2,
    new int[] {             3,
      0, 1,               }
      2, 3
    }                     new int[]
                              {0, 1, 2, 3}


##### No C-style Array Declarations

The square brackets form a part of the type, not the variable: `String[] args`, not `String args[]`.

#### Switch Statements

**Terminology Note:** Inside the braces of a switch block are one or more statement groups. Each statement group consists of one or more switch labels (either `case FOO:` or `default:`), followed by one or more statements.

##### Indentation
As with any other block, the contents of a switch block are indented +2.

After a switch label, a newline appears, and the indentation level is increased +2, exactly as if a block were being opened. The following switch label returns to the previous indentation level, as if a block had been closed.

##### Fall-Through: Commented

Within a switch block, each statement group either terminates abruptly (with a `break`, `continue`, `return` or thrown exception), or is marked with a comment to indicate that execution will or might continue into the next statement group. Any comment that communicates the idea of fall-through is sufficient (typically `// fall through`). This special comment is not required in the last statement group of the switch block. Example:

    switch (input) {
      case 1:
      case 2:
        prepareOneOrTwo();
        // fall through
      case 3:
        handleOneTwoOrThree();
        break;
      default:
        handleLargeNumber(input);
    }


##### The Default Case Is Present

Each switch statement includes a `default` statement group, even if it contains no code.

#### Annotations

Annotations applying to a class, method or constructor appear immediately after the documentation block, and each annotation is listed on a line of its own (that is, one annotation per line). These line breaks do not constitute line-wrapping, so the indentation level is not increased. Example:


    @Override
    @Nullable
    public String getNameIfPresent() { ... }


Exception: A single parameterless annotation may instead appear together with the first line of the signature, for example:


    @Override public int hashCode() { ... }


Annotations applying to a field also appear immediately after the documentation block, but in this case, multiple annotations (possibly parameterized) may be listed on the same line; for example:


    @Partial @Mock DataLoader loader;


There are no specific rules for formatting parameter and local variable annotations.

#### Comments

##### Block Comment Style

Block comments are indented at the same level as the surrounding code. They may be in `/* ... */` style or `// ...` style. For multi-line `/* ... */` comments, subsequent lines must start with `*` aligned with the `*` on the previous line.


    /*
     * This is          // And so           /* Or you can
     * okay.            // is this.          * even do this. */
     */


Comments are not enclosed in boxes drawn with asterisks or other characters.

Tip: When writing multi-line comments, use the `/* ... */` style if you want automatic code formatters to re-wrap the lines when necessary (paragraph-style). Most formatters don't re-wrap lines in `// ...` style comment blocks.

##### Commenting Guidelines

Commenting source code is intended to enable future readers/editors for the code to quickly understand and come up to speed on the logic in order to facilitate a community where anyone can read and modify any code. Comments in code are intended to help readers of the code to quickly gain an understanding of the purpose of a file, class, method, etc. There is no way to avoid having to read code, but with a few well placed comments in classes you can quickly speed up developers understanding of the code.

Ensure the following code is commented:

- Line level comments - any code which is complex or doing something out of the ordinary.
- File level comments - any file which has multiple purposes, or whose name doesn’t clearly state its sole purpose.

Examples:

This is a bad comment:

    // foo is the sum of 1 + 1
    int foo = 1 + 1;

This comment is uncessary as the purpose of the code is pretty self-explanatory. Also this comment is far too specific. Should `foo` be updated in a later commit we could end up with the following:

    // foo is the sum of 1 + 1
    int foo = 5;

This only aids in making the code more difficult to read.

This is a good comment:

<TBD>

#### Modifiers

Class and member modifiers, when present, appear in the order recommended by the Java Language Specification:


    public protected private abstract static final transient volatile synchronized native strictfp

#### Numeric Literals

`long`-valued integer literals use an uppercase `L` suffix, never lowercase (to avoid confusion with the digit `1`). For example, `3000000000L` rather than `3000000000l`.

## Naming

### Rules Common To All Identifiers

Identifiers use only ASCII letters and digits, and in two cases noted below, underscores. Thus each valid identifier name is matched by the regular expression `\w+` .

Special prefixes or suffixes, like those seen in the examples `name_`, `mName`, `s_name` and `kName`, should not be used.

### Rules By Identifier Type

#### Package Names

Package names are all lowercase, with consecutive words simply concatenated together (no underscores). For example, `com.example.deepspace`, not `com.example.deepSpace` or `com.example.deep_space`.

#### Class Names

Class names are written in UpperCamelCase

Class names are typically nouns or noun phrases. For example, `Character` or `ImmutableList`. Interface names may also be nouns or noun phrases (for example, `List`), but may sometimes be adjectives or adjective phrases instead (for example, `Readable`).

There are no specific rules or even well-established conventions for naming annotation types.

Test classes are named starting with the name of the class they are testing, and ending with `Test`. For example, `HashTest` or `HashIntegrationTest`. `IntegrationTest` may be shortened to `IT`.

#### Method Names

Method names are written in lowerCamelCase

Method names are typically verbs or verb phrases. For example, `sendMessage` or `stop`.

Underscores may appear in JUnit test method names to separate logical components of the name. One typical pattern is `test_`, for example `testPop_emptyStack`. There is no One Correct Way to name test methods.

#### Constant Names

Constant names use `CONSTANT_CASE`: all uppercase letters, with words separated by underscores. But what is a constant, exactly?

Every constant is a static final field, but not all static final fields are constants. Before choosing constant case, consider whether the field really feels like a constant. For example, if any of that instance's observable state can change, it is almost certainly not a constant. Merely intending to never mutate the object is generally not enough. Examples:


    // Constants
    static final int NUMBER = 5;
    static final ImmutableList NAMES = ImmutableList.of("Ed", "Ann");
    static final Joiner COMMA_JOINER = Joiner.on(',');  // because Joiner is immutable
    static final SomeMutableType[] EMPTY_ARRAY = {};
    enum SomeEnum { ENUM_CONSTANT }

    // Not constants
    static String nonFinal = "non-final";
    final String nonStatic = "non-static";
    static final Set mutableCollection = new HashSet();
    static final ImmutableSet mutableElements = ImmutableSet.of(mutable);
    static final Logger logger = Logger.getLogger(MyClass.getName());
    static final String[] nonEmptyArray = {"these", "can", "change"};


These names are typically nouns or noun phrases.

#### Non-Constant Field Names

Non-constant field names (static or otherwise) are written in lowerCamelCase

These names are typically nouns or noun phrases. For example, `computedValues` or `index`.

#### Parameter Names

Parameter names are written in lowerCamelCase.

One-character parameter names should be avoided.

#### Local Variable Names

Local variable names are written in lowerCamelCase. Local variable names are more tolerant to abbreviations but not if it effects readability.

One-character names should be avoided, except for temporary and looping variables.

Even when final and immutable, local variables are not considered to be constants, and should not be styled as constants.

#### Type Variable Names

Each type variable is named in one of two styles:

- A single capital letter, optionally followed by a single numeral (such as `E`, `T`, `X`, `T2`)
- A name in the form used for classes (see Class names), followed by the capital letter `T` (examples: `RequestT`, `FooBarT`).

### Camel Case: Defined

Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like "IPv6" or "iOS" are present. To improve predictability, the OpenDaylight Code Style specifies the following (nearly) deterministic scheme.

Beginning with the prose form of the name:

1. Convert the phrase to plain ASCII and remove any apostrophes. For example, "Müller's algorithm" might become "Muellers algorithm".
2. Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
    * Recommended: if any word already has a conventional camel-case appearance in common usage, split this into its constituent parts (e.g., "AdWords" becomes "ad words"). Note that a word such as "iOS" is not really in camel case per se; it defies any convention, so this recommendation does not apply.
3. Now lowercase everything (including acronyms), then uppercase only the first character of:
    * ... each word, to yield upper camel case, or
    * ... each word except the first, to yield lower camel case
4. Finally, join all the words into a single identifier.

Note that the casing of the original words is almost entirely disregarded. Examples:

| Prose                   | formCorrect         | Incorrect           |
|-------------------------|---------------------|---------------------|
| "XML HTTP request"      | `XmlHttpRequest`    | `XMLHTTPRequest`    |
| "new customer ID"       | `newCustomerId`     | `newCustomerID`     |
| "inner stopwatch"       | `innerStopwatch`    | `innerStopWatch`    |
| "supports IPv6 on iOS?" | `supportsIpv6OnIos` | `supportsIPv6OnIOS` |
| "YouTube importer"      | `YouTubeImporter`   |                     |


> Note: Some words are ambiguously hyphenated in the English language: for example "nonempty" and "non-empty" are both correct, so the method names `checkNonempty` and `checkNonEmpty` are likewise both correct.

## Programming Practices

### @Override: Always Used

A method is marked with the `@Override` annotation whenever it is legal. This includes a class method overriding a superclass method, a class method implementing an interface method, and an interface method respecifying a superinterface method.

> Exception:`@Override` may be omitted when the parent method is `@Deprecated`.

### Caught Exceptions: Not Ignored

Except as noted below, it is very rarely correct to do nothing in response to a caught exception. (Typical responses are to log it, or if it is considered "impossible", rethrow it as an `AssertionError`.)

When it truly is appropriate to take no action whatsoever in a catch block, the reason this is justified is explained in a comment.


    try {
      int i = Integer.parseInt(response);
      return handleNumericResponse(i);
    } catch (NumberFormatException ok) {
      // it's not numeric; that's fine, just continue
    }
    return handleTextResponse(response);


Exception: In tests, a caught exception may be ignored without comment if it is named `expected`. The following is a very common idiom for ensuring that the method under test does throw an exception of the expected type, so a comment is unnecessary here.


    try {
      emptyStack.pop();
      fail();
    } catch (NoSuchElementException expected) {
    }


### Static Members: Qualified Using Class

When a reference to a static class member must be qualified, it is qualified with that class's name, not with a reference or expression of that class's type.


    Foo aFoo = ...;
    Foo.aStaticMethod(); // good
    aFoo.aStaticMethod(); // bad
    somethingThatYieldsAFoo().aStaticMethod(); // very bad


### Finalizers

It is extremely rare to override `Object.finalize`.

> Tip: Don't do it. If you absolutely must, first read and understand [Effective Java](http://books.google.com/books?isbn=8131726592) Item 7, "Avoid Finalizers," very carefully, and then don't do it.

## Javadoc

### Formatting

#### General Form

The basic formatting of Javadoc blocks is as seen in this example:


    /**
     * Multiple lines of Javadoc text are written here,
     * wrapped normally...
     */
    public int method(String p1) { ... }


... or in this single-line example:


    /** An especially short bit of Javadoc. */


The basic form is always acceptable. The single-line form may be substituted when there are no at-clauses present, and the entirety of the Javadoc block (including comment markers) can fit on a single line.

#### Paragraphs

One blank line—that is, a line containing only the aligned leading asterisk (`*`)—appears between paragraphs, and before the group of "at-clauses" if present. Each paragraph but the first has `` immediately before the first word, with no space after.

#### At-clauses

Any of the standard "at-clauses" that are used appear in the order `@param`, `@return`, `@throws`, `@deprecated`, and these four types never appear with an empty description. When an at-clause doesn't fit on a single line, continuation lines are indented four (or more) spaces from the position of the `@`.

### The Summary Fragment

The Javadoc for each class and member begins with a brief summary fragment. This fragment is very important: it is the only part of the text that appears in certain contexts such as class and method indexes.

This is a fragment—a noun phrase or verb phrase, not a complete sentence. It does not begin with `A {@code Foo} is a...`, or `This method returns...`, nor does it form a complete imperative sentence like `Save the record.`. However, the fragment is capitalized and punctuated as if it were a complete sentence.

Tip: A common mistake is to write simple Javadoc in the form `/** @return the customer ID */`. This is incorrect, and should be changed to `/** Returns the customer ID. */`.

### Where Javadoc Is Used

At the minimum, Javadoc is present for every `public` class, and every `public` or `protected` member of such a class, with a few exceptions noted below.

Other classes and members still have Javadoc as needed. Whenever an implementation comment would be used to define the overall purpose or behavior of a class, method or field, that comment is written as Javadoc instead. (It's more uniform, and more tool-friendly.)

#### Exception: Self-Explanatory Methods

Javadoc is optional for "simple, obvious" methods like `getFoo`, in cases where there really and truly is nothing else worthwhile to say but "Returns the foo".

Important: it is not appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a method named `getCanonicalName`, don't omit its documentation (with the rationale that it would say only `/** Returns the canonical name. */`) if a typical reader may have no idea what the term "canonical name" means!

#### Exception: Overrides

Javadoc is not always present on a method that overrides a supertype method.

