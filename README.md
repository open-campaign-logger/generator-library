# generator-library

This repository contains the generator library files for the Campaign Logger random generator (https://campaign-logger.com/generator).

The library tables can be called from other tables by using syntax like this:

    "{lib:metal}".

This would produce the name of a random metal from the 'metal' table in the library. For example:

> gold

For instructions on creating your own Campaign Logger generators, please see the separate *GeneratorGuide* document.


## Credits

Typically the authors of each table are listed in the initial comment block of the files. Additionally Johnn Four, Jochen Linnemann and Ron Calbick are given general credit for their multiple contributions to the table library.

## License

The generator library is licensed under the Apache-2.0 license: https://github.com/open-campaign-logger/generator-library/blob/developer/LICENSE

Some contributions use other, compatible licenses:
* CC BY-SA 2.5: https://creativecommons.org/licenses/by-sa/2.5/ (https://www.apache.org/legal/resolved.html#cc-sa)

To include such material special additions are needed, see below:

### CC BY-SA 2.5

Add the following attribution and license properties (given values are examples):

```json
  "attribution": {
    "creators": "ChristopherA and other contributors to RPGnet Wiki",
    "copyright": "RPGnet Wiki, https://wiki.rpg.net/index.php/RPGnetWiki:Copyrights",
    "licence": "https://creativecommons.org/licenses/by-sa/2.5/",
    "disclaimer": "https://wiki.rpg.net/index.php/RPGnetWiki:General_disclaimer",
    "source": "https://wiki.rpg.net/index.php/Aspects_List",
    "changes": "Translation into JSON file format for generator usage"
  },
  "licence": "https://creativecommons.org/licenses/by-sa/2.5/"
```
