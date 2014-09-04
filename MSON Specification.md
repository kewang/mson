# MSON Specification
Markdown Syntax for Object Notation (MSON) is a plain text syntax for the description and validation of data structures.

In MSON, data structures are described as compound types. Elements of these compound types can be themselves instances of other types.

## How to Read the Grammar
- An arrow (→) is used to mark grammar productions and can be read as "can consist of".
- Syntactic categories are indicated by _italic_ text
- Literal words and punctuations are indicated by a `code span`
- Alternative grammar productions are separated by vertical bars (|)
- Optional syntactic categories and literals are marked by _[italic text in square brackets]_
- Grammar production can span several lines and ends at another grammar production and/or the end of a paragraph.

## Markdown Syntax
Note this reference is using ATX-style headers (#) and hyphens-style lists (-) exclusively. However you MAY use Setext-style headers and/or asterisk (*) or pluses (+) style list if you prefer.

## Types

- Named Types

    - Built-in

        MSON provides following pre-defined named types:

        - Primitive Types

            - `Boolean` or `Boolean`
            - `String`
            - `Number`

        - Compound Types

            - `Array`
            - `Enumeration` or `Enum`
            - `Object`
    
    - User-defined 

        See _[Named Type Definition]()_.

- Unnamed Types

    To be decided.

    - Compound Types
        
        - Tuple

## Element
Element of a compound type.

_Element_ → _[Array Element]()_ | _[Object Element]()_ | _[Enumeration Element]()_

_Array Element_ → _[Item Element]()_ | _[Element Mixin]()_

_Object Element_ → _[Property Element]()_ | _[Element Mixin]()_

_Enumeration Element_ → _[Member Element]()_ | _[Element Mixin]()_

### Item Element
Array Item Element – one item in an array.

_Item Element_ → _[Type Instance]()_

### Property Element
Object Property Element - one property (attribute) of an object. _Property Name_ is a fully-fledged _Type Instance_.

_Property Element_ → _[Property Name]()_ `:` _[Type Instance]()_ _[opt]_
_Property Name_ → [Literal Value]() | _[Type Instance]()_

### Member Element
Enumeration Member Element - one member of an enumeration. Enumeration _Member Value_ is a fully-fledged _Type Instance_.

_Member Element_ → _[Member Value]()_ `:` _[Type Instance]()_ _[opt]_
_Member Value_ → [Literal Value]() | _[Type Instance]()_

### Element Mixin
Mixin Element is a special type of _Element_ that adds _Elements_ from another compound type to the parent's _Elements_. The source and destination MUST be of the same compound type.

_Element Mixin_ → `- Include` _[Type Name]()_

### Type Instance
Define an instance of a type.

A _Type Instance_ definition MUST include _Value_ or _Type Annotation_. _Type Instances_ of the `Object` or `Enumeration` base type MUST NOT specify a value.  A _Value_ of an instance of unspecified type MAY imply the type of the instance.

_Type Instance_ → _[Value]()_ _[opt]_ _[Type Annotation]()_ _[opt]_ `-` _[Description]()_ _[opt]_

_[Additional Description]()_ _[opt]_

_[Instance Elements]()_ _[opt]_

_[Validations]()_ _[opt]_ _– to be decided_

### Value
Based on the _Type Instance_ the _Value_ syntax is:

_Value_ → _[Array Values List]()_ | _[Primitive Value]()_

_Array Values List_ → _[Value]()_ | _[Value]()_`,` _[Array Values List]()_

_Primitive Value_ → [Literal Value]()

### Type Annotation
Type annotation explicitly specifies the type of an MSON instance.

_Type Annotation_ → `(`_[Type Attributes List]()_ _[opt]_`,` _[Type Specifications List]()_`)`

_Type Attributes List_ → _[Type Attribute]()_ | _[Type Attribute]()_`,` _[Type Attributes List]()_

_Type Specifications List_ →  _[Type Specification]()_ | _[Type Specification]()_`,` _[Type Specifications List]()_

_Type Specification_ → _[Type Name]()_`:` _[Type Specifications List]()_ _[opt]_ 

### Type Attribute

- `required` - instance of this type is required
- `optional` - syntactic sugar for optional instance of this type

### Type Name
Name of a type including built-in named types. Some limitations apply (see [Reserved Characters & Keywords]()).

### Literal Value
Literal value of a type instance. Some limitations apply (see [Reserved Characters & Keywords]()).

### Description
_Description_ → _[Markdown-formatted text]_

### Additional Description
_Additional Description_ → _[Markdown-formatted text]_

### Instance Elements
Define elements of a compound type instance. If preceded by an _Additional Description_ the _Instance Elements_ MUST start with _Instance Elements Delimiter_. The _Instance Elements Delimiter_ MUST be nested one additional list level under the parent's _Named Type Definition_.

_Instance Elements_ → _[Instance Elements Delimiter]()_ _[opt]_ 

_[Elements List]()_

_Instance Elements Delimiter_ → `-` _[Elements Delimiter]()_

### Elements Delimiter
Based on the base type the _Elements Delimiter_ is defined as follows:

- `Array` base type:  

    _Elements Delimiter_ → `Elements`

- `Enumeration` base type: 

    _Elements Delimiter_ → `Members`

- `Object` base type: 
    
    _Elements Delimiter_ → `Properties`

### Elements List
List of elements of a compound type. Based on the base type the _Elements List_ is defined as follows:

- `Array` base type:

    _Elements List_ → 

    `-` _[Array Element]()_

    _[Elements List]()_ _[opt]_

- `Enumeration` base type:

    _Elements List_ → 

    `-` _[Member Element]()_

    _[Elements List]()_ _[opt]_

- `Object` base type:

    _Elements List_ → 

    `-` _[Properties Element]()_

    _[Elements List]()_ _[opt]_    

## Named Type Definition
Define a new named type based on an existing type.

_Named Type Definition_ → `#` _[New Type Name]()_ `(`_[Type Annotation]()_`)`

_[Description]()_ _[opt]_

_[Named Type Elements]()_ _[opt]_

_[Validations]()_ _[opt]_ _– to be decided_

_New Type Name_ → _[Type Name]()_

### Named Type Elements
Define elements of a compound type instance. If preceded by an _Description_ the _Named Type Elements_ MUST start with _Named Type Elements Delimiter_. The _Named Type Elements Delimiter_ SHOULD be nested one additional header level under the parent's _Type Instance_. _Elements_ MUST be nested one additional level under the _Named Type Elements Delimiter_ if present otherwise one additional level under the parent's _Type Instance_.

_Instance Elements_ → _[Named Type Elements Delimiter]()_ _[opt]_ 

_[Elements List]()_

_Named Type Elements Delimiter_ → `##` _[Elements Delimiter]()_

## Reserved Characters & Keywords
When using following characters or keywords in an _Instance Name_, Literal Value or _Type Name_ the name or literal MUST be enclosed in backticks `` ` ``.

### Characters

`:`, `(`,`)`, `<`, `>`, `{`, `}`, `[`, `]`, `_`, `*`, `-`, `+`, `` ` `` 

### Keywords 

`Element`, `Elements`, `Property`, `Properties`, `Member`, `Members`, `Include`

Note keywords are case insensitive.

### Additional Keywords
Following keywords are reserved for future use:

`Trait`, `Traits`, `Parameter`, `Parameters`, `Attribute`, `Attributes`, `Filter`, `Validation`, `Choice`, `Choices` `One of`, `Enumeration`, `Enum`, `Object`, `Array`
