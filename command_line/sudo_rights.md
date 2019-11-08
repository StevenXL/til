# Sudo Rights

Sometimes, we need to know if a user has sudo rights in a system. If the user we
are curious about is the current user, we can try to run any command that
requires `sudo` and see what happens.

For a more elegant approach, we can use:

```bash
sudo -l
```

If we want to see the permissions for a user other than the current one, we can
use the `-U` option:

```bash
sudo -l -U otheruser
```
