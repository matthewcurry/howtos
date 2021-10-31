# DiffOrig

The `:DiffOrig` command is the only command in Vim's help system which is not
defined by Vim.  Instead, a handy definition is listed:

```
command DiffOrig vert new | set bt=nofile | r ++edit # | 0d_
                \e | diffthis | wincmd p | diffthis
```

The purpose of this command is to diff the current state of the buffer with
what is written to disk.  It is a bit opaque, so let us deconstruct it a bit.

## command

From `:help :command`, we can see that this defines a new command.

## vert

`:vert new` runs the `:new` command within a vertical split.

## set bt=nofile

`bt` is shorthand for `buftype`.  `nofile` makes the buffer a sort of "scratch"
buffer; it has no filename, and Vim will not warn you if you do not save this
buffer.

## r ++edit #

`:r` is shorthand for `:read`.  `++edit` is an option (see `:help ++opt`); for
example, this could be use to used to set the `fileformat` differently, or to
read as a binary file. `#` represents the alternate buffer register; see `:help
quote_#` for more information

## 0d_

This deletes the top blank line, an artifact from `:read`.

## diffthis

Sets the new buffer as one of the buffers to view in diff mode.

## windcmd p

Go to the last accessed window.  (Same as `CTRL-W p`.)

## diffthis

Sets the original bufferr as one to be diffed.
