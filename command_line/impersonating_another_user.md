# Impersonating Another User

On the command line, if we want to pretend to be another user, we can use the
`-u` option of the `sudo` command:

```
sudo -u <another user>
```

Now, all command that you run will be ran as that other user, instead of the
invoking user - i.e, yourself.
