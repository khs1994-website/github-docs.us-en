---
title: pack ls
versions:
  fpt: '*'
  ghae: '*'
  ghec: '*'
  ghes: '*'
topics:
  - Advanced Security
  - Code scanning
  - CodeQL
type: reference
product: '{% data reusables.gated-features.codeql %}'
autogenerated: codeql-cli
intro: |-
  [Experimental] [Deep plumbing] List the CodeQL packages rooted at
  this directory. This directory must contain a qlpack.yml or
  .codeqlmanifest.json file.
---


<!-- Content after this section is automatically generated -->

{% data reusables.codeql-cli.man-pages-version-note %}

## Synopsis

```shell{:copy}
codeql pack ls <options>... -- <dir>
```

## Description

\[Experimental] \[Deep plumbing] List the CodeQL packages rooted at
this directory. This directory must contain a qlpack.yml or
.codeqlmanifest.json file.

## Primary options

#### `<dir>`

The root directory of the package or workspace, defaults to the current
working directory. If this parameter points to a directory containing a
qlpack.yml, then this operation will run on only that CodeQL package. If
this parameter points to a directory containing a codeql-workspace.yml,
then this operation will run on all CodeQL packages in the workspace.

### Options for configuring which CodeQL packs to apply this command to.

#### `--format=<fmt>`

Select output format, either `text` *(default)* or `json`.

#### `--groups=[-]<group>[,[-]<group>...]`

List of CodeQL pack groups to include or exclude from this operation. A
qlpack in the given workspace is included if:

*   It is in at least one of the groups listed without a minus sign (this
    condition is automatically satisfied if there are no groups listed
    without a minus sign), and
*   It is not in any group listed with a minus sign

### Common options

#### `-h, --help`

Show this help text.

#### `-J=<opt>`

\[Advanced] Give option to the JVM running the command.

(Beware that options containing spaces will not be handled correctly.)

#### `-v, --verbose`

Incrementally increase the number of progress messages printed.

#### `-q, --quiet`

Incrementally decrease the number of progress messages printed.

#### `--verbosity=<level>`

\[Advanced] Explicitly set the verbosity level to one of errors,
warnings, progress, progress+, progress++, progress+++. Overrides `-v`
and `-q`.

#### `--logdir=<dir>`

\[Advanced] Write detailed logs to one or more files in the given
directory, with generated names that include timestamps and the name of
the running subcommand.

(To write a log file with a name you have full control over, instead
give `--log-to-stderr` and redirect stderr as desired.)