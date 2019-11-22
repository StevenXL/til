# --include and --silent

[curl](https://curl.haxx.se/docs/manpage.html) is a tool for transferring data
from a server to the local machine. `curl`'s surface area is very large, as it
supports multiple protocols, and has many options.

When fetching data via the HTTP protocol, `curl` will, by default, output the
body of the HTTP response message to standard out.

What if we wanted to see the entire HTTP response message? For that, we can use
a combination of the [-i](https://curl.haxx.se/docs/manpage.html#-i) and the
[-s](https://curl.haxx.se/docs/manpage.html#-s) options.

[-i](https://curl.haxx.se/docs/manpage.html#-i) stands for *--include*, which
will include the HTTP response message headers, including the HTTP response
line. 

[-s](https://curl.haxx.se/docs/manpage.html#-s) stands for *--silent*, which
will turn off progress meters and errors.

Thus, to save an entire HTTP response message to a file, we could run;

`curl -is http://www.google.com > http_message.txt`
