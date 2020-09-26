# User-Ids

One important concept is to identify tasks effortlessly with recognizable, consistent alphabetical ids.
To distinguish them clearly from the unique ids (uuid/ulid) and potential ids of the plumber, they will henceforth be called user-ids.

These ids are always lowercase.
Note that they only have to unique among open tasks. Closed tasks loose their userid and will have a new one generated should they be reactivated.

1. Generate an index of the task description: An array of all uppercase letters and initials.
2. If there is a parent task with a user-id, take its first letter as start of the id. Remove the first letter of the index if it is identical.
3. Find consecutive letters (preferably among the uppercase letters) in the index that yield a unique id with a length of 2-3.
4. Take up to 3 letters of the description and see if they create a unique id.
5. Otherwise, find a random combination of 2-3 characters that isn't used yet.

Example:

    Configure my ArchLinux properly

Index:

    CmALp

Generated ids, in order:

    ca
    al
    cal
    
    cm
    ma
    lp
    cma
    mal
    alp

    co
    con

If there is a parent with the id `c`:

    ca
    cl
    cal
    
    cm
    cp

    co
    con

Taskwarrior UDA: userid
