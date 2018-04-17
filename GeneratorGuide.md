[//]: # (User Guide document for the Generator functionality of the Campaign Logger gamemastering tool.)

[//]: # (Authors: ELF Vesala, Peter Sotos.)

[//]: # (Date: 2018-04-17.)


# Generator Guide

This document explains how to create random table generators for the *Campaign Logger*  gamemastering utility.

For documentation on other Campaign Logger topics, please refer to the Campaign Logger site at https://campaign-logger.com/.

## Table of Contents

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Generator Guide](#generator-guide)
	- [Table of Contents](#table-of-contents)
	- [JSON](#json)
	- [Crash Course](#crash-course)
		- [All Those Brackets](#all-those-brackets)
		- [`name`](#name)
		- [`resultPattern`](#resultpattern)
		- [`tables`](#tables)
		- [Using your custom generators](#using-your-custom-generators)
		- [That's It!](#thats-it)
	- [Using Your Custom Generators](#using-your-custom-generators)
- [Advanced Topics](#advanced-topics)
	- [Subtables](#subtables)
	- [External Tables](#external-tables)
		- [Public Library Calls](#public-library-calls)
		- [Local Generator Calls](#local-generator-calls)
		- [Private Library Calls](#private-library-calls)
		- [External Subtable Calls](#external-subtable-calls)
	- [Weighing Values](#weighing-values)
	- [Variables](#variables)
		- [Local Variables](#local-variables)
			- [Setting Variables for a Single Entry](#setting-variables-for-a-single-entry)
			- [Setting Variables for a Table](#setting-variables-for-a-table)
		- [Global Variables](#global-variables)
	- [Pattern Matching and Replacing](#pattern-matching-and-replacing)
		- [Extending variable content](#extending-variable-content)
	- [Non-Repeating Results](#non-repeating-results)
	- [Random Number Generation](#random-number-generation)
	- [Formatting the output](#formatting-the-output)
		- [Emphasis](#emphasis)
		- [Linebreaks and Tabulation](#linebreaks-and-tabulation)
		- [Headings and Subheadings](#headings-and-subheadings)
		- [Superscipt and Subscript](#superscipt-and-subscript)
		- [Colors](#colors)
		- [Table Formatting](#table-formatting)
	- [Text Transformation](#text-transformation)
	- [Templates](#templates)
- [Recommended Practices](#recommended-practices)
	- [A Closer Look at Table Properties](#a-closer-look-at-table-properties)
		- [`name`](#name)
		- [`resultPattern`](#resultpattern)
		- [Metadata Properties](#metadata-properties)
		- [Table Properties in Detail](#table-properties-in-detail)
			- [`name`](#name)
			- [`explanation`](#explanation)
			- [`entries`](#entries)
- [Recommended Subtable Organization](#recommended-subtable-organization)
	- [Generic Subtables](#generic-subtables)
		- [common](#common)
		- [rare](#rare)
	- [World-Specific Subtables](#world-specific-subtables)
		- [myth](#myth)
		- [fantasy](#fantasy)
		- [modern](#modern)
		- [futuristic](#futuristic)
	- [Special Subtables](#special-subtables)
		- [Ideas for Special Subtables](#ideas-for-special-subtables)
- [Pro Tips](#pro-tips)
- [Debugging Tips](#debugging-tips)
- [Troubleshooting](#troubleshooting)

<!-- /TOC -->


## JSON

Campaign Logger random table generator files are written using the JSON (JavaScript Object Notation) syntax.

JSON files are just plain text that follows some specific rules. JSON files can be edited with just about any basic text editor (but preferably not with a full-fledged word processor that would mangle the text with its own formatting).

There are several JSON tutorials available online, for example:

* http://www.json.org/
* https://www.w3schools.com/js/js_json.asp
* https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON

But a deeper knowledge of JSON is not really necessary for writing Campaign Logger generators. Just follow this guide.

Although the JSON syntax may seem daunting at first, it also brings with it flexibility, expandability and powerful features that can be used for creating some quite advanced generators. A standardized file format also means that there are several readily available (and often free) tools that make the task easier.

It is recommended to write generator tables using a text editor with JSON support. Several specialized JSON editors are freely available online - for example:

* https://github.com/jdorn/json-editor
* https://github.com/rsuter/VisualJsonEditor (Windows only)
* https://atom.io/ (JSON plugins must be installed separately)

The generator JSON file names can contain spaces and other special characters supported by the file system.


## Crash Course

Let's have a look at a simple table that just picks a fantastic-sounding place name from a ready-made list. This example has been condensed to just two entries to save space:

    {
    	"name": "location table",
    	"resultPattern": "{location}",
    	"tables": [
    		{
    			"name": "location",
    			"entries": [
    				"Aorgareth",
    				"Zambrastil"
    			]
    		}
    	]
    }

(For a longer version of this table, see the `nameLocation.json` table in the Campaign Logger generator library.)


### All Those Brackets

Note how the generator starts and ends with curly brackets. The JSON format uses plenty of brackets, and all of open brackets must be matched by a similar closing bracket. Mismatched brackets are probably the most common reason for JSON errors, so pay attention to them.

However, the spacing and indentation of the brackets does not matter. The table would work even if it was written as a single line. It is just customary to use a more spacious layout that makes the text easier to read.


### `name`

The `name` property defines the name that is shown the Campaign Logger user interface. The name can contain alphanumeric characters (`a` - `z`, `A` - `Z`, `0` - `9`), as well as underscore (`_`) and space (` `) characters. Note that for example a dash (~~-~~) is not allowed. It is recommended not to start the name with a number, as calling the table from another table will then not work.


### `resultPattern`

The `resultPattern` property defines what the generator outputs. If a table `name` is included within `{`curly brackets`}`, that table is called (rolled on) and the random result is displayed on the place of the table name.

The `resultPattern` string can also contain static text, like this:

    "resultPattern": "You are in {nameLocation}.",

This could result in:

> You are in Zambrastil.


### `tables`

The `tables` property contains one or more tables that the generator may call. This is a list of tables (even for one table), and the JSON syntax dictates that lists must begin and end with square brackets:

    "tables": [
        ]

But that is not all: every table in the list must begin and end with curly brackets.

And if there are several tables in the `tables` property, there must be commas between the curly brackets that surround each table. (But not after the last one.)

Each of the tables has the following properties:

* **`name`**: As already seen, the `name` property gives the table a name that can be used for calling that table and outputting the result. Again, the name can contain alphanumeric characters (`a` - `z`, `A` - `Z`, `0` - `9`), as well as underscore (`_`) and space (` `) characters. Note that for example a dash (`-`) is not allowed. It is recommended not to start the name with a number, as calling the table from another table will then not work.

* **`entries`**: The `entries` property contains a list of the results that this table can produce. Each entry must be enclosed in quotation marks. There must be commas between each entry. (But not after the last entry.)

And because the `entries` property is also a list of individual entries, JSON again requires that the list of entries must be surrounded with square brackets:

    "entries": [
    ]

Each entry must be enclosed within quotes, and separated by commas (again, no comma after the last entry):

      "Aorgareth",
      "Zambrastil"

Entries can be strings ranging from a single character to an entire paragraph of text, as long as the text is included between quotation marks.

### Using your custom generators

There is a button underneath the Log Entry section of the Campaign Logger called "Show Generators". Clicking on this button will show your custom generators.

### That's It!

That is all you need for creating basic tables that produce randomly picked results from lists.

Your next step could be experimenting with more complex `resultPattern` definitions that include several table calls. Something like:

    "resultPattern": "In the {location}, you encounter a {monster} that is guarding a {treasure}.",


## Using Your Custom Generators

To install a new generator to Campaign Logger, click the **Options** button (the cogwheel icon) and select the **Manage Custom Generators** option. From the Generators drop down menu, select the **Add new generator...** option and click the **OK** button.

Now select and copy the new generator text to the clipboard (**Ctrl+C**), select the default text from the New Generator window (**Ctrl+A**), and replace the default generator content by pasting the copied generator text into the text box (**Ctrl+V**). Click the **Save** button.

In the Campaign Logger user interface, there is a button underneath the *Log Entry* section called **Show Generators**. Clicking on this button will show your custom generators.

The Generators display can have several tabs. The *Uncategorized* tab shows the generators that do not have their `Categories` property defined. Other generators are listed under different tabs, according to their `Categories` value.


# Advanced Topics

The Campaign Logger generator engine offers also several advanced tools for creating more complex generators.


## Subtables

As seen in the in *Crash Course* section above, a table can call another table located in the same file:

  "resultPattern": "{location}",

		"resultPattern": "In the {location}, you encounter a  {monster} that is guarding {treasure}.",

A string inside the {curly braces} is the `name` property of a table in the same JSON file, and the table call string will be replaced in the generator output by the result of that subtable call. For example:

*In the dungeon, you encounter a dragon that is guarding a vast hoard of gold.*


## External Tables

Not all tables that are called by your table must be located in the same file. You can call another table in three different ways, depending on where the external table is located.


### Public Library Calls

The Campaign Logger server has a standard library of help tables that can be called from any table by adding the `lib:` string in front of the table name:

    {lib:generatorName}

For public library calls, the generator name used in the call is the name of the JSON file.


### Local Generator Calls

Help tables installed on your own Campaign Logger setup using the **Manage Custom Generators** option can be called from any table by adding the `gen:` string in front of the table name:

    {gen:generatorName}

For local generator calls, the generator name used in the call is the `name` property of the generator.


### Private Library Calls

Help tables installed on the *Private Library* section of the **Manage Custom Generators** option can be called from any table by adding the `private:` string in front of the table name:

    {private:generatorName}

For private library calls, the generator name used in the call is the `name` property of the generator.


### External Subtable Calls

The external table call can also specify a subtable within the called generator. To do that, insert a hash mark (#) after the file name, and then the name of the subtable.

    {lib:generatorName#subtable name}

Note that there are no spaces in front of after the `#` character. The subtable name can contain spaces.


## Weighing Values

If you want some table entries to be more common than others, one way to do that is to add more entries for the more common result. For example, a generator randomizing the fingers of a hand could look like the following:

				{
					"name": "finger",
					"entries": [
						"thumb",
						"finger",
						"finger",
						"finger",
						"finger"
					]
				},

But the same effect can be achieved with a handier method (sorry!). The  *multiplicity* property (`m`) defines a value specifying how many times more likely that entry will be selected, when compared to an ordinary single entry. The above finger example could be written like this:

				{
					"name": "finger",
					"entries": [
						"thumb",
						{
							"m": 4,
							"v": "finger"
						},
					]
				},

The `"m"` property causes this entry to appear four times as often as an ordinary entry. The `"v"` property contains the output produced by that entry.


## Random Number Generation

The `dice:` function allows you to simulate dice rolls - even for dice for which there is no physical die available. The following dice call would produce results between 13 and 31:

    {dice:3d7+10}

Note that only one modifier (such as the `+10` in the above example) is supported. The modifier can only be a plus (`+`) or minus (`-`) operation, not for example multiplier (~~*~~) or another die roll.


## Variables

Variables can be a powerful feature when designing advanced generators. They can be defined using the optional `variables` property.

Variables are specified as a list of value pairs - the key (name of the variable) and its default value:

    "variables": {
      "variable 1": "1",
      "variable 2": "another value"
    },

**Note**: Variable names cannot begin with a number. Otherwise they can contain alphanumeric characters (`a` - `z`, `A` - `Z`, `0` - `9`), as well as underscore (`_`) and space (` `) characters. Note that for example a dash (`-`) is not allowed.


### Local Variables

#### Setting Variables for a Single Entry

		{
			"v": "hostage",
			"set": {
				"actant": "captive",
				"actants": "captives",
				"actantgroup": "group of captives"
			}


#### Setting Variables for a Table

		{
		     "name": "captive",
		     "set": {
		            "actant": "captive",
		            "actants": "captives",
		     },
		     "entries": [
		       "prisoner",
		       "hostage"
		     ]
		   },


### Global Variables


## Pattern Matching and Replacing

		{
		 "resultPattern": "{var:firstLevel}",
		 "variables": {
		   "firstLevel": "{table}",
		   "secondLevelHelper": "table",
		   "secondLevel": "{{var:secondLevelHelper}2}",
		   "thirdLevelHelper1": "table",
		   "thirdLevelHelper2": 42,
		   "thirdLevel": "{{{{var:thirdLevelHelper1}{var:thirdLevelHelper2}x}y}z}"
		 },
		 "tables": [
		   {
		     "name": "table",
		     "entries": [
		       "First level var-to-table call"
		     ]
		   },
		   {
		     "name": "table2",
		     "entries": [
		       "Second level var-to-table call"
		     ]
		   },
		   {
		     "name": "table42x",
		     "entries": [
		       "table42"
		     ]
		   },
		   {
		     "name": "table42y",
		     "entries": [
		       "table42"
		     ]
		   },
		   {
		     "name": "table42z",
		     "entries": [
		       "{table3}"
		     ]
		   },
		   {
		     "name": "table3",
		     "entries": [
		       "Third level var-to-table call"
		     ]
		   }
		 ]
		}


### Extending variable content

Add more content into a variable:

			{ "x": "{var:x} new text" }




## Non-Repeating Results

Let's say you have created a *treasure* generator. A `{treasure}` call could well produce the following result:

> a golden scepter, a crown, a golden scepter, a golden scepter

Luckily it is easy to avoid duplicated results like the scepter in the example above. Just add an exclamation mark to the front of the table name, and call the treasure table like this:

				{!treasure}

This kind of a *non-repeating* call creates a temporary copy of the table, and with each call removes the selected result from the temporary table. This means that a result will not occur more than once.

Technically the standard `{table}` call refers to the normal instance of the table, while a non-repeating `{!table}` call refers to a temporary copy. The normal instance's entries remain unchanged. The entries in the temporary copy get fewer and fewer until the temporary table is empty. When the temporary table is empty, non-repeating calls return only empty results.

Unique table instances share the same variables and globals with the normal table instances. Unique instances have the same scope as local variables, i.e. the results remain unique only when called from the same generator file.


## Formatting the output

### Emphasis

The following formatting commands can be used to add emphasis to output:

* The `{u|underlined}` string will be displayed as _underlined_.

* The `b|bolded}` string will be displayed as **bold**.

* The `i|italics}` string will be displayed as *italics*.

* The `s|strike-through}` string will be displayed as ~~strike-through~~.

It is also possible to combine several formatting commands:

		{ubis│underlined, bolded, italics and struck-through text}


### Linebreaks and Tabulation

It's possible to control horizontal and vertical spacing with the following control characters:

* **`\r`** - return
* **`\n`** - newline
* **`\t`** - tabulator

To add an indented footnote on the next line after an oracle's statement, you could use a `resultPattern` like this:

    "resultPattern": "{oracle statement} \r\n\t 1) This may not come true.",


### Headings and Subheadings

Start a line with two equal signs `==` and a space, write your text, and end the line with a space and two equal signs. Now you have a first level headline. Use three equal signs for level two, four for level three, etc. The lowest subheading level is 6, marked with 7 equal signs.

		== Level 1 Headline ==

		=== Level 2 Headline ===

		==== Level 3 Headline ====

		===== Level 4 Headline =====

		====== Level 5 Headline ======

		======= Level 6 Headline =======


### Superscipt and Subscript

* Superscript: The `{/|Superscript}` string is displayed  as superscript.

* Subscript: The `{\|Subscript}` string is displayed as subscript.


### Colors

Text can be colored by using the hash character `#` followed by a six-digit hexadecimal RGB color value.

In the RGB color value, the first 2 characters (0 - F) specify the intensity of the red color component, the following 2 the intensity of the green color component and the last 2 character the intensity of the blue color component. The lowest value for each component is `00`, and the highest value `FF`.

For example the following formatting would produce bolded and bright red text:

		{b#CC0000|bold red text}

RR - the red component 0-255 in hexadecimal notation, i.e. 0 = 00, 255 = FF
GG - the green component ...
BB - the blue component ...


### Table Formatting

Generator output can be formatted as table by using the following syntax:

		|| one || two || three\n
		|| a   || b   || c

This will produce the following output

one | two | three
--- | --- | -----
a   | b   | c

Note that the pairs of vertical bars (`||`) must be separated from table cell contents by at least one space (` `). Lines must start immediately with vertical bars (with no space in front), and lines must end without vertical bars.

 To clarify:

		||<space>one<space>||<space>two<space>||<space>three\n||<space>a<space>||<space>b<space>||<space>c

**Tip**: If you create a table with only one cell, that will produce a box around the output. Just start the output with the `|| `string. This could be used for example to highlight readout sections in an adventure generator.


## Text Transformation

* Upper case: The `{b|THIS is an example|ucase}` string is displayed as "THIS IS AN EXAMPLE".

* Lower case: The `{b|THIS is an example|lcase}` string is displayed as "this is an example".

* Capitalization: The `{b|THIS is an example|caps}` string is displayed as "This Is An Example".

* Title case: The `{b|THIS is an example|tcase}` string is displayed as "This Is an Example".

* Sentense case: The `{b|THIS is an example|scase}` string is displayed as "THIS is an example".


## Templates

The `_template_empty.json` file (note the initial underscore character) in the generator repository contains a ready-made structure for creating complex generators that can be used with different types of genres (such as fantasy and modern worlds).

Also available is a simplified template for short help tables: `_template_simple.json`.

Both templates have locations that need to be filled in for each new table, such as the table names and comment fields that require customization. These locations are marked with `XXX`.

The templates follow the recommended practices explained in the following section.


# Recommended Practices

This section is about practices that are by no means necessary, but can make it easier to edit and manage your table collection.

Complex generators (as opposed to simpler tables) use a standard structure to make table usage more uniform and allow support for various genres.


## A Closer Look at Table Properties

### `name`

This mandatory property specifies the name that shown in the list of available generators in the Campaign Logger user interface.

Table names can include spaces. They must begin with a non-numeric character. So, instead of ~~4 seasons~~, name the table as *four seasons*, for example.

Complex tables (typically producing more than a single phrase of output) are named 'generators'. It is a recommended practice to capitalized generator names (for example "Weather Generator") to make them stand out better in table listings.

Simple tables that output just a few words are not capitalized (as in "weather").

The above mentioned "Weather Generator" could produce a detailed weather report, maybe even several sentences long. A simple "weather table could just print out "heavy rain" or "clear skies", for example.


### `resultPattern`

A simple `resultPattern` can contain just a single call to a subtable:

    "resultPattern": "{color}",

This would call the `color` subtable (which in turn could be a more complex beast and call several different tables multiple times). Still, the  output from that single table call would be returned as the result of rolling on this table.

A more complex generator could have several calls in the result pattern, as in the *food* table:

    "resultPattern": "{appearance} {recipe} with {ingredients} ({finishing})",

Note that in addition to the table calls, the result pattern can also contain static words that will be a part of the output every time. In this example, the ingredients will always be preceded by the word "with".


### Metadata Properties

Each JSON file can contain several optional properties for defining metadata (background information) about the file - like who has written it, when was it last updated, improvement ideas, and so on.

The metadata properties do not affect the functionality of the table, but they provide documentation that helps using and updating the tables.

Any of the metadata properties can be omitted. Still, using them is recommended, as they make sharing tables easier and can be useful reminders even for yourself.

Include whichever optional properties that are relevant for the file. All of the optional properties are included in the `_template_empty.json` sample file for reference.

The following optional properties can be used:

* **`explanation`**: A one-sentence explanation of the table: what kind of results can be expected.

* **`structure`**: A brief explanation of the structure of the file and the conventions used. This is useful especially for complex generators whose logic can be difficult to decipher. The `_template_empty.json` sample file contains a basic example of a structure property. You can modify it to your liking.

* **`note`**: Notes and instructions for other users, especially for those who plan to update the table. The note should make the logic easier to understand. for example explain some strange seeming details of the implementation. As an example, the `modifier.json` generator contains the following note:

      "note": "Does not include 'quite', as its meaning can vary.",

* **`format`**: Help tables that are called by different tables must have their content worded in a format that is compatible with the calls. The property can be used to add about a note about this - for example if the entries should match singular or plural forms, or both. This example is from the `complication.json` table:

		"format": "'X is/are {complication}'. No full stop in the end.",


* **`bugs`**: If there are known problems with the table, they can be listed  here. This helps you (or whoever next works on the table) to notice the pitfalls, and maybe even fix them.

* **`to do`**: Information to you and other table authors about still missing functionality and ideas for expansion.

* **`see`**: If there are related generator files that could be helpful, they can be mentioned here.

* **`date`**: The date of the last update, preferably using the international notation of year-month-day (for example *2017-09-27*). This helps in sorting the tables and also lets others to see if the file is likely to still be in active development.

* **`authors`**: Persons who have contributed to the file, listed in chronological order (the newest contributor last). Listing all of the authors is not only polite, it also helps other users who may want to give feedback or ask for assistance.

* **`sources`**: This property is for listing the references and other information sources that have been helpful when creating the file. This helps others to expand the file and find more information about the subject.

* **`genre`**: If the table is intended for only certain genres, it can be specified here. This presents one way to scan for suitable tables. Commonly used genres are the following:

  * **fantasy** (imaginary realms of myth and magic)
  * **futuristic** (science fiction worlds in)
  * **historical** (some period of real-world history)
  * **modern** (contemporary settings resembling the real world)
  * **myth** (mundane world with hidden, mythic secrets)
  * **universal** (not specific to a genre)

  Using these common categories helps to organize the tables in a standard way, but any genre definition can be used.

* **`categories`**: The categories property specifies a class in which the file belongs to. This is used for organizing the generator files in the Campaign Logger user interface. It is recommended to select one of the following values:

  * **character** (PC and NPC generation)
  * **encounter** (events and random encounters)
  * **item** (equipment and treasure generation)
  * **magic** (spells and magic items)
  * **meta** (metadata, such as templates and examples)
  * **monster** (beast and creature generation)
  * **name** (person, item and location names)
  * **story** (plot and adventure generation)
  * **utility** (help tables for use by other tables)
  * **world** (locations, cultures and history)

  The categories property can contain freeform text, so it is possible to specify also other categories. But using a value from the above list makes it easier to organize tables by different contributors in a uniform way.

* **`tags`**: Tags are a more freeform way to organize the generator files. They make it easier to find content that is related to a keyword. Tags should be preceded by the hash symbol (`#`) and separated by commas.  You can specify as many tags as you like.

  Examples of tags could be for example the following:

      "tags": "#sea, #caribbean, #pirates"

If some of the metadata properties are not relevant, they can of course be omitted. (For example if there are no relevant references for the `see` property.)

It is also possible to define additional properties in the JSON files. However, it is recommended to stick to the properties defined above. This way it is easier to automatically categorize the tables, for example.

Also tables can use these metadata properties, such as an `explanation` to add notes specific to a particular subtable. These metadata properties are completely optional.

This example from the direction table contains a note explaining why the called subtables are named as they are:

    {
      "name": "rare",
      "explanation": "Rare compass direction systems.",
      "note": "A table name cannot begin with a number!",
      "entries": [
        "{sixteen winds}",
        "{thirtytwo winds}"
      ]
    },


**Note**: If a metadata property is not necessary for a file, either leave that property out, or leave its value field empty. For example, if your table has no bugs, please _do not_ fill the *`bugs`* property with "none known". Just leave it empty. Keeping unnecessary fields empty makes it easier to automatically find tables that need debugging, for example.


### Table Properties in Detail

#### `name`

    "name": "",


#### `explanation`

    "explanation": "",


#### `entries`

The `entries` property contains a list of the items in the table. The whole of the list is enclosed in square brackets.

Individual entries are written in quotes. Entries can be character strings from a single letter to an entire paragraph of text.

There is a comma between each entry:

    "entries": [
      "entry",
      "another entry",
      "the final entry"
    ]

*Note*: The final entry in the list does not have a comma after it.


# Recommended Subtable Organization

It is a recommended practice to make any generators intended to be shared with other users as generic as possible, so that they are suitable for many different worlds.

Therefore each generator should begin with generic tables whose results would work with a fantasy campaign as well as a science fiction setting. These are the subtables that the `resultPattern` should call by default.

Typically there should be at least generic tables named `common` and `rare`, for producing usual and unusual results for just about any setting.


## Generic Subtables

### common

The `common` subtable is intended for common and generic variants of the table's subject - stuff that would be well-known in just about any kind of world.


### rare

The `rare` subtable should contain rarer variants of the table's subject - but still those that could exist in almost any world.


## World-Specific Subtables

After the generic `common` and `rare` tables, genre-specific tables should follow. These world-specific subtables should be reached only if specifically called with a `{lib:Generator#subtable}` type call.


### myth

The `myth` subtable is the right place for variants based on actual myths or beliefs. They are great for modern or historical worlds that contain some supernatural or mythic elements, but not far out fantasy fare.


### fantasy

The `fantasy` subtable is the right place for variants that typically exist only in worlds of high or epic fantasy (i.e. are not based on actual myths or beliefs). Fantasy tables can contain call of for example the following subtables in the `special` subtable: *magic* and *primitive*.


### modern

The `modern` subtable is intended for variants that are met only in worlds depicting modern or near history.


### futuristic

The `futuristic` subtable is the place for variants that belong in the world of science fiction or science fantasy - i.e. variants that do not exist yet.


## Special Subtables

The end of the file is a good location for subtables that are may be called by not only the tables in the same file, but also by external calls from other files.

An example of this kind case could be the *color* table that contains the "black and white" subtable in end. That table produces only shades of black, grey and white and could be called for example to describe items in a black & white photograph.

It is perfectly OK if there is overlap between the results produced by different tables.

### Ideas for Special Subtables

To make your generator usable for many different situations, consider including some of the following special tables in the end of your generator:

* **magic** (magical variants)
* **primitive** (prehistoric variants)


# Pro Tips

The JSON format does not support proper comments, but it is possible to add a comment to each table using the `explanation` property. Sometimes it would be useful to add comments also between tables, for example to separate different groups of tables. This can be done adding an element like this between tables:

		{
			"******************************************": "SPECIAL CASES"
		},

An example of this kind of a separator is included in the generator template files.


# Debugging Tips

You can check the workings of a certain part of the generator by modifying the `resultPattern` string to execute a specific subtable call.

For example if you wanted to check how well the new subtable you just added works, you can temporarily change the `resultPattern` value into the name of that subtable:

		"resultPattern": "{my new subtable}",

Now the generator will produce results only from the specified subtable.  


# Troubleshooting

* **A table call is not working.**

  Check that the table `name` property is correctly formed. The name of a called table cannot begin with a number. Otherwise the name can contain alphanumeric characters (`a` - `z`, `A` - `Z`, `0` - `9`), as well as underscore (`_`) and space (` `) characters. Note that for example a dash (~~-~~) is not allowed.
