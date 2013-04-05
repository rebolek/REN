# RENO - REadable NOtation

## Introduction

RENO is simple yet powerful data exchange language. It's very human friendly and pleasant to read. See for yourself.

    RENO[
        Title: "RENO Example"
        Type: example
        Date: 5-4-2013
        Version: 0.0.1
        Author: "Boleslav Brezovsky"
    ]
    
    "hello world"
    {
        hello multiline
        world
    }
    word
    3.14
    [a b c d]
    info: [time is 2:45 and date is 2013-4-5]
    person: object! [
        name: "Jaro"
        age: 1
        email: jaro@leto.com
    ]

    ; this is comment

    example: map! [
        "key1" "value1"
        "key2" http://value2.com
        "key3" value3@value.com
    ]

See? It's simple.

##Syntax

###Whitespaces

Whitespaces are space, tab and newline. They work as delimiters. There's no comma anywhere. You can put as many whitespaces as you want anywhere you want.

    a:
    [      1
    "b"
          c@d.e ]

is same as

    a: [1 "b" c@d.e]

###Comment  

Comments start with ;

    ; this is comment

###String

There are two types of string. Single line and multi line. Single line string tarts and ends with quotes. Multiline string starts with `{` and ends with `}`. String is UTF-8 encoded. Special characters are escaped with ^ (see table below)

    "single line"

    {multi
    line}

...table with escapes


###Word

Word contains alphanumerical characters, numbers and ( ??? ). Word cannot start with number. Words are used to construct domain specific languages (DSL) or dialects. More on DSL/dialects later.

    word after word

###Key (set-word)

Key (also set-word) is used to indicate that word should get following value. Format of set-word is word followed by colon.

    a: 1
    name: "Pepa"
    color: #FF00FF

###Integer

64bit integer number

    1
    -23242

###Floating point

Floating point number

    3.14
    1.2e34

###Email

Standard email - see [RFC5322](http://tools.ietf.org/html/rfc5322#section-3.4.1)

    test@example.com


###URI

Standard URI - see [RFC3986](http://tools.ietf.org/html/rfc3986)
    
    http://www.example.com


###Date and time

Date and time as defined in [RFC3339](http://www.ietf.org/rfc/rfc3339.txt)
You can use slash instead of T, so it's more human readable.

    12:45
    14-3-2013
    1-1-2000/13:20
    1937-01-01T12:00:27.87+00:20

###Block

    [ "this" is: block ]

###Object

Object is collection of set-words (keys) and values kept together in block and prefixed with object! word.

###Map

Map is similiar to object but can have strings as keys. 

##Structure

###Header

Header is optional but you are encouraged to use it. Unless you know what you are sending and where, you should add header as it enhances readability. Header can contain name, type, version, date, checksum and other useful informations about following data. Parse can for example just read header and decide if it makes sense to read rest of data (they may require higher version, data are expired etc.). Format of header is RENO followed by block of key/value pairs. Header can be empty also.


