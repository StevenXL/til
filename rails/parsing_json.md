# Parsing JSON

Ruby's standard library contains a [JSON
module](https://ruby-doc.org/stdlib-2.6.5/libdoc/json/rdoc/JSON.html) that can
be used to parse JSON, via the
[JSON.parse](https://ruby-doc.org/stdlib-2.6.5/libdoc/json/rdoc/JSON.html#method-i-parse)
function.

```ruby
source = '{"id": 1,"name": "A green door","price": 12.50,"tags": ["home", "green"]}'

JSON.parse(source) =>  {"id"=>1, "name"=>"A green door", "price"=>12.5, "tags"=>["home", "green"]}
```

[JSON.parse](https://ruby-doc.org/stdlib-2.6.5/libdoc/json/rdoc/JSON.html#method-i-parse)
takes an optional hash as a second argument, representing a set of options. The
`object_class` option is particularly interesting, because it allows us declare
what class the parsed JSON should be initialized with. The option is
particularly useful when used with the
[OpenStruct](https://ruby-doc.org/stdlib-2.5.1/libdoc/ostruct/rdoc/OpenStruct.html) class:

```ruby
source = '{"id": 1,"name": "A green door","price": 12.50,"tags": ["home", "green"]}'

parsed = JSON.parse(source) =>  {"id"=>1, "name"=>"A green door", "price"=>12.5, "tags"=>["home", "green"]}

parsed.id => 1
parsed.tags => ["home", "green"]
```

[JSON.parse](https://ruby-doc.org/stdlib-2.6.5/libdoc/json/rdoc/JSON.html#method-i-parse)
will even take care of initializing nested objects with the `object_class`
recursively:

```ruby
# Notice nested object in the top-level parent property
source = '{"name": "Steven", "age": 33, "parent": {"name": "Mom", "age": 70}}'

person = JSON.parse(source, object_class: OpenStruct)

person.parent.name => 'Mom'
```
