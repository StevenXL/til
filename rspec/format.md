# Formatting RSpec Output

`RSpec` ships with several formatters that determine how output from a test
suite is displayed to a user. By default, `RSpec` uses the *progress* formatter,
which looks something like this:

```
....F.....*.....
```

However, we can specify a different formatter using the `--formatter` option.

```
rspec spec --format documentation
```

The ouput using the documentation formatter looks like this:

```
something
  does something that passes
  does something that fails (FAILED - 1)
  does something that is pending (PENDING: No reason given)
  does something that is skipped (PENDING: No reason given)
```

RSpec's
[documentation](https://relishapp.com/rspec/rspec-core/docs/command-line)
documents other command-line options that can be used to modify RSpec's default
behavior.
