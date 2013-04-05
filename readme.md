﻿# [人] - REN - REadable Notation

## Introduction

**REN** is simple yet powerful data exchange format. It's very human friendly and pleasant to read. 
Every value in **REN** has it's own type to allow easier describtion of your data. See for yourself.

There are strings:

    "hello world"

And there are words:
	
	hello world   ; hello world

Numbers are also supported in integer and floating point form.

    1
	-1
	3.14
	-2.354e3

Another type will tell you where to get help:

    http://server.com/readme
	
You get the idea.

The basic form of data collection in REN is block. 
	
    [1 "a" e@ma.il and so://on]	
	
You can also add header to data transmission to inform the loader about type of data.
	
    REN[
        Title: "REN Example"
        Type: example
        Date: 5-4-2013
		Expires: never
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

###Why?

It's much easier to eyes than XML and JSON and at the same time it's at least as powerful. Broader datatype support is also great property of REN. 

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

Comments start with `;`

    ; this is comment

###String

There are two types of string. Single line and multi line. Single line string tarts and ends with quotes. Multiline string starts with `{` and ends with `}`. String is UTF-8 encoded. Special characters are escaped with `^` (see table below)

    "single line"

    {multi
    line}


<table>
<tr><th>Character</th>
<th>Definition</th>
<tr>
<td>
^"
</td><td>
Inserts a double quote (").
</td>
<tr>
<td>
^}
</td><td>
Inserts a closing brace (}).
</td>
<tr>
<td>
^^
</td><td>
Inserts a  caret (^).
</td>
<tr>
<td>
^/
</td><td>
Starts a new line.
</td>
<tr>
<td>
^(line)
</td><td>
Starts a new line.
</td>
<tr>
<td>
^-
</td><td>
Inserts a tab.
</td>
<tr>
<td>
^(tab)
</td><td>
Inserts a tab.
</td>
<tr>
<td>
^(page)
</td><td>
Starts a new page.
</td>
<tr>
<td>
^(back)
</td><td>
Erases one character to the left of the insertion point.
</td>
<tr>
<td>
^(null)
</td><td>
Inserts a null character.
</td>
<tr>
<td>
^(escape)
</td><td>
Inserts an escape character.
</td>
<tr>
<td>
^(letter)
</td><td valign="top" bgcolor="white">
Inserts control-letter (A-Z).
</td>
<tr>
<td>
^(xxxx)
</td><td>
Inserts an Unicode character by hexidecimal (xxxx) number.
</td></tr></table>



###Word

Word contains alphanumerical characters, numbers and any of following characters:

    ? ! . ' + - * & | = _

Word cannot start with number. Words are used to construct domain specific languages (DSL) or dialects. More on DSL/dialects later.

    word after word

Some syntax is reserved for future but not currently implemeted:
	
	'world        ; key reference with "'"
	hello :world  ; hello and get 'world key value
	hello: world  ; set hello to 'world
    hello: :world ; set hello to 'world key value
	
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

Header is optional but you are encouraged to use it. Unless you know what you are sending and where, you should add header as it enhances readability. Header can contain name, type, version, date, checksum and other useful informations about following data. Parse can for example just read header and decide if it makes sense to read rest of data (they may require higher version, data are expired etc.). Format of header is REN followed by block of key/value pairs. Header can be empty also.


