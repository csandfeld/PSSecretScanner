![PSSecurityScanner logo goes here](.\pssecurityscanner.png)

# PSSecretScanner

Super simple passwordscanner built using PowerShell.

Scan your code, files, folders, and repos for accidentily exposed secrets using PowerShell.

## Features

- Give a list of files to scan and we will check for any pattern matches in those files.

- Select output depending on how you want this to behave in f.ex a pipeline, console, or wrap it in a script to create your own handling.

- Use an excludelist to prevent false positives, or if you _really_ want to include secrets in your code, by creating a exclude file and passing it to the `-Excludelist` parameter.
The format of the excludelist _must_ be

```Text
<Full\path\to\file.txt>;<linenumber>;<Line>
Ex. 
C:\MyFiles\template.json;51;-----BEGIN RSA PRIVATE KEY-----
C:\MyRepo\MyModule.psm1:18:password = supersecret!!
```

## Installation

- From the PSGallery, run `Install-Module PSSecretScanner`

- Clone this repo, and run `Invoke-Build` to build the module localy.

## Background

I couldn't find a proper secret scanner for PowerShell so I wrote my own.

From the beginning it was just a list of regex patterns stolen from the [OWASP SEDATED security scanner repo](https://github.com/OWASP/SEDATED) that I ran through `Select-String`, as I thought the OWASP tools was way to advanced for my needs, and way to hard to wrap in a powershell script.
From there it kind of grew, and hopefully it will grow even more.

## About Regex patterns

- The baseline is the list found at the OWASP repo, but converted to PowerShell Regex standard (PCRE I think it's called..)
- Added `_Azure_AccountKey` pattern found at [Detect-secrets from YELP](https://github.com/Yelp/detect-secrets)
- Added patterns from [h33tlit](https://github.com/h33tlit/secret-regex-list#readme) (thank you [Simon Wåhlin](https://github.com/SimonWahlin/) for telling me)

_The added underscore `_` to names in the pattern list is simply to make them easier to work with in PowerShell._

## Features to add

Yes, even keeping it simple there are stuff I might want to add some day, or if you want to, feel free to create a PR.

- Parallelization - make it faster on huge repos.
- More filetypes! I kind of just winged it for now.
- ~~Steal and recreate [testcases from the OWASP page](https://github.com/OWASP/SEDATED/tree/master/testing/regex_testing) to make sure the regexes work as expected!~~ This is now converted to issue [#11](https://github.com/bjompen/PSSecretScanner/issues/11)

## Changelog

- 2022-07-26
  - Moved GetConfig to external helper function in order to make Find-Secret easier to write tests for.
- 2022-07-24
  - Added a pattern for personal access token
  - Added tests for regex patterns (some, not all of them)
  - Added unit tests for some functions (WIP)
  - Added the logo, becuse it's fun to draw stuff.
- 1.0.7, 2022-07-22
  - This is the first changelog entry so anything before this is without dates. ([#5](https://github.com/bjompen/PSSecretScanner/issues/5))
  - Refactored the module to a folder structure. ([#10](https://github.com/bjompen/PSSecretScanner/issues/10))
  - Added build script (Invoke-Build)
  - Changed from comment based to markdown documentation ([#7](https://github.com/bjompen/PSSecretScanner/issues/7))
- 1.0.6
  - Full support for PS 5.1 and later.
- 1.0.5
  - Some messing about with PS7 support
- 1.0.4
  - Added 24 patterns from the h33tlit list
- 1.0.(1..3)
  - Added and changed some patterns and added some functionality
- 1.0.0
  - First release. Basically a wrapper around `Select-String` with a .txt file of OWASP patterns.
