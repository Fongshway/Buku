<h1 align="center">Buku</h1>

<p align="center">
<a href="https://github.com/jarun/Buku/releases/latest"><img src="https://img.shields.io/github/release/jarun/buku.svg" alt="Latest release" /></a>
<a href="https://aur.archlinux.org/packages/buku"><img src="https://img.shields.io/aur/version/buku.svg" alt="AUR" /></a>
<a href="http://braumeister.org/formula/buku"><img src="https://img.shields.io/homebrew/v/buku.svg" alt="Homebrew" /></a>
<a href="https://packages.debian.org/search?keywords=buku&searchon=names&exact=1"><img src="https://img.shields.io/badge/debian-stretch+-blue.svg?maxAge=2592000" alt="Debian Strech+" /></a>
<a href="http://packages.ubuntu.com/search?keywords=buku&searchon=names&exact=1"><img src="https://img.shields.io/badge/ubuntu-zesty+-blue.svg?maxAge=2592000" alt="Ubuntu Zesty+" /></a>
<a href="https://github.com/jarun/buku/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-GPLv3-yellow.svg?maxAge=2592000" alt="License" /></a>
<a href="https://travis-ci.org/jarun/Buku"><img src="https://travis-ci.org/jarun/Buku.svg?branch=master" alt="Build Status" /></a>
</p>

<p align="center">
<a href="https://asciinema.org/a/8pm3q3n5s95tvat8naam68ejv"><img src="https://asciinema.org/a/8pm3q3n5s95tvat8naam68ejv.png" alt="Asciicast" width="734"/></a>
</p>

`buku` is a powerful bookmark management utility written in Python3 and SQLite3. When I started writing it, I couldn't find a flexible cmdline solution with a private, portable, merge-able database along with browser integration. Hence, `buku` (after my son's nickname).

`buku` fetches the title of a bookmarked web page and stores it along with any additional comments and tags. You can use your favourite editor to compose and update bookmarks. With multiple options to search bookmarks, including regex and a deep scan mode (particularly for URLs), finding a bookmark is very easy. Multiple search results can be opened in the browser at once.

Though a terminal utility, it's possible to add bookmarks to `buku` without touching the terminal! Refer to the section on [GUI integration](#gui-integration). If you prefer the terminal, thanks to the shell completion scripts, you don't need to memorize any of the options. There's an Easter egg to revisit random forgotten bookmarks too.

There are several [projects](#related-projects) based on `buku`, including a browser plug-in.

*Buku* is too busy to track you - no history, obsolete records, usage analytics or homing.

<p align="center">
<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=RMLTQ76JSXJ4Q"><img src="https://tuxtricks.files.wordpress.com/2016/12/donate.png" alt="Donate via PayPal!" title="Donate via PayPal!" /></a>
</p>

## Table of Contents

- [Features](#features)
- [Installation](#installation)
  - [Dependencies](#dependencies)
  - [Installing from this repository](#installing-from-this-repository)
    - [Running as a standalone utility](#running-as-a-standalone-utility)
    - [Debian package](#debian-package)
  - [Installing with a package manager](#installing-with-a-package-manager)
- [Shell completion](#shell-completion)
- [Usage](#usage)
  - [Cmdline options](#cmdline-options)
  - [Operational notes](#operational-notes)
- [GUI integration](#gui-integration)
  - [Add bookmarks from anywhere](#add-bookmarks-from-anywhere)
  - [Import bookmarks to browser](#import-bookmarks-to-browser)
- [Sync database across systems](#sync-database-across-systems)
- [As a library](#as-a-library)
- [Related projects](#related-projects)
- [Mentions](#mentions)
- [Examples](#examples)
- [Contributions](#contributions)
- [Copyright](#copyright)

## Features

- Fast, clean interface with distinct symbols
- Text editor integration to compose and edit
- Fetch page title from the web, add tags and comments
- Multiple search modes, including `deep` and `regex`
- Continuous search at prompt with on the fly mode switch
- Open bookmarks and search results in browser
- Import/export in HTML (FF, Chrome compatible) or Markdown
- Shorten and expand URLs
- Manual password protection using AES256 encryption
- Portable, merge-able database to sync between systems
- Additional options for power users (see help or man page)
- Completion scripts (Bash, Fish, Zsh), man page with examples
- Minimal dependencies

## Installation

### Dependencies

`buku` requires Python 3.3 or later.

To install package dependencies, run:

    $ sudo pip3 install urllib3 cryptography beautifulsoup4 requests
or on Ubuntu:

    $ sudo apt-get install python3-urllib3 python3-cryptography python3-bs4 python3-requests

### Installing from this repository

If you have git installed, run:

    $ git clone https://github.com/jarun/Buku/
or download the latest [stable release](https://github.com/jarun/Buku/releases/latest) or [development version](https://github.com/jarun/Buku/archive/master.zip).

Install to default location (`/usr/local`):

    $ sudo make install

To remove, run:

    $ sudo make uninstall
`PREFIX` is supported. You may need to use `sudo` with `PREFIX` depending on your permissions on destination directory.

#### Running as a standalone utility

`buku` is a standalone utility. From the containing directory, run:

    $ chmod +x buku.py
    $ ./buku.py

#### Debian package

If you are on a Debian (including Ubuntu) based system visit [the latest stable release](https://github.com/jarun/Buku/releases/latest) and download the `.deb` package. To install, run:

    $ sudo dpkg -i buku-$version-all.deb

Please substitute `$version` with the appropriate package version.

### Installing with a package manager

- [PyPi](https://pypi.python.org/pypi/buku/) (`$ pip install buku`)
- [AUR](https://aur.archlinux.org/packages/buku/)
- [Homebrew](http://braumeister.org/formula/buku)
- [Debian](https://packages.debian.org/search?keywords=buku&searchon=names&exact=1)
- [Ubuntu](http://packages.ubuntu.com/search?keywords=buku&searchon=names&exact=1)
- [Ubuntu PPA](https://launchpad.net/~twodopeshaggy/+archive/ubuntu/jarun/)
- [Void Linux](https://github.com/voidlinux/void-packages/tree/master/srcpkgs/buku) (`$ sudo xbps-install -S buku`)

## Shell completion

Shell completion scripts for Bash, Fish and Zsh can be found in respective subdirectories of [auto-completion/](https://github.com/jarun/Buku/blob/master/auto-completion). Please refer to your shell's manual for installation instructions.

## Usage

### Cmdline options

```
usage: buku [OPTIONS] [KEYWORD [KEYWORD ...]]

Powerful command-line bookmark manager. Your mini web!

POSITIONAL ARGUMENTS:
      KEYWORD              search keywords

GENERAL OPTIONS:
      -a, --add URL [tag, ...]
                           bookmark URL with comma-separated tags
      -u, --update [...]   update fields of an existing bookmark
                           accepts indices and ranges
                           refresh the title, if no edit options
                           if no arguments:
                           - update results when used with search
                           - otherwise refresh all titles
      -w, --write [editor|index]
                           open editor to edit a fresh bookmark
                           to update by index, EDITOR must be set
      -d, --delete [...]   remove bookmarks from DB
                           accepts indices or a single range
                           if no arguments:
                           - delete results when used with search
                           - otherwise delete all bookmarks
      -h, --help           show this information and exit
      -v, --version        show the program version and exit

EDIT OPTIONS:
      --url keyword        bookmark link
      --tag [+|-] [...]    comma-separated tags
                           clear bookmark tagset, if no arguments
                           '+' appends to, '-' removes from tagset
      -t, --title [...]    bookmark title; if no arguments:
                           -a: do not set title, -u: clear title
      -c, --comment [...]  description of the bookmark
                           clears description, if no arguments
      --immutable N        disable title fetch from web on update
                           N=0: mutable (default), N=1: immutable

SEARCH OPTIONS:
      -s, --sany           find records with ANY search keyword
                           this is the default search option
      -S, --sall           find records with ALL search keywords
                           special keywords -
                           "blank": entries with empty title/tag
                           "immutable": entries with locked title
      --deep               match substrings ('pen' matches 'opens')
      --sreg               run a regex search
      --stag               search bookmarks by a tag
                           list all tags, if no search keywords

ENCRYPTION OPTIONS:
      -l, --lock [N]       encrypt DB file with N (> 0, default 8)
                           hash iterations to generate key
      -k, --unlock [N]     decrypt DB file with N (> 0, default 8)
                           hash iterations to generate key

POWER TOYS:
      -e, --export file    export bookmarks in Firefox format html
                           export markdown, if file ends with '.md'
                           format: [title](url), 1 entry per line
                           use --tag to export only specific tags
      -i, --import file    import Firefox or Chrome bookmarks html
                           import markdown, if file ends with '.md'
      -m, --merge file     add bookmarks from another buku DB file
      -p, --print [...]    show record details by indices, ranges
                           print all bookmarks, if no arguments
                           -1 shows the bookmark with highest index
      -f, --format N       limit fields in -p or Json search output
                           N=1: URL, N=2: URL and tag, N=3: title
      -r, --replace oldtag [newtag ...]
                           replace oldtag with newtag everywhere
                           delete oldtag, if newtag not specified
      -j, --json           Json formatted output for -p and search
      --nc                 disable color output
      --np                 do not show the prompt, run and exit
      -o, --open [...]     browse bookmarks by indices and ranges
                           open a random bookmark, if no arguments
      --oa                 browse all search results immediately
      --shorten index|URL  fetch shortened url from tny.im service
      --expand index|URL   expand a tny.im shortened url
      --tacit              reduce verbosity
      --threads N          max network connections in full refresh
                           default N=4, min N=1, max N=10
      -V                   check latest upstream version available
      -z, --debug          show debug information and verbose logs

SYMBOLS:
      >                    title
      +                    comment
      #                    tags
```

### Operational notes

- The database file is stored in:
  - **$XDG_DATA_HOME/buku/bookmarks.db**, if XDG_DATA_HOME is defined (first preference) or
  - **$HOME/.local/share/buku/bookmarks.db**, if HOME is defined (second preference) or
  - the **current directory**.
- If the URL contains characters like `;`, `&` or brackets they may be interpreted specially by the shell. To avoid it, add the URL within single or double quotes (`'`/`"`).
- URLs are unique in DB. The same URL cannot be added twice.
- Bookmarks with immutable titles are listed with bold `(L)` after the URL.
- **Tags**:
  - Comma (`,`) is the tag delimiter in DB. A tag cannot have comma(s) in it. Tags are filtered (for unique tags) and sorted. Tags are stored in lower case and can be replaced, appended or deleted.
  - Releases prior to [v2.7](https://github.com/jarun/Buku/releases/tag/v2.7) support both capital and lower cases in tags. From v2.7 all tags are stored in lowercase. An undocumented option `--fixtags` is introduced to modify the older tags. It also fixes another issue where the same tag appears multiple times in the tagset of a record. Run `buku --fixtags` once.
- **Update** operation:
  - If --title, --tag or --comment is passed without argument, clear the corresponding field from DB.
  - If --url is passed (and --title is omitted), update the title from web using the URL.
  - If indices are passed without any other options (--url, --title, --tag, --comment and --immutable), read the URLs from DB and update titles from web. Bookmarks marked immutable are skipped.
  - Can update bookmarks matching a search, when combined with any of the search options and no arguments to update are passed.
- **Delete** operation:
  - When a record is deleted, the last record is moved to the index.
  - Delete doesn't work with range and indices provided together as arguments. It's an intentional decision to avoid extra sorting, in-range checks and to keep the auto-DB compaction functionality intact. On the same lines, indices are deleted in descending order.
  - Can delete bookmarks matching a search, when combined with any of the search options and no arguments to delete are passed.
- **Search** works in mysterious ways:
  - Case-insensitive.
  - Matches words in URL, title and tags.
  - --sany : match any of the keywords in URL, title or tags. Default search option.
  - --sall : match all the keywords in URL, title or tags.
  - --deep : match **substrings** (`match` matches `rematched`) in URL, title and tags.
  - --sreg : match a regular expression (ignores --deep).
  - --stag : search bookmarks by a tag, or list all tags alphabetically with usage count (if no arguments).
  - Search results are indexed serially. This index is different from actual database index of a bookmark record which is shown in bold within `[]` after the URL.
- **Encryption** is optional and manual. AES256 algorithm is used. To use encryption, the database file should be unlocked (-k) before using `buku` and locked (-l) afterwards. Between these 2 operations, the database file lies unencrypted on the disk, and NOT in memory. Also, note that the database file is *unencrypted on creation*.
- **Editor** support:
  - A single bookmark can be edited before adding. The editor can be set using the environment variable *EDITOR* or by explicitly specifying the editor. The latter takes preference. If -a is used along with -w, the details are populated in the editor template.
  - In case of edit and update (a single bookmark), the existing record details are fetched from DB and populated in the editor template. The environment variable EDITOR must be set Note that -u works independently of -w.
  - All lines beginning with "#" will be stripped. Then line 1 will be treated as the URL, line 2 will be the title, line 3 will be comma separated tags, and the rest of the lines will be parsed as descriptions.
- **Proxy** support: environment variable *https_proxy*, if defined, is used to tunnel data for both http and https connections. The supported format is:

        http[s]://[username:password@]proxyhost:proxyport/

## GUI integration

![buku](http://i.imgur.com/8Y6PTPw.png)

`buku` can be integrated in a GUI environment with simple tweaks.

### Add bookmarks from anywhere

With support for piped input, it's possible to add bookmarks to `buku` using keyboard shortcuts on Linux and OS X. CLIPBOARD (plus PRIMARY on Linux) text selections can be added directly this way. The additional utility required is `xsel` (on Linux) or `pbpaste` (on OS X).

The following steps explore the procedure on Linux with Ubuntu as the reference platform.

1. To install `xsel` on Ubuntu, run:

        $ sudo apt install xsel
2. Create a new script `bukuadd` with the following content:

        #!/bin/bash

        xsel | buku -a

  `-a` is the option to add a bookmark.
3. Make the script executable:

        $ chmod +x bukuadd
4. Copy it somewhere in your `PATH`.
5. Add a new keyboard shortcut to run the script. I use `<Alt-b>`.

#### Test drive

Copy or select a URL with mouse and press the keyboard shortcut to add it to the `buku` database. The addition might take a few seconds to reflect depending on your internet speed and the time `buku` needs to fetch the title from the URL. To avoid title fetch from the web, add the `-t` option to the script.

To verify that the bookmark has indeed been added, run:

    $ buku -p | tail -3
and check the entry.

#### Tips

- To add the last visited URL in Firefox to `buku`, use the following script:

        #!/bin/bash

        sqlite3 $HOME/.mozilla/firefox/*.default/places.sqlite "select url from moz_places where last_visit_date=(select max(last_visit_date) from moz_places)" | buku -a
- If you want to tag these bookmarks, look them up later using:

        $ buku -S blank
Use option `-u` to tag these bookmarks.

### Import bookmarks to browser

`buku` can export (or import) bookmarks in HTML format recognized by Firefox, Google Chrome and Internet Explorer.

To export all bookmarks, run:

    $ buku --export path_to_bookmarks.html
To export specific tags, run:

    $ buku --export path_to_bookmarks.html --tag tag 1, tag 2
Once exported, import the html file in your browser.

## Sync database across systems

`buku` has the capability to import records from another `buku` database file. However, users with a cloud service client installed on multiple systems can keep the database synced across these systems automatically. To achieve this store the actual database file in a synced directory and create a symbolic link to it in the location where the database file would exist otherwise. For example, `$HOME/.local/share/buku/bookmarks.db` can be a symbolic link to `~/synced_dir/bookmarks.db`.

## As a library

`buku` can be used as a powerful bookmark management library. All functionality are available through carefully designed APIs. `main()` is a good usage example. It's also possible to use a custom database file in multi-user scenarios. Check out the documentation for the following APIs which accept an optional argument as database file:

    BukuDb.initdb(dbfile=None)
    BukuCrypt.encrypt_file(iterations, dbfile=None)
    BukuCrypt.decrypt_file(iterations, dbfile=None)
NOTE: This flexibility is not exposed in the program.

## Related projects

- [bukubrow](https://github.com/SamHH/bukubrow), WebExtension for browser integration
- [oil](https://github.com/AndreiUlmeyda/oil), search-as-you-type cli frontend
- [buku_run](https://github.com/carnager/buku_run), rofi frontend

## Mentions

- [One Thing Well](http://onethingwell.org/post/144952807044/buku)
- [It's F.O.S.S.](https://itsfoss.com/buku-command-line-bookmark-manager-linux/)
- [Make Tech Easier](https://www.maketecheasier.com/manage-browser-bookmarks-ubuntu-command-line/)
- [LinuxUser Magazine 01/2017 Issue](http://www.linux-community.de/LU/2017/01/Das-Beste-aus-zwei-Welten)

## Examples


1. **Edit and add** a bookmark from editor:

        $ buku -w
        $ buku -w 'macvim -f' -a https://ddg.gg search engine, privacy
The first command picks editor from the environment variable `EDITOR`. The second command will open macvim with option -f and the URL and tags populated in template.
2. **Add** a bookmark with **tags** `search engine` and `privacy`, **comment** `Search engine with perks`, **fetch page title** from the web:

        $ buku -a https://ddg.gg search engine, privacy -c Search engine with perks
        336. https://ddg.gg
        > DuckDuckGo
        + Alternative search engine with perks
        # privacy,search engine
where, >: title, +: comment, #: tags
3. **Add** a bookmark with tags `search engine` & `privacy` and **immutable custom title** `DDG`:

        $ buku -a https://ddg.gg search engine, privacy -t 'DDG' --immutable 1
        336. https://ddg.gg (L)
        > DDG
        # privacy,search engine
Note that URL must precede tags.
4. **Add** a bookmark **without a title** (works for update too):

        $ buku -a https://ddg.gg search engine, privacy -t
5. **Edit and update** a bookmark from editor:

        $ buku -w 15012014
This will open the existing bookmark's details in the editor for modifications. Environment variable `EDITOR` must be set.
6. **Update** existing bookmark at index 15012014 with new URL, tags and comments, fetch title from the web:

        $ buku -u 15012014 --url http://ddg.gg/ --tag web search, utilities -c Private search engine
7. **Fetch and update only title** for bookmark at 15012014:

        $ buku -u 15012014
8. **Update only comment** for bookmark at 15012014:

        $ buku -u 15012014 -c this is a new comment
Applies to --url, --title and --tag too.
9. **Export** bookmarks tagged `tag 1` or `tag 2` to HTML and markdown:

        $ buku -e bookmarks.html --tag tag 1, tag 2
        $ buku -e bookmarks.md --tag tag 1, tag 2
All bookmarks are exported if --tag is not specified.
10. **Import** bookmarks from HTML and markdown:

        $ buku -i bookmarks.html
        $ buku -i bookmarks.md
11. **Delete only comment** for bookmark at 15012014:

        $ buku -u 15012014 -c
Applies to --title and --tag too. URL cannot be deleted without deleting the bookmark.
12. **Update** or refresh **full DB** with page titles from the web:

        $ buku -u
        $ buku -u --tacit (show only failures and exceptions)
This operation does not modify the indexes, URLs, tags or comments. Only title is refreshed if fetched title is non-empty.
13. **Delete** bookmark at index 15012014:

        $ buku -d 15012014
        Index 15012020 moved to 15012014
The last index is moved to the deleted index to keep the DB compact.
14. **Delete all** bookmarks:

        $ buku -d
15. **Delete** a **range or list** of bookmarks:

        $ buku -d 100-200
        $ buku -d 100 15 200
16. **Search** bookmarks for **ANY** of the keywords `kernel` and `debugging` in URL, title or tags:

        $ buku kernel debugging
        $ buku -s kernel debugging
17. **Search** bookmarks with **ALL** the keywords `kernel` and `debugging` in URL, title or tags:

        $ buku -S kernel debugging
18. **Search** bookmarks **tagged** `general kernel concepts`:

        $ buku --stag general kernel concepts
19. List **all unique tags** alphabetically:

        $ buku --stag
20. Run a **search and update** the results:

        $ buku -s kernel debugging -u --tag + newtag
21. Run a **search and delete** the results:

        $ buku -s kernel debugging -d
22. **Encrypt or decrypt** DB with **custom number of iterations** (15) to generate key:

        $ buku -l 15
        $ buku -k 15
The same number of iterations must be specified for one lock & unlock instance. Default is 8, if omitted.
23. **Show details** of bookmarks at index 15012014 and ranges 20-30, 40-50:

        $ buku -p 20-30 15012014 40-50
24. **Show all** bookmarks with real index from database:

        $ buku -p
        $ buku -p | more
25. **Replace tag** 'old tag' with 'new tag':

        $ buku -r 'old tag' new tag
26. **Delete tag** 'old tag' from DB:

        $ buku -r 'old tag'
27. **Append (or delete) tags** 'tag 1', 'tag 2' to (or from) existing tags of bookmark at index 15012014:

        $ buku -u 15012014 --tag + tag 1, tag 2
        $ buku -u 15012014 --tag - tag 1, tag 2
28. **Open URL** at index 15012014 in browser:

        $ buku -o 15012014
29. List bookmarks with **no title or tags** for bookkeeping:

        $ buku -S blank
30. List bookmarks with **immutable title**:

        $ buku -S immutable
31. **Shorten URL** www.google.com and the URL at index 20:

        $ buku --shorten www.google.com
        $ buku --shorten 20
32. More **help**:

        $ buku -h
        $ man buku

## Contributions

Pull requests are welcome. Please visit [#103](https://github.com/jarun/Buku/issues/103) for a list of TODOs.
<br>
<p><a href="https://gitter.im/jarun/Buku"><img src="https://img.shields.io/gitter/room/jarun/buku.svg?maxAge=2592000" alt="gitter chat" /></a></p>

## Copyright

Copyright © 2015-2017 [Arun Prakash Jana](mailto:engineerarun@gmail.com)
