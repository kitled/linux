# `llm`





## `q`

https://entropicthoughts.com/q

`q` is an idea of a wrapper for `llm`.

Create a script, for instance called `q`.

`/usr/local/bin/q`

```sh
#!/bin/sh

llm -s "Answer in as few words as possible. Use a brief style with short replies." -m claude-3.5-sonnet "$*"
```

Longer prompt using a `heredoc`:

```sh
q <<'EOF'
> I have the following Perl code
>
>     @content[sort { $dists[$a] <=> $dists[$b] } 0..$#content];
>
> What does it do?
EOF
```

Supply file context with `cat` pipe.

```sh
cat LICENCE | q What licence is this?
```



Examples

```sh
git grep -n WeirdObject | q Where is WeirdObject defined?
```


```sh
q ’$ find bin -exec “perl -lpE ‘s/Two Wrongs/Entropic Thoughts/’ {}” find: missing argument to -exec. What is wrong? Thanks.’
```



