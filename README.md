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

## Task types
There are essentially 4 types of things we do:
- tasks: things to be done once e.g. hand in an assignment, fix a bug
- activities: can't be completed e.g. browse the internet, gaming, spend time with family
- habits: repeat in a set interval, e.g. go for a run daily, pay rent - sometimes skippable
- chores: recur in regular intervals without a strict due date, e.g. do laundry, cut nails - can be postponed but not skipped

Even though they won't be clearly distinguished by a single property, they will be mapped through some default properties:
- Inspired by [tasklite](https://tasklite.org/concepts.html), habits have a `repeat` property while chores have a `recur` property - both can be frozen.
- Tasks and activities may be distinguished by a `size` property, where activities have a special size value of `-`, marking them incompletable. Alternatively, activities may be prefixed with a star as it is done in Todoist

These basic types also incorporate other types:
- an area is simply an activity with subtasks
- a project is a task with subtasks

Since "task" is one of these types, entities of any of these types can be called "items" within the implementation to avoid confusion.

## User Stories
- areas
- housework projects
- GTD
- Agile

### Reports I need
- Review active projects
- Find out tasks to batch when going out
- Find things I can do when I am (outside watching the babies e.g. cut nails | focused, wanting to do some (writing|programming) in the morning | unfocused in the afternoon e.g. check mails | taking a break from work on the computer e.g. do laundry | eating/snacking something e.g. watch a video/read a paper | listening to an audiobook e.g. digging, hang out the laundry)

# Inspirations

## Taskwarrior

I have been using [taskwarrior] for a few weeks now, but I am already starting to lose trust again. I don't work on most of the tasks I've entered, and if I do, I rarely remember checking them off.

### Issues

- A big issue holding me back is a missing notion of subtasks. You either have to use projects, dependencies or create a complete custom hack - either a script or hook. This also makes entering tasks more verbose as I have to specify multiple tags repeatedly which could otherwise simply be inherited.
- Recurrence is a longstanding issue, but can somewhat be solved by plugins: https://github.com/tbabej/task.shift-recurrence and https://github.com/JensErat/task-relative-recur
- UDAs have a lot less options than inbuilt properties
- CLI can be too verbose: I am missing some way to set shorthands for attributes, e.g. "p" for project, dates are rather inflexible
- Keeping all reports aligned with custom attributes is a hassle, since you can't base reports off each other
- ids change whenever a task is completed, so you constantly have double-check or might complete a wrong task

### What it does well

- Great customizability with reports, UDAs, DOM etc
- Great extendability with hooks
- Integrates with many tools (e.g. vimwiki, powerlevel10k, timewarrior)

# Links

[taskwarrior]: https://taskwarrior.org/  
- https://tasklite.org/related.html: List of CLI-oriented productivity systems  
  Should have a look at:
  - org mode
  - taskell
- https://www.wired.com/2016/03/best-to-do-list-app/: Why technology 
