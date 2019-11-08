# Ignoring Files

`ag` is a command-line utility that allows us to search through a set of input
files for a specific pattern.

For example, we can search all files in the current directory for the pattern
`foo` like this:

```bash
ag foo
```

An interesting behavior of `ag` is that, **by default**, `ag` will ignore a file
if:

1. It is a binary file
2. It is a hidden file
3. The filename matches a pattern in `.gitignore`, `.hgignore`, or `.ignore`
4. The filename matches a pattern in `$HOME/.agignore`

We can use the `-a` flag to search all files (except hidden ones), or the `-u`
flag to search all files, including hidden ones.

More options on what `ag` should / should not ignore can be found in the man
pages, in the `IGNORING FILES` section.
