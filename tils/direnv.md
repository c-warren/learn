---
title: direnv
topics:
    - shell
    - direnv
    - zsh
references: 
    - groxx
    - https://blog.ffff.lt/posts/direnv-and-nvmrc/
    - https://direnv.net/
---

# direnv

direnv is an extremely light shell extension that allows you to:
    - update environment variables when entering a directory
    - execute scripts when entering a directory

It does this in a sufficiently lightweight way that it can be imperceptible running over the top of the shell.
It is also very portable - it can be used in basically any shell. 

## "Replacing" nvm 

This can be used to provide part of the behaviour of something like `nvm`, `jenv`, etc. 
Instead of running (somewhat) compute intensive operations to figure out which version to of node to load, direnv does nothing until you run the node executable itself. 
It does not replace sourcing the executables (so nvm still has a purpose), but with:

```
.envrc
export NODE_VERSION_PREFIX=v
export NODE_VERSIONS=~/.nvm/versions/node
use node 16
```

You no longer need nvm fully setup within your profile.

## .envrc

The .envrc powers direnv - whenever one exists, it will update the env vars/execute whatever is in there. 

```bash
#.envrc
PATH_add bin
```

[PATH_add](https://direnv.net/man/direnv-stdlib.1.html) will add `${pwd}/bin` to the PATH environment variable (within this directory) - any tools within the `bin` directory will then be accessible.
Combined with `zsh` and a script:

```zsh
#!/usr/bin/env zsh
// build my tool
```

And you have an auto-building tool accessible from the command line, whenever you're in the directory (and not outside it). Handy!


