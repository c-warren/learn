---
title: Checking vulnerabilities in golang
topics:
    - go
    - security
    - vulnerability_check
    - supply_chain_attack
references: 
    - https://words.filippo.io/dependabot/
---

# Checking vulnerabilities in golang

Vulnerability checkers are a great way to proactively upgrade packages when a security flaw has been detected (and publicised) in them. This can help prevent a wide variety of attacks on your system (especially supply chain attacks). 

Some vulnerability checkers (e.g dependabot) will raise requests for all packages that have been detected - regardless of whether the affected APIs are used by your application. This causes unnecessary upgrade thrash for developers:
* reviewing the change
* identifying whether it is relevant
* dealing with the repecussions of the change (e.g having to upgrade other associated packages, any breaking changes)

For golang the `govulncheck` package is able to statically analyse your application to determine if the vulnerabilities are relevant or not - reducing this toil. 

## govuln

```bash
go install golang.org/x/vuln/cmd/govulncheck@latest

# In your project directory
govulncheck ./...
```

This should output something like this:
```bash
Your code is affected by 25 vulnerabilities from the Go standard library.
This scan also found 5 vulnerabilities in packages you import and 8
vulnerabilities in modules you require, but your code doesn't appear to call
these vulnerabilities.
Use '-show verbose' for more details.
```

To see which packages were omitted, run with `-show verbose`:

```bash
govulncheck --show verbose ./...
```




