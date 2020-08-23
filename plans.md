# Implementation

The implementation is split into two parts:

The [plumber][plumbing], wrapping low-level storage/database access to the database in verbose commands, and the porcelain, providing a pretty interface to it.

## Underlying implementation - [plumbing]

Provides commands such as:
- add <mods> -> add a new task
- <filter> modify <mods> -> modify task metadata
- prune -> delete tasks with status:deleted permanently
- <filter> get <prop> -> gets a property of the first task matching the filter

### Tasklite

To speed up development, I am considering [tasklite] under the hood for now rather than developing my own plumbing tool.

Additional relevant commands:
- runsql -> providing low-level access

### Taskwarrior

Since I am currently running [taskwarrior], this might also be a good foundation. It is also more tolerant about custom properties than Tasklite.

It has worse performance and some shortcomings, but these can mostly be addressed by the porcelain layer. A weak point however is that access isn't as powerful as with SQL, e.g. listing all subtasks of a parent can't be done as easily.

## Taskporc/Tofu

My own idea for a plumber, conceptually minimal without any convenient shortcuts or configuration - just basic commands to modify and read the database.

Maybe prefix internal values (ulid and state) with underscore. Does no interpretation of parameters, only storage.

## The task manager - [porcelain][plumbing]

Name ideas: Taskforc/Tofo

Basically a parser & visualizer with a few quirks.

Unknown commands are passed on to the plumber with potential parameter modifications.

Reports & parameter configurations should support some kind of inheritance, i.e. allowing to create a new property/report that initially uses all values from its ancestor.

Automatically generates short ids for new tasks based on description - first initials, then following letters in case of collisions (e.g. "al" for "Arch Linux setup"), length may depend on whether there is a parent (root tasks get single letter if possible) unless an id is explicitly set.

### Commands

Most commands are specified in a default config file, and even most standard commands are aliases:
- _delete_=modify status:delete
- _do_/_done_=modify status:done
- _log_=add status:done
- _addx_=add status:incompletable  
  name tbd; this is an important concept: such tasks cannot be completed, so they can denote categories (e.g. home, work) or activities (e.g. play piano, go outside) which can turn a task manager into a time manager

This allows the user to define and redefine the commands without any programming. It provides insights into existing inner workings and makes sure that internals are accessible for power use.

#### Special commands

These commands need to be hardcoded, they can't be specified as simple aliases:
- _<id> add_=add parent:id
- _cd <id>_=selects a specific task - all further commands will be executed on this task and its children rather than all tasks  
  Special cases:
  - `..` to select the parent
  - special values prefixed with underscore e.g. `_archive`, `_backlog`, `_waiting` -> if they can be auto-generated they can also replace [taskwarrior contexts](https://taskwarrior.org/docs/context.html)!

### Parsing

The parameters for commands are pre-parsed separately, which allows custom simplified syntax. A separate config file specifies the properties with allowed values and potential shorthands, e.g.
```
scheduled.name=Scheduled
scheduled.label=Sched.
scheduled.type=date
scheduled.shorthand=sch

size.name=Size
size.values=-,s,m,l
size.shorthand=s
```

- **name** is for display in detailed info
- **label** is used as column header for reports - default: _name_
- **type** is used to potentially parse and translate the value - default: text (e.g. a date type will transform "5:00" to add the current date or even translate it to a timestamp - maybe even use the `date` command here?)
- **values** can define allowed values
- **shorthand** can specify an alternative way to specify this property on the commandline, e.g. when adding a new task

### Reports

Adds support for reports, which can also be configured in a separate config file. They can specify queries, limits, sorting and which properties to display.

Especially important is the support for tree displaying -> using the parent relation, as well as incorporating datestamps of parents.

### Configuration
HOME=$XDG_CONFIG_HOME/tofo

$HOME/\*rc for general configuration - multiple files allow cleaner config
-> alternative: directory structure mimicking DOM for UNIX-style, but worse performance

$HOME/hooks contains hooks:
- on-start/on-exit
- on-change (any task change, add or mod)
- on-add (new task)
- on-mod (existing task changed)

$XDG_CACHE_HOME may contain the current state (context/entered task), since resetting that does no harm. Depending on the implementation, it could also hold a cache of the compiled rc.

# General plans

## Task attributes

A task has attributes, and each attribute is of one of the following types: 
- string, timestamp, number, date, list (of strings)
- it should be possible to restrict a property to a set of values (enum)

### Special attributes
- uid: string-computed (ulid/uuid)
- id: string (generated automatically, user-friendly but can be changed)
- parent: string
- entered, modified: timestamp
- urgency: number-computed
- status: enum-string-computed
- virtual tags?
- completable: boolean

### Standard attributes
- tags: list
- annotations: list
- wait,scheduled,due,until,recur: timestamp
- relativeRecur: boolean
- start,stop: timestamp

### Custom attributes
- priority, size: enum-string
- url: string

## Command ideas
- `add <mods>`: add a new task under the currently selected one
- `<id> add <mods>`: add a new task with <id> as parent

### Selected Task
If there is no selected task, the selected task is assumed to be an empty invisible root task
- `cd <id>`: select the given task (alternative names: open,select,ct("change task"))
- `show`: show details for the currently selected task (maybe also subtasks)
- `list`: list all direct subtasks
- `tree`: recursively list subtasks

### More commands

For more, see [taskwarrior] for now...

[tasklite]: https://tasklite.org
[plumbing]: https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain
[taskwarrior]: https://taskwarrior.org/  
