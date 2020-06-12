Months ago, I started my quest to refine task management. I was using Todoist at the time and wasn't satisfied.
I barely trusted it anymore - it was lacking capabilities I wanted, such as a wait date as well as a difference between scheduled and due. Since it is a proprietary service, extending it would have been very challenging.
I was also growing more accustomed and comfortable with the terminal and the UNIX philosophy.

So I started exploring the CLI task managers and found [taskwarrior] - it seemed the perfect tool at first glance, with great customizability and control. But it too turned out to have some fundamental flaws.

# The concept of trust

I believe I first heard CGP Grey using this term, but I have not found it anywhere on the Internet with this meaning.

Trusting a digital tool, such as a task manager, means you rely on it. If you don't trust a tool, you will use it less and tend towards alternatives e.g. pen and paper.
To build trust with a task manager, you need to put all your tasks in it and set it up in a way that fits your workflow.

For that, a task management system needs to enable these three actions:
- quickly capture new information
- planning & reviewing
- retrieving tasks that should be done and can be done in the current context

# Design

The most important rule: Everything is a task. There is nothing else.  

Projects, Areas, Epics - they can all be mapped onto tasks, and doing so will allow you to leverage the same toolset on everything.
A project or epic is a completable task with subtasks - it can itself be a subtask.  
If you want to divide your task list into areas, these can simply be uncompletable tasks at the root level.

With everything being a task, areas and projects can also have all kinds of tags and attributes. And then subtasks may inherit these attributes (particularly tags).

More fundamentals:
- UNIX philosophy: use plain text is possible, separate into independent modules
- Complete control: Inbuilt reports and attributes should use available configuration, so that the user can change fundamental parts of the system

## Task attributes

A task has attributes, and each attribute is of one of the following types: 
- string, timestamp, number, date
- a list of each of these(or only string?) is possible as well
- it should be possible to restrict a property to a set of values (enum)

Special attributes:
- id: string
- parent: string
- entered, modified: timestamp
- urgency: number-computed
- status: enum-string-computed
- virtual tags?
- completable: boolean

Standard attributes:
- tags: list-string
- annotations: list-string
- wait,scheduled,due,until,recur: timestamp
- relativeRecur: boolean
- start,stop: timestamp

Custom attributes
- priority, size: enum-string
- url: string

## User Stories

- areas
- housework projects
- GTD
- Agile

## Reports I need

- Review active projects
- Find out tasks to batch when going out
- Find things I can do when I am (outside watching the babies e.g. cut nails | focused, wanting to do some (writing|programming) in the morning | unfocused in the afternoon e.g. check mails | taking a break from work on the computer e.g. do laundry | eating/snacking something e.g. watch a video/read a paper | listening to an audiobook e.g. digging, hang out the laundry)


# Commands


`add <desc> <mods>` - add a new task

`<id> add <desc> <mods>` - add a new task with <id> as parent

## Selected Task

If there is no selected task, the selected task is assumed to be an empty invisible root task

`cd <id>` - select the given task (alternative names: open,select,ct("change task"))

`show` - show details for the currently selected task (maybe also subtasks)

`list` - list all direct subtasks

`tree` - recursively list subtasks

## More commands

For more, see [taskwarrior] for now...

# Inspirations

## Taskwarrior

I have been using [taskwarrior] for a few weeks now, but I am already starting to lose trust again. I don't work on most of the tasks I've entered, and if I do, I rarely remember checking them off.

### Issues

- A big issue holding me back is a missing notion of subtasks. You either have to use projects, dependencies or create a complete custom hack - either a script or hook.
- Recurrence is a longstanding issue, but can somewhat be solved by plugins: https://github.com/tbabej/task.shift-recurrence and https://github.com/JensErat/task-relative-recur
- UDAs have a lot less options than inbuilt properties
- CLI can be too verbose: I am missing some way to set shorthands for attributes, e.g. "p" for project, dates are rather inflexible
- Keeping all reports aligned with custom attributes is a hassle, since you can't base reports off each other
- ids shift around annoyingly, so you constantly have to look them up

### What it does well

- Great customizability with reports, UDAs, DOM etc
- Great extendability with hooks
- Integrates with many tools (e.g. vimwiki, powerlevel10k, timewarrior)

# Links

taskwarrior: https://taskwarrior.org/
https://tasklite.org/related.html
https://www.wired.com/2016/03/best-to-do-list-app/

