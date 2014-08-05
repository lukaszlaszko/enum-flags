# Flags for C++ 11 enum classes

C++ 11 gives a new type of enums — *enum classes*.
These are strongly typed enumerations, they do not cast implicitly to or from
the underlying integer type. They also do not leak enumeration values to
enclosing scope (thus the names of this values do not clash and you no longer
need to prefix/suffix them with something to disambiguate).

The only drawback to type safety is you cannot treat variables of *enum class*
types as sets of flags. That is because *enum classes* do not cast to integers
and there are no bitwise operators overloads defined for them.

This project brings a `flags` class template which:
* does not convert to or from integer types;
* does implicitly convert from enum it was instantiated by;
* does not convert to that enum;
* supports all bitwise operations except for shifts (since shifts are not very
  reasonable for flags);
* explicitly converts to bool (which allows to use it in boolean contexts, like
  in branching or loop conditions);
* provides access to the underlying integer through member functions.

## Description
``` c++
template <class E> class flags;
```
where `E` is an enum.

### Member types
Member type    |Definition                                                |Notes
---------------|----------------------------------------------------------|---------------------------------------------
enum_type      |The first template parameter (E)                          |
underlying_type|The integer type that is used as representation of `flags`|Equivalent of `std::underlying_type<E>::type`

### Member functions

#### Constructors and assignment operators
`flags()`                    |Default contructor (all flags are unset)
`flags(flags::enum_type)`    |Sets flag denoted by the parameter
`flags(const flags&)`        |Copy constructor
`flags(flags&&)`             |Move constructor
`operator=(flags::enum_type)`|Unsets all flags, then sets flag denoted by the parameter
`operator=(const flags&)`    |Copy assignment
`operator=(flags&&)`         |Move assignment

#### Bitwise operators
Note, all functions in this group return reference to `flags`, except for
`operator~` which returns a new `flags` value.

`operator|=(flags::enum_type)`|Bitwise OR with flag denoted by the parameter
`operator|=(const flags&)`    |Bitwise OR with flags in the parameter
`operator&=(flags::enum_type)`|Bitwise AND with flag denoted by the parameter`
`operator&=(const flags&)`    |Bitwise AND with flags in the parameter
`operator^=(flags::enum_type)`|Bitwise XOR with flag denoted by the parameter
`operator^=(const flags&)`    |Bitwise XOR with flags in the parameter
`operator~()`                 |Bitwise negation

#### Boolean conversions
`operator bool()`|Returns `true` if any of the flags is set
`operator!()`    |Returns `true` if none of the flags is set

#### Raw access to integer representation
`underlying_value()`                          |Returns integer representation
`set_underlying_value(flags::underlying_type)`|Sets integer representation to new value

#### Various functions
`swap(flags)`|Swaps to `flags` values

### Nonmember functions
Note, all functions in this group return new `flags` value.

`operator|(flags, flags)`           |Bitwise OR of two `flags` values
`operator|(flags, flags::enum_type)`|Bitwise OR of a `flags` value and a flag denoted by an enum value
`operator|(flags::enum_type, flags)`|Bitwise OR of a `flags` value and a flag denoted by an enum value
`operator&(flags, flags)`           |Bitwise AND of two `flags` values
`operator&(flags, flags::enum_type)`|Bitwise AND of a `flags` value and a flag denoted by an enum value
`operator&(flags::enum_type, flags)`|Bitwise AND of a `flags` value and a flag denoted by an enum value
`operator^(flags, flags)`           |Bitwise XOR of two `flags` values
`operator^(flags, flags::enum_type)`|Bitwise XOR of a `flags` value and a flag denoted by an enum value
`operator^(flags::enum_type, flags)`|Bitwise XOR of a `flags` value and a flag denoted by an enum value