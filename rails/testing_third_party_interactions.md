# Testing Third Party Interactions

Intracting with third-party services - a.k.a., an API - is a common
responsibility of web applications. As such, we need to have a clear
understanding of how to test these interactions. There are a [few different
ways](https://thoughtbot.com/blog/testing-third-party-interactions) to do so.

Importantly, regardless of testing method, we want to make sure to encapsulate
interactions with third-party APIs in a single object.

## Stubbing the Adapter
One way is to use [an
adapter](https://refactoring.guru/design-patterns/adapter/ruby/example).
First, we create a custom class that acts as the public interface for the
third-party API. Secondly, we create adapters that implement that interface. In
our tests, we simply stub the target interface.

For example, let's say we have the following target interface:

```ruby
class SmsClient
  cattr_accessor :adapter

  attr_reader :client

  def initialize
    @client = adapter.new
  end

  def send_message(args)
    @client.send_message(args)
  end
end
```

Then, we can write the following test:

```ruby
it '<test>' do
  allow(SmsClient).to receive(:send_message).and_return(...)
end
```


## Swapping out the Adapter

In this method of testing, instead of stubbing out the target interface, we use
an in-memory adapter. There are various ways to use the in-memory adapter, as
detailed [here](https://thoughtbot.com/blog/testing-sms-interactions) and
[here](https://thoughtbot.com/blog/faking-external-services-in-tests-with-adapters).
Then, in our test, we make assertions against our in-memory adapter:

```ruby
it '<test>' do
  expect(InMemoryAdapter.message.last).to eq(...)
end
```

## Stubbing the Network

Another way to test third-party interactions is to stub the network. The "Rails
way" of doing this is to use [webmock](https://rubygems.org/gems/webmock) to
intercept HTTP requests. This allow us to avoid making real HTTP request, and we
can test behavior that depends on how the third-party API responds to our
request. If we do go this route, we want to make sure that our [fake responses
mimic real-world responses](./webmock.md).

Our spec would then look like this:

```ruby
it '<test>' do
  stub_request(...).to_return(...)
end
```

## Swapping out the Server

I won't go into the details of this approach. It is simply too heavy-handed, and
I don't see any benefits of using this versus stubbing out the HTTP requests via
webmock. Please see the original article for further links on this methodology.

## Conclusion

While it is important to know that all these different means of testing
third-party interactions exists, there are some that we can usually dismiss to
make our lives easier.

First, the *swapping out the server* method (a.k.a. using a fake) requires a lot
of work and is usually overkill.

Secondly, the *stubbing the adapter* method should not be used because, if we
have an adapter in the first place, it is a small leap to use an in-memory
adapter which is specifically tailored to behave in a certain way in our testing
environment. We should always [prefer a mock object over
mock](http://blog.plataformatec.com.br/2015/10/mocks-and-explicit-contracts/).
Therefore, if we were thinking of *stubbing the adapter*, we might as well *swap
the adapter* instead.

That leaves us with a choice between *swapping the adapter* and *stubbing the
network*.

*Swapping the adapter* makes it easy to test that a method was called with a
particular set of arguments. This can be seen when, in our example, we used
`InMemoryAdapter.messages.last`, since now we can make assertions against the
properties of that message. However, it is difficult to change the behavior of
our adapter on a per-test basis. We'd have to resort to stubbing, and avoiding
stubbing is the reason why we dismissed *stubbing the adapter* for *swapping the
adapter* in the first place.

*Stubbing the network**, however, makes it easy to have different behavior per
test. We can stub the network request to return a 200, 400, etc. Furthermore, we
can assert against what HTTP message would have been sent to the server in
various but I prefer:

```ruby
stub = stub_request(...).with(body: ... headers: ..., query: ...)

expect(stub).to have_been_requested
```

For these reasons, the *stubbing the network* route seems to be the best way to
go.
