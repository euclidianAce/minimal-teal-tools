# Minimal Teal Tools

A set of tools for Teal, a typed dialect of Lua.

This repo is basically for platforms that don't have access to lfs, argparse, or any other dependency the official cli may gain in the future.
Ideally this is a testament to how nice the api `tl` provides that we can write these tools as minimal as they are.

## Current Tools

- compile: writes the compiled output of a single file to stdout.
- typecheck: type checks a single file.

Basically, the following are equivalent to the `tl` cli
```sh
compile foo.tl > foo.lua
# is equivalent to
tl gen foo.tl
```

```sh
typecheck foo.tl
# is equivalent to
tl check foo.tl
```

Each tool can only take one argument. Following the unix philosophy, compose the tools available to you to get the desired behavior.
```sh
tl check foo.tl bar.tl baz.tl
# in bash would be done as
for file in {foo.tl,bar.tl,baz.tl}; do typecheck $file; done
```
```sh
tl gen *.tl
# in bash would be done as
for file in *.tl; do compile $file > $(echo $file | cut -d'.' -f1).lua; done
```

## Making new tools

### Arbitrary restrictions are arbitrary.
- The current tools are pure lua with the only dependency being `tl`. This is probably the only hard requirement.
- Should be small. Sub ~50 lines? Sure, why not?
- Don't make sacrifices in readability to save less than 2 or 3 lines of code. short ~= simple
	- we could make everything 1 line of code if we wanted but that's no fun
