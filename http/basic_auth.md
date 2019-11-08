# Basic Auth

[HTTP Basic
Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) is an
*authentication scheme* wherein the HTTP client provides credentials in a
request header.

With HTTP Basic Authentication, the *username* and *password* is sent by the
client in a request header called `Authentication`. The value of this header is
the word `Basic` followed by a space and the **base64 encoding** of the
"username:password" string.

For example, if I am trying to authenticat to a web server as the user "steven"
with the password "foo", I would send an HTTP request message with the header
`Authentication` set to the value `Basic c3RldmVuOmZvbw==`.

`c3RldmVuOmZvbw==` is the base64 encoding of "steven:foo", which you can confirm
in yourself in any browser by running `bota("steven:foo")` in the JavaScript
console.
