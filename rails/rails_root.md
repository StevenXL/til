# Rails.root

`Rails.root` returns an instance of
[Pathname#join](https://ruby-doc.org/stdlib-2.6.3/libdoc/pathname/rdoc/Pathname.html)
that represents the root directory of our application. By taking advantage of
`Pathname#join`, we can easily reference any file or directory in our
application:

```ruby
Rails.root.join('spec', 'fixtures', 'docusign_responses', 'envelope.txt')
```

In fact, because `Pathname#join` returns another pathname object, we can
abstract the above so that we don't have to keep writing out the relative path
to get to our docusign responses:

```ruby
docusign_responses = Rails.root.join('spec', 'fixtures', 'docusign_responses')

envelope_txt = docusign_responses.join('envelope.txt')
```
