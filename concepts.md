All task managers I have seen so far were quite opinionated. Since task management is a deeply personal matter, this likely yields many people to abandon these digital tools not long after embracing them.
To build trust in the system, it needs to be your system.

So the foundational principle of this task manager is to rely as little as possible on special mechanics, implementing all convenience behavior and specialized features in a generalized way, allowing to easily compose and customize them.

One only really needs key-value properties and tags (valueless properties) which can be assigned to tasks,
all other structures which humans might want can be derived from these in the frontend.

Then one can define views/lists/filters using key-value and tag queries:
```
[view]
	big=size:l
	break=size:s -pc
```
