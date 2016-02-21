# CONTENTdm Field Inspector, a tool to get values from a specific field in a collection.

## Overview

This tool provides two commands that allow you to select a field in a CONTENTdm collection and get a list of all unique values in that field.

## Requirements

PHP command-line interface 5+.

## Usage

There are two steps in generating a report of field values, 1) getting a list of CONTENTdm field 'nicknames' and 2) generating the list of unique values for a specific nickname.

For example, to get a list of the field nicknames for a collection, run this command:

```php cdminspect --what=nicknames --alias=vanpunk```

where the value of `--alias` is the CONTENTdm collection's alias (string identifying a particular collection) without the leading `/`). This will produce a list of fields and their nicknames:

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
php cdminspect --what=values --nickname=bands --alias=vanpunk
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

*cdm_url: The full URL to the CONTENTdm server's web API. Can also be configured in the `$ws_url` variable with the cdminspect script.
*output_file: Path to a file where the output of `--what=values --nicknames=` will be saved. Can be absolute of relative to the cdminspect script. Default is './cdminspect_output.txt'.
*email: An email address or comma-separated list of email addresses that will be mailed the contents of this file. Only works on systems that allow email from PHP scripts.
*error_log: Path to a file where errors are logged. Default is './error_log.txt'.

## Background

This tool was cobbled together in support of Simon Fraser University (SFU) Library's migration from CONTENTdm to Islandora. We needed a way to get an overview of how consistently (or inconsistenly) date values, rights statements, etc. were in our 120+ CONTENTdm collections.


cdminspect is not written using modern PHP best practices since it was developed quickly to meet a specific need. But, it does what it promises.

## Plugins

The main cdminspect script iterates over all the parent-level objects in the specified collection. It delegates actions to all the objects in the collections to plugins. Currently, there are only two plugins, one to get the field nicknames and one to loop through all the objects and get the value from a field. Other plugins could be added if someone can think of another useful task they want to perfom with this tool.

## Future

Once SFU has migrated away from CONTENTdm, its staff won't have any use for this utility... but that's not to say that it should be abandoned. Perhaps some other CONTENTdm site will take it on and make it more useful.

## License

GPL3.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)



