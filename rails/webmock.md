# Webmock

[webmock](https://github.com/bblimke/webmock) is a library for stubbing HTTP
responses in Ruby. The library allows us to declare what we want the *HTTP
response* to be for a given HTTP request.

webmock is used in testing environments to avoid making real HTTP requests.
While there are many ways to declare which HTTP requests to stub out, in
general, the syntax goes something like this:

```ruby
stub_request(:get, 'http://www.google.com').to_return(..)
```

The interesting part of the above is that the `to_return` method hs several
sigantures. We could pass in a hash, as in `.to_return(body: ..., headers: {..},
status: ...)`. Another option is that we can pass a string representing an HTTP
response message to the `to_return` method: `.to_return(<HTTP Message>)`.

I strongly suggest that we pass an HTTP response message to the `to_return`
method. Doing so allow us to [capture](../curl/include_and_silent.md) *real*
HTTP responses and make sure that we are testing against responses that will
actually be returned by the server.
