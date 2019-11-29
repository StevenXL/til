# Form Objects

Form objects are plain old Ruby objects that act like a form. Form objects are
appropriate when:

1. A page's UI does *not* map to our data model.
2. We want to perform some side-effects around the form's lifecycle and don't
   want to pollute our ActiveRecord model or controller with that
   responsibility.
3. We need to perform different validations on the same ActiveRecord model under
   different contexts.

There are a few common patterns when creating a form object:

## ActiveModel

First, we want to make sure to [include
ActiveModel::Model](https://edgeapi.rubyonrails.org/classes/ActiveModel/Model.html).
This will ensure that we have mass attribute assignment, validations, and
conversion helpers available.

To make this work properly, we have to:

1. Make sure to have attribute accessors for all keys that we expect the params
   hash to have.
2. If we ever over-write `#initialize` (more on why we'd do that later), make
   sure to call `super(params)` first so that the attributes can be assigned.

## Validations

Because `ActiveModel::Model` includes
[ActiveModel::Validations](https://edgeapi.rubyonrails.org/classes/ActiveModel/Validations.html),
we can validate our attributes using the same API we are accustomed to.

```ruby
class SignUp
  include ActiveModel::Model

  attr_accessor :team_name, :user_name

  validate :team_name, presence: true
  validate :user_name, presence: true
end
```

However, we will often be creating objects with their own validations, and we'll
want to bubble up any errors on those objects to be first-class citizens of our
form object:

```ruby
class SignUp
  include ActiveModel::Model

  attr_accessor :team_name, :user_name

  validates :team_name, presence: true
  validates :user_name, presence: true
  validate :team_is_valid

  private

  def team
    @team ||= Team.new(name: team_name)
  end

  def team_is_valid
    if team.invalid?
      promote_errors(team.errors)
    end
  end

  def promote_errors(child_errors)
    child_errors.each do |attribute, message|
      errors.add(attribute, message)
    end
  end
end
```

## Children

In the code above, notice that any ActiveRecord models that we are initialized
are *memoized* and initialized from the data passed into the form. Do not make
the mistake of initializing the ActiveRecord models in the initialize method.

## Collections

A common use-case for form objects is to facilitate creating a collection of
ActiveRecord models. However, in order to do this correctly, there are a few
things we need to do. Collections, frankly, are simply different.

### The View

In order to pass a collection of attributes to the controller, our form UI will
need to render a collection of attributes as well. We do this by using the
[fields_for](https://www.theodinproject.com/courses/ruby-on-rails/lessons/advanced-forms)
view helper:


```ruby
<% f.object.teams.each do |team| %>
  <%= f.fields_for :teams, team do |team_form| %>
    ...
  <% end %>

<% end %>
```

In the above code, `f` is a form builder that is bound to our form object. We
are then going to render the fields for each team that our form object knows
about. We can replace `...` with the Rails DSL for building out a form, taking
care to use the `team_form` form builder.

### collections_attributes=

In order for Rails to pass in a collection of attributes, we need to add
a `<collection>s_attributes=` method to our form object. When Rails detects this
method, it will pass in a hash of the attributes in the params:

```ruby
class SignUp
  include ActiveModel::Model

  attr_accessor :user_name, :teams

  validates :user_name, presence: true
  validates :teams, length: { minimum: 1 }

  private

  def teams_attributes=(values)
    @teams = values.map do |_, params|
      Team.new(params)
    end
  end

  def promote_errors(child_errors)
    child_errors.each do |attribute, message|
      errors.add(attribute, message)
    end
  end
end
```

Notice that we are violating our previous declaration here to not initialize
ActiveRecord models in the initialization step of our form object. We are in
fact doing that through our `#teams_attributes=` method. (We've also made
`@teams` an accessor).

### def initialize(params)

In our view, we are are rendering the fields for each team that our form object
knows about. But what if our form does not know about any teams yet? Well, we
wouldn't render any team fields. To address that issue, we need to over-ride the
`initialize` method:

```ruby
...
def initialize(params)
  super(params)
  @team ||= [Team.new]
end
...
```

Notice that we took heed of the advice to always call `super(params)` if we
over-ride `initialize`. Because that method will call `teams_attributes=`, if
teh user provided any team attributes, we will initalize `Team` instances with
those attributes. Otherwise, we will initialize a single team so that our view
can render, at minium, the fields for a single team.

## References

1. [Form Objects Techniques and
   Patterns](https://medium.com/@jaryl/disciplined-rails-form-object-techniques-patterns-part-1-23cfffcaf429)
2. [Form Objects Annotated](https://forum.upcase.com/t/form-objects/2267)
3. [RailsConf 2017 Talk](https://youtu.be/g5mRPLxTh7k)
4. [Refactorign to Form Objects](https://youtu.be/9luYcnfZ7Ok)
