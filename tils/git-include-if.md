---
title: git include-if
topics:
    - git
references: 
    - groxx
---

# git include-if

Sometimes it may be necessary to use different git configuration based on the repository (or some other attribute) you are working in. 
To do so, create a new file with the configuration you want for your repository.
e.g, to only add gpgsigned commits to the learn repository, create a new configuration file specifically for learn:
```
# .git/learn
[commit]
    gpgsign = true

[tag]
    gpgSign = true
```

Then add an includeIf rule to the end of the gitconfig file:
```
# .gitconfig
...
[includeIf "hasconfig:remote.*.url:git@github.com:c-warren/learn"]
    path = .git/learn
```
