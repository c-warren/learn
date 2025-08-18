---
title: global gitignore file
topics:
    - git
references: 
    - # list of references - links, people, etc.
---

# Global Gitignore File

When you need to ignore a file/path/pattern across multiple repositories you can add a global gitignore file. 

To configure it, create a new file:
```
// .git/gitignore
.DS_Store
.vscode
```

And reference that file in your .gitconfig:
```
// .gitconfig
...
[core]
    excludesfile = /path/to/.git/gitignore
```

You can also use the cli to set it:
```bash
git config --global core.excludesfile /path/to/.git/gitignore
```

## Combining with includeIf

If you want to do this for specific repositories, rather than all repositories, add the same block to your repository specific file:

```
// .gitconfig
...
[includeIf hasconfig:remote.*.url:git@github.com:c-warren/*]
    path = .git/c-warren

// .git/c-warren
[core]
    excludesfile = /path/to/.git/c-warren_gitignore

// .git/c-warren_gitignore
*.js
```
