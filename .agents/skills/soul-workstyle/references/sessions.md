# Sessions on one machine

Several sessions often run at the same time on this machine. They share repositories, project folders, services and
the GPU, so one session can move a folder or stop a service while another session uses it. `~/.claude/SESSIONS.md`
lists the sessions that hold something that another session could break.

## When a row is needed

Add a row before you:

- move, rename or delete a folder outside your own worktree, scratchpad or `.tmp/<task>/`
- stop, restart or reconfigure a service, a server or a background process that you did not start
- start a long process that holds a shared resource, such as the GPU, a port or a large download

Reading files and changing files in your own worktree need no row.

## The row

The file is a table with the columns Session, Started, Goal, Holds, May restart and State. Holds lists the folders
that the session moves or keeps open, and State is `active` or `waiting for restart`.

1. Read the file. If the file does not exist, create it with the header row
2. If an `active` row of another session names the same folder or service, do not act. Tell the user which session
   holds it, and wait for the user's decision
3. Check the running processes and the open files for the same service or folder too, because a session can hold
   them without a row
4. Add your row, then act
5. When you finish, remove your row. If a restart or a move still waits, set the state to `waiting for restart`, write
   the exact command in the row, and put that command first in the report

A row older than one day belongs to a session that has ended. Remove it and say so in the report.

## Long steps

After a step that took several minutes, check that the paths and services that you use still exist before the next
step, because another session can move them in the meantime.
