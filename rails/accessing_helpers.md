# Accessing Helpers

Have you ever tried to access an  [ActionView Helper
Method](https://api.rubyonrails.org/v6.0.0/classes/ActionView/Helpers.html) from
the rails console, only to discover that the current context does not have the
method in scope?

```
pry(main)> pluralize(1, 'person')

NoMethodError: undefined method `pluralize' for main:Object
```

The reason that this is occuring is because ActionView Helper Methods are
declared in modules - in order to have access to those helper methods, we have
to include the module(s) that define the methods:

```
pry(main)> include ActionView::Helpers

pry(main)> pluralize(1, 'person')

=> "1 person"
```

However, there is an easier way!

We can simply call the
[helper](https://api.rubyonrails.org/classes/Rails/ConsoleMethods.html#method-i-helper),
which gives us access to all the helper methods available to controllers.

```
pry(main)> helper.pluralize(1, 'person')

=> "1 person"

```
