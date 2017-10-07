[//]: # (User Guide document for the Generator functionality of the Campaign Logger gamemastering tool.)

[//]: # (Author: Esko Vesala.)

[//]: # (Date: 2017-10-06.)


# Generator Guide

This document explains how to create random table generators for the *Campaign Logger*  gamemastering utility.

For documentation on other Campaign Logger topics, please refer to the Campaign Logger site at https://campaign-logger.com/.


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

(For a longer version of this table, see the *nameLocation* table in the Campaign Logger generator library.)


### All Those Brackets

Note how the generator starts and ends with curly brackets. The JSON format uses plenty of brackets, and all of open brackets must be matched by a similar closing bracket. Mismatched brackets are probably the most common reason for JSON errors, so pay attention to them.

However, the spacing and indentation of the brackets does not matter. The table would work even if it was written as a single line. It is just customary to use a more spacious layout that makes the text easier to read.


### name

The *name* property defines the name that is shown the Campaign Logger user interface. The name can consist of several words.


### resultPattern

The *resultPattern* property defines what the generator outputs. If a table *name* is included within {curly brackets}, that table is called (rolled on) and the random result is displayed on the place of the table name.

The *resultPattern* string can also contain static text, like this:

    "resultPattern": "You are in {nameLocation}.",

This could result in:

> You are in Zambrastil.


### tables

The *tables* property contains one or more tables that the generator may call.

The *tables* property consists of a list of tables, and the JSON syntax dictates that lists must begin and end with square brackets:

    "tables": [
        ]

But that is not all: every table in the list must begin and end with curly brackets.

And if there are several tables in the *tables* property, there must be commas between the curly brackets that surround each table. (But not after the last one.)

Each of the tables has the following properties:

* **name**: As already seen, the *name* property gives the table a name that can be used for calling that table and outputting the result.

* **entries**: The *entries* property contains a list of the results that this table can produce. Each entry must be enclosed in quotation marks. There must be commas between each entry. (But not after the last entry.)

And because the *entries* property is a list of individual entries, JSON again requires that the list of entries must be surrounded with square brackets:

    "entries": [
    ]

Each entry must be enclosed within quotes, and separated by commas (again, no comma after the last entry):

      "Aorgareth",
      "Zambrastil"

Entries can be strings from a single character to an entire paragraph of text.


### That's it!

That is all you need for creating basic tables that produce randomly picked results from lists.

Your next step could be experimenting with more complex *resultPattern* definitions that include several table calls. Something like:

    "resultPattern": "In a {location}, you encounter {monster} that is guarding {treasure}.",


# Advanced Topics

The Campaign Logger generator engine offers also several advanced tools for creating more complex generators.


## Formatting the output

It's possible to add the following formatting controls to table output:

* **\r** - return
* **\n** - newline
* **\t** - tabulator

To add an indented footnote on the next line after an oracle's statement, you could use a *resultPattern* like this:

    "resultPattern": "{oracle statement} \r\n\t 1) This may not come true.",


## External Subtables

Not all tables used in the *resultPattern* definition must be located in the same file.

Campaign Logger comes with a standard set of help tables that can be called from any table by add the *lib:* string in front of the table name:

    {lib:generatorName#tableName}

    {private:tableName}

    {gen:generatorName#tableName}


## Weighing Values

If you want some table entries to be more common than others, you can specify a *multiplicity* property to it.

This example defines an entry that appears thee times more likely than unweighed values:

        {
          "m": 3,
          "v": "three times as common entry"
        },


## Variables

Variables can be a powerful feature when designing advanced generators. They can be defined using the optional *variables* property.

Variables are specified as a list of value pairs - the key (name of the variable) and its default value:

    "variables": {
      "variable 1": "1",
      "variable 2": "another value"
    },


## Template

The *_empty_template* table contains a structure for creating complex generators that are compatible with different genres. Table names and comment fields requiring customization are marked with *XXX*.

The template follows the recommended practices explained in the following section.


# Recommended Practices

This section is about practices that are by no means necessary, but can make it easier to edit and manage your table collection.

Complex generators (as opposed to simpler tables) use a standard structure to make table usage more uniform and allow support for various genres.


## A Closer Look at Table Properties

### name

This mandatory property specifies the name that shown in the list of available generators in the Campaign Logger user interface.

Complex tables (typically producing more than a single phrase of output) are named 'generators'. The generator names are capitalized (as in "Weather Generator") to make them stand out better in table listings.

Simple tables that output just a few words are named 'tables' and not capitalized (as in "weather table").

The above mentioned "Weather Generator" could produce a detailed weather report, maybe even several sentences long. A simple "weather table" could just print out "heavy rain" or "clear skies", for example.

**Note**: Table names must begin with a non-numeric character. So, instead of *4 seasons*, name the table *four seasons*, for example.


### resultPattern

A simple resultPattern can contain just a single call to a subtable:

    "resultPattern": "{color}",

This would call the color subtable (which in turn could be a more complex beast and call several different tables multiple times). Still, the  output from that single table call would be returned as the result of rolling on this table.

A more complex generator could have several calls in the result pattern, as in the Food Generator:

    "resultPattern": "{appearance} {recipe} with {ingredients} ({finishing})",

Note that in addition to the table calls, the result pattern can also contain static words that will be a part of the output every time. In this example, the ingredients will always be preceded by the word "with".


### Metadata Properties

Each JSON file can contain several optional properties for defining metadata (background information) about the file - like who has written it, when was it last updated, improvement ideas, and so on.

The metadata properties do not affect the functionality of the table, but they provide documentation that helps using and updating the tables.

Any of the metadata properties can be omitted. Still, using them is recommended, as they make sharing tables easier and can be useful reminders even for yourself.

Include whichever optional properties that are relevant for the file. All of the optional properties are included in the *_empty_template* sample file for reference.

The following optional properties can be used:

* **explanation**: A one-sentence explanation of the table: what kind of results can be expected.

* **structure**: A brief explanation of the structure of the file and the conventions used. This is useful especially for complex generators whose logic can be difficult to decipher. The *_empty_template* sample file contains a basic example of a structure property. You can modify it to your liking.

* **note**: Notes and instructions for other users, especially for those who plan to update the table. The note should make the logic easier to understand. for example explain some strange seeming details of the implementation. As an example, the modification table contains the following note:

      Note: Does not include 'quite', as its meaning can vary.

* **bugs**: If there are known problems with the table, they can be listed  here. This helps you (or whoever next works on the table) to notice the pitfalls, and maybe even fix them.

* **to do**: Information to you and other table authors about still missing functionality and ideas for expansion.

* **see**: If there are related generator files that could be helpful, they can be mentioned here.

* **date**: The date of the last update, preferably using the international notation of year-month-day (for example *2017-09-27*). This helps in sorting the tables and also lets others to see if the file is likely to still be in active development.

* **authors**: Persons who have contributed to the file, listed in chronological order (the newest contributor last). Listing all of the authors is not only polite, it also helps other users who may want to give feedback or ask for assistance.

* **sources**: This property is for listing the references and other information sources that have been helpful when creating the file. This helps others to expand the file and find more information about the subject.

* **genre**: If the table is intended for only certain genres, it can be specified here. This presents one way to scan for suitable tables. Commonly used genres are the following:

  * *fantasy* - imaginary realms of myth and magic
  * *futuristic* - science fiction worlds in
  * *historical* - some period of real-world history
  * *modern* - contemporary settings resembling the real world
  * *mythological* - mundane world with hidden, mythic secrets
  * *universal* - not specific to a genre

  Using these common categories helps to organize the tables in a standard way, but any genre definition can be used.

* **category**: The category property specifies a class in which the file belongs to. This can  help in organizing the generator files. Select one from the following:

  * *character* - PC and NPC generation  
  * *encounter* - events and random encounters
  * *item* - equipment and treasure generation
  * *magic* - spells and magic items
  * *meta* - metadata, such as templates and examples
  * *monster* - beast and creature generation
  * *name* - person, item and location names
  * *story* - plot and adventure generation
  * *utility* - help tables for use by other tables
  * *world* - locations, cultures and history

  While it is possible to specify also other categories, it is recommended to pick from the above list. This makes it easier to organize all tables in a uniform way.

* **tags**: Tags are a more freeform way to organize the generator files. They make it easier to find content that is related to a keyword. Tags should be preceded by the hash symbol (#) and separated by commas.  You can specify as many tags as you like.

  Examples of tags could be for example the following:

      "tags": "#sea, #caribbean, #pirates"

If some of the metadata properties are not relevant, they can of course be omitted. (For example if there are no relevant references for the "see" property.)

It is also possible to define additional properties in the JSON files. However, it is recommended to stick to the properties defined above. This way it is easier to automatically categorize the tables, for example.

Also tables can use these metadata properties, such as an *explanation* to add notes specific to a particular subtable. These metadata properties are completely optional.

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


**Note**: If a metadata property is not necessary for a file, either leave that property out, or leave its value field empty. For example, if your table has no bugs, do not fill the *bugs* property with "none known". Just leave it empty. Keeping unnecessary fields empty makes it easier to automatically find tables that need debugging, for example.


### Table Properties in Detail

#### Name

    "name": "",


#### Explanation

    "explanation": "",


#### Entries

The *entries* property contains a list of the items in the table. The whole of the list is enclosed in square brackets.

Individual entries are written in quotes. Entries can be character strings from a single letter to an entire paragraph of text.

There is a comma between each entry:

    "entries": [
      "entry",
      "another entry",
      "the final entry"
    ]

*Note*: The final entry in the list does not have a comma after it.


### Recommended Subtable Organization

It is a recommended practice to make any generators intended to be shared with other users as generic as possible, so that they are suitable for many different game worlds.

Therefore each generator should begin with generic tables whose results would work with a fantasy campaign as well as a science fiction setting. These are the subtables that the *resultPattern* should call by default.

Typically there should be at least generic tables named *common* and *rare*, for producing usual and unusual results for just about any setting.

After these generic tables, genre-specific tables should follow. These subtables should be reached only if specifically called with a *{lib:Generator#subtable}* call.


#### common


#### rare

    "name": "mythological",
    "explanation": "Imaginary X once rumoured to have existed.",


#### rare mythological

    "name": "rare mythological",
    "explanation": "Rare imaginary X once rumored to have existed.",


#### fantasy

    "name": "fantasy",
    "explanation": "Imaginary X for fantasy worlds.",

#### rare fantasy

    "name": "rare fantasy",
    "explanation": "Rare imaginary X for fantasy worlds.",

#### modern

    "name": "modern",
    "explanation": "X for modern worlds.",


#### rare modern

    "name": "rare modern",
    "explanation": "Rare X for modern worlds.",


#### futuristic

    "name": "futuristic",
    "explanation": "Imaginary X for science fiction worlds.",


#### rare futuristic

    "name": "rare futuristic",
    "explanation": "Rare imaginary X for science fiction worlds.",


### Special Subtables

The end of the file is a good location for subtables that are not called from other tables in the same file, but by external calls from other files.

An example of this kind case could be the color table that contains the "black and white" subtable in end. That table produces only black, grey and white results and could be called for example to describe items in a black & white photograph.

It is perfectly OK if there is overlap between the results produced by different tables.
