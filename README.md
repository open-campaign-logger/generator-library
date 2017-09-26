# generator-library

This repository contains the generator library files for the Campaign Logger random generator (https://campaign-logger.com/generator).

The library tables can be called from other tables by using syntax like this:

    "{lib:metal}".

This would produce the name of a random metal from the 'metal' table in the library. For example:

> gold


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


### Optional Properties

Each JSON file can also contain several optional properties. The optional properties do not affect the functionality of the table, but they are documentation that helps using and updating the tables.

Any of the optional properties can be omitted. Still, using them is recommended, as they make sharing tables easier and can be useful reminders even for yourself.

Include whichever optional properties that are relevant for the file. All of the optional properties are included in the *_empty_template* sample file.

The following optional properties are used:

* **explanation**: A one-sentence explanation of the table: what kind of results can be expected.

* **structure**: All the files in the library should contain an introductory text that briefly explains the structure of the file and the conventions used. This way the user can see the basic information from any file, without referring to external documentation.

  A boilerplate introduction is included in the *_empty_template* file.

* **note**: Notes and instructions for users, especially to those who plan to update the table. The note could explain some quirks of the table that may be difficult to understad for a casual observer. As an example, the modification table contains the following note:

      Note: Does not include 'quite', as its meaning can vary.

* **to do**: Information to you and other table authors about still missing functionality and ideas for expansion.

* **see also**: If there are some related library files that could be helpful, mention them here.

* **date**: The date of the last update, preferably using the international notation of year, month and day. (for example *2017-09-26*). This helps others to see if the file is likely to be still in active development.

* **authors**: Persons who have contributed to the file, listed in chronological order (newest contributor last). Listing all of the authors is not only polite, but also helps other users to ask for assistance and comments, should the need arise.

* **sources**: List the references and other sources that have been helpful when creating the file. This helps others expand the file and seek clarification by themselves, if some details appear obscure to them.

If some of these are not relevant, they can of course be omitted. (For example if there are no relevant references for the "see also" note.)

Note: It is also possible to define additional properties in the JSON files. However, it is recommended to stick to the properties defined above. This way it is easier to automatically categorize the tables, for example.


### Tables

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

Produce entries for different types of genres and worlds.

The 'common' and 'rare' tables are generic and should be compatible with almost any world.

To retain compatibility with most settings, the  non-generic tables should be called only from other tables by specifying their name directly.

Some subtables intentionally overlap."

  "tables": [


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

Note how the final entry does not have a comma after it.



##### Weighing Values

  {
    "m": 10,
    "v": "{common}"
  },


### Subtables

#### common

#### rare

    "name": "mythological",
    "explanation": "Imaginary XXX once rumoured to have existed.",


#### rare mythological

    "name": "rare mythological",
    "explanation": "Rare imaginary XXX once rumoured to have existed.",


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
