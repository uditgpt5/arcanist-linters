# Arcanist Linters

This is a collection of custom [Arcanist][] linters that we've written at
Pinterest.

## Linters

### Apache Thrift Linter

Lints for errors in [Apache Thrift](http://thrift.apache.org/) IDL (schema)
files using the `thrift` compiler.

```json
{
    "type": "thrift",
    "include": "(\\.thrift$)",
    "flags": [
        "--allow-64bit-consts"
    ],
    "version": ">= 0.9.0",
    "thrift.generators": [
        "py:dynamic,utf8strings,new_style,slots",
        "java",
        "go",
        "erl"
    ],
    "thrift.includes": [
        ".",
        "common"
    ]
}
```

### Go Vet Linter

Uses the [Go vet command](https://golang.org/cmd/vet/) to lint for suspicious
code constructs.

```json
{
    "type": "govet",
    "include": "(^src/example.com/.*\\.go$)"
}
```

### Python Debugger Linter

Hunts for stray Python debugger ([pdb][]) statements.

[pdb]: https://docs.python.org/2/library/pdb.html

```json
"pdb": {
    "type": "python-debugger",
    "include": "(\\.py$)"
}
```

### Python Imports Linter

Lints for illegal Python module imports.

```json
{
    "type": "python-imports",
    "python-imports.pattern": "(mock)",
    "include": "(\\.py$)",
    "exclude": "(^tests/)"
}
```

### Python Requirements Linter

Ensures Python package requirements in [requirements.txt files][req-txt] are
sorted and unique.

```json
{
    "type": "requirements-txt",
    "include": "(requirements.txt$)"
}
```

[req-txt]: https://pip.readthedocs.org/en/latest/user_guide/#requirements-files

## Installation

In short, you'll need to add this repository to your local machine and tell
Arcanist to load the extension. You either can do this globally or on a
per-project basis.

Once installed, the individual linters can be enabled and configured via the
project's `.arclint` file. See the [Arcanist Lint User Guide][lint-guide] for
details.

## Global Installation

Arcanist can load modules from an absolute path, but because it also searches
for modules one level up from itself on the filesystem, it's convenient to
clone this repository at the same level as `arcanist` and `libphutil`.

```
$ git clone https://github.com/pinterest/arcanist-linters.git pinterest-linters
$ ls
arcanist
pinterest-linters
libphutil
```

Then, tell Arcanist to load the module by editing `~/.arcconfig` (or
`/etc/arcconfig`):

```json
{
  "load": ["pinterest-linters"]
}
```

## Project Installation

You can also load `arcanist-linters` on a per-project basis. In that case,
using a [git submodule](https://git-scm.com/docs/git-submodule) is probably
the most convenient approach.

```
$ git submodule add https://github.com/pinterest/arcanist-linters.git .pinterest-linters
$ git submodule update --init
```

Then, enable the module in your project-level `.arcconfig` file:

```json
{
  "load": [".pinterest-linters"]
}
```

[Arcanist]: https://secure.phabricator.com/book/phabricator/article/arcanist/
[lint-guide]: https://secure.phabricator.com/book/phabricator/article/arcanist_lint/