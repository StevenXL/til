# Loading Environmental Variables

I usually use [direnv](https://github.com/direnv/direnv) to manage environmental
variables automatically.

However, if you want something quick and dirty, you can create a file with the
following format:

```
export USERNAME=MyUserName
export PASSWORD=MyPassword
```

I usually name this file `.env`. I can then load it via:

```bash
source .env
```
