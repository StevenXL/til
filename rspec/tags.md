# Tags

In Rspec, we can tag *example groups* - i.e., `describe`s - and *examples* -
i.e., `it`'s.

```ruby
describe "group with tagged specs", :describe_tag do
  it "example I'm working now", :it_tag => :slow do
    # example code
  end

  it "example I'm working now", :it_tag => :fast do
    # example code
  end
end
```

In the above code, we have declared two tags - `:describe_tag` and `:it_tag`.
Because we have not declared a value or the `:describe_tag`, RSpec will use the
default value of `true`. The `:it_tag` has two values, `:slow` and `:fast`.

Once we have tagged an *example group* / *example*, we can use the tag or tag /
value combo to filter our examples:

```
# will only run examples with the <tag> tag.
be rspec -t <tag> 

# will only run examples with that tag / value combo
be rspec -t <tag>:<value>

# will exclude examples with the tag <tag>
be rspec -t ~<tag>
```

We can also use tags to perform some configuration based on the tags:

```ruby
RSpec.configure do |config|
  config.include ProductHelper, type: :feature
end
```
