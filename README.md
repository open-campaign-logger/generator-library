# generator-library

This repository contains the generator library files for the Campaign Logger random generator (https://campaign-logger.com/generator).

The library tables can be called from other tables by using syntax like this:

    "{lib:metal}".

This would produce the name of a random metal from the 'metal' table in the library. For example:

> gold


## JSON

Generator files are written using JSON (JavaScript Object Notation). As the JSON syntax can get complex to write manually, it is recommended to use a JSON editor for creating generator tables. Several JSON editors are freely available on the Internet.


## Table Structure

Complex generators (as opposed to simpler tables) use a standard structure to make table usage more uniform and allow support for various genres.


### name

This mandatory item specifies the name that shown in the list of available generators in the Campaign Logger user interface.

Complex tables (typically producing more than a single phrase of output) are named 'generators'. The generator names are capitalized (as in "Weather Generator") to make them stand out better in table listings.

Simple tables that output just a few words are named 'tables' and not capitalized (as in "weather table").

The above mentioned "Weather Generator" could produce a detailed weather report, maybe even several sentences long. A simple "weather table" could just print out "heavy rain" or "clear skies", for example.


### resultPattern

A simple resultPattern can contain just a single call to a subtable:

    "resultPattern": "{color}",

This would call the color subtable (which in turn could be a more complex beast and call several different tables multiple times). Still, the  output from that single table call would be returned as the result of rolling on this table.

A more complex generator could have several calls in the result pattern, as in the Food Generator:

    "resultPattern": "{appearance} {recipe} with {ingredients} ({finishing})",

Note that in addition to the table calls, the result pattern can also contain static words that will be a part of the output every time. In this example, the ingredients will always be preceded by the word "with".


### Variables

Variables is an optional property that is not usually needed in basic tables. However, variables can be a powerful feature when designing advanced generators.

Variables a specified as a list or value pairs - the name of the variable and the default value:

    "variables": {
      "variable 1": "1",
      "variable 2": "another value"
    },


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

* **category**: The category property specifies a class in which the file belongs to. This can  help in organizing the generator files. Select one from the following:

  * *character* - PC and NPC generation  
  * *encounter* - events and random encounters
  * *item* - equipment and treasure generation
  * *monster* - beast and creature generation
  * *name* - person, item and location names
  * *story* - adventure and plot generation
  * *world* - locations, cultures and history

  While it is possible to specify also other categories, it is recommended to pick from the above list. This makes it easier to organize all tables in a uniform way.

* **tags**: Tags are a more freeform way to organize the generator files. They make it easier to find content that is related to a keyword. Tags should be preceded by the hash symbol (#) and separated by commas.  You can specify as many tags as you like.

  Examples of tags could be for example the following:

      "tags": "#sea, #sailing, #pirates"

If some of the metadata properties are not relevant, they can of course be omitted. (For example if there are no relevant references for the "see" property.)

It is also possible to define additional properties in the JSON files. However, it is recommended to stick to the properties defined above. This way it is easier to automatically categorize the tables, for example.

**Note**: If a metadata property is not necessary for a file, either leave that property out, or leave its content field empty. For example, if your table has no bugs, do not fill the *bugs* property with "none known". Just leave it empty. Keeping unnecessary fields empty makes it easier to automatically find tables that need debugging, for example.


### Tables

The tables property contains a list of table definitions,

    "tables": [
        {
          <tables are located here>
        }
    ]

Tables can also use the same metadata properties as the file, such as *explanation* to add notes to anyone who reads or edits the table. Metadata properties are completely optional for tables.

Table syntax is like the following:

    "tables": [
      {
        "name": "name of the table",
        "explanation": "Brief description.",
        "entries": [
          "entry ",
          "another entry",
          "the final entry"
        ]
      }
    ]


#### Name

    "name": "XXX",


#### Explanation

    "explanation": "XXX",


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



##### Weighing Values

This example defines an entry that appears thee times more likely than unweighed values:

    {
      "m": 3,
      "v": "three times as common entry"
    },


### Subtables

Produce entries for different types of genres and worlds.

The 'common' and 'rare' tables are generic and should be compatible with almost any world.

To retain compatibility with most settings, the  non-generic tables should be called only from other tables by specifying their name directly.

Some subtables intentionally overlap.


#### common


#### rare

    "name": "mythological",
    "explanation": "Imaginary XXX once rumoured to have existed.",


#### rare mythological

    "name": "rare mythological",
    "explanation": "Rare imaginary XXX once rumored to have existed.",


#### fantasy

    "name": "fantasy",
    "explanation": "Imaginary XXX for fantasy worlds.",

#### rare fantasy

    "name": "rare fantasy",
    "explanation": "Rare imaginary XXX for fantasy worlds.",

#### modern

    "name": "modern",
    "explanation": "XXX for modern worlds.",


#### rare modern

    "name": "rare modern",
    "explanation": "Rare XXX for modern worlds.",


#### futuristic

    "name": "futuristic",
    "explanation": "Imaginary XXX for science fiction worlds.",


#### rare futuristic

    "name": "rare futuristic",
    "explanation": "Rare imaginary XXX for science fiction worlds.",


### Special Subtables

The end of the file is a good location for subtables that are not called from other tables in the same file, but by external calls from other files.

An example of this kind case could be the color table that contains the "black and white" subtable in end. That table produces only black, grey and white results and could be called for example to describe items in a black & white photograph.


## Template

The *_empty_template* table contains a structure for creating complex generators that are compatible with different genres. Table names and comment fields requiring customization are marked with *XXX*.


## Credits

Typically the authors of each table are listed in the initial comment block of the files. Additionally Johnn Four, Jochen Linnemann and Ron Calbick are given general credit for their multiple contributions to the table library.
