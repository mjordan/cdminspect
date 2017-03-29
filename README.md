# CONTENTdm Collection Inspector

A tool to a produce reports of various types about CONTENTdm collections.

## Overview

This tool provides several commands that allow you to:

1.  get a list of the pointers for all the objects in a collection.
2.  get a list of the file types (extensions) for simple CONTENTdm objects or the document type for compound objects in a collection.
3.  retrieve the .cpd (compound structure) files for all compound objects in a collection.
4.  select a metadata field in a CONTENTdm collection and get a list of all unique values in that field.

Only objects that are parent-level (that is, objects that are not children of CONTENTdm compound objects) are inspected.

## Requirements

PHP 5+ command-line interface.

## Installation and configuration

Clone this Github repo, or download the zip. No configuration is necessary, but you will probably want to change the value of the `$ws_url` variable in the cdminspect script to be your CONTENTdm server.

## Usage

All commands require the `--inspect` option (which tells cdminsect what to inspect) and the `--alias` option (which tells it what collection to inspect). Specific examples follow. The alias is the string which identifies a particular CONTENTdm collection and can be found in the URL for collections and objects within a collection. For example, the alias in the following CONTENTdm collection URL is 'vanpunk':

`http://content.lib.sfu.ca/cdm/landingpage/collection/vanpunk`

### CONTENTdm pointers

Running cdminspect with `--inspect=pointers` will generate a simple list of object pointers:

```php cdminspect --inspect=pointers --alias=vanpunk```

```
Retrieving object pointers for the '/vanpunk' collection...
..................................................................
872
905
937
954
975
992
1009
1026
1039
1058
```

and so on.

### Object types

To generate a report of the type of compound documents in a collection, use 'object_type' as the value of the  `--inspect` option:

```
php cdminspect --inspect=object_type --alias=vanpunk
```

The output contains one row per object, with the object's pointer, wheterh or not it is compound or simple, and the document type seperated by commas:

```
Analysing object types for the '/vanpunk' collection...
.....................
240,compound,Document
645,compound,Document
1294,compound,Document
1395,compound,Document
1584,compound,Document
1859,compound,Document
2294,compound,Document
2608,compound,Document
3202,compound,Document
3476,compound,Document
3977,compound,Document
4342,compound,Document
4871,compound,Document
5180,compound,Document
5353,compound,Document
5738,compound,Document
6323,compound,Document
6489,compound,Document
7374,compound,Document
7893,compound,Document
8593,compound,Document
```

The output for simple (i.e., single-file) objects shows the extension of the file:

```
1614,simple,jp2
1615,simple,jp2
1616,simple,jp2
1617,simple,jp2
1618,simple,jp2
1619,simple,jp2
1620,simple,jpg
1621,simple,jpg
1622,simple,pdf
```

### .cpd (compound structure) files

CONTENTdm represents the structure of a compound object in a .cpd file in either XML or JSON format. cdminspector can retrieve the XML version of these files for all compound objects in a collection using the following command:

```php cdminspect --inspect=cpd_files --alias=vanpunk```

A .cpd XML file will be generated and save for each compound object in the collection identified by the value of the `--alias` option. The files are saved in a directory named using the alias (e.g., "vanpunk_cpd_files") in the same directory where `cdminspect` is run.

### Metadata field values

Generating a report of field values involves two steps, 1) getting a list of CONTENTdm field 'nicknames' and 2) generating the list of unique values for a specific nickname.

For example, to get a list of the field nicknames for a collection, run this command:

```php cdminspect --inspect=nicknames --alias=vanpunk```

This command will produce a list of fields and their nicknames:

```
Field nicknames for Vancouver Punk Rock Collection

Field label => field nickname
=============================
Title => title
Bands => bands
Poster Artist => poster
Description => descri
Date => date
Display date => displa
Type => type
Format => format
Identifier => identi
Producer or Presenter => produc
Language => langua
Venue => venue
Dimensions (inches) => dimens
Poster text => postea
Rights => rights
Cover Artist => cover
Publisher => publis
Editor => editor
Author => author
Subjects => subjec
Place => place
Full text => full
Archival file => fullrs
OCLC number => dmoclcno
Date created => dmcreated
Date modified => dmmodified
CONTENTdm number => dmrecord
CONTENTdm file name => find
```

Then, to generate the list of unique values in a field, run the following command

```
php cdminspect --inspect=field_values --nickname=bands --alias=vanpunk
```

Running this command to get a list of all the unique values from the 'bands' field ('bands' is the nickname, to the right of the human-readable field name 'Bands') for the Vancouver Punk Rock Collection.

A sample of the output from this particular command is:

```
The Diefenbakers
The Dijits
The Dils
The Dinnettes
The Dishrags
The Dragons
The Droogs
Thee Atoms
The Eddy Dutchman Jazz Trio
The Edsells
The Enemy
The Enigmas
The Escorts
The Evaporators
The Exploited
The Explosions
The Fabulous Wallies
The Fabulous Wallys
The Fartz
The Fastbacks
The Feederz
```
Ah ha! Looks like using this tool has paid off: the 'Bands' field contains two versions of 'The Fabulous Wallies.'


## Other options

* `--cdm_url`: The full URL to the CONTENTdm server's web API. Can also be configured in the `$ws_url` variable with the cdminspect script.
* `--output_file`: Path to a file where the output will be saved. Can be absolute of relative to the cdminspect script. Default is './cdminspect_output.txt'. Even if this option is provided, the output is sent to STDOUT.
* `--email`: An email address or comma-separated list of email addresses that will be mailed the contents of this file. Only works on systems that allow email from PHP scripts.
* `--error_log`: Path to a file where errors are logged. Default is './error_log.txt'.

For example,

```
php cdminspect --inspect=values --nickname=bands --alias=vanpunk --output_file=/tmp/vanpunk.bands.txt
```

etc.

## Plugins

The main cdminspect script iterates over all the parent-level objects in the specified collection. It delegates actions to all the objects in the collections to plugins. Each of the values for `--inspect` (pointers, object_type, nicknames, and field_values) has its own plugin. Other plugins could be added if someone can think of another useful task they want to perfom with this tool.

## Background

This tool was cobbled together in support of Simon Fraser University (SFU) Library's migration from CONTENTdm to Islandora. We needed a way to get an overview of how consistently (or inconsistenly) date values, rights statements, etc. were in our 120+ CONTENTdm collections.

cdminspect is not written using modern PHP best practices since it was developed quickly to meet a specific need. But, it does what it promises.

## Future

Once SFU has migrated away from CONTENTdm, its staff won't have any use for this utility... but that's not to say that it should be abandoned. Perhaps some other CONTENTdm site will take it on and make it more useful.

## License

Public domain.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)
