# Ghyston.editorconfig

## Overview

The purpose of this package is to distribute a shared `.editorconfig` file, and have a single source of truth for which
rules we use on our projects. It is intended to be used alongside [CSharpier](https://csharpier.com/), so most of the
formatting rules have been disabled.

It's not possible for a `.editorconfig` file to be referenced/extended from a NuGet package, so we take the approach of
copying the `.editorconfig` file into the project at build time.

A lot of the rules come from [StyleCop.Analyzers](https://www.nuget.org/packages/StyleCop.Analyzers/), which is a
dependency of this package. There is no need to install it separately.

## Installation

- Install this package from NuGet, using our private source
  - Build the project, and you'll get an `.editorconfig` file in the project root (the same folder as your `.csproj`).
    Note that this will overwrite any existing `.editorconfig` file.
- Ignore the generated `.editorconfig` file from git, by adding `/*/.editorconfig` to your `.gitignore`
  (assuming you only want to ignore the `.editorconfig` files one level below the git root, which is usually where your
  `.csproj` files are)
- Make sure your build pipeline runs the following two commands, to ensure that all rules are followed:
  - `dotnet format style --verify-no-changes`
  - `dotnet format analyzers --verify-no-changes`

## Overrides

If you need to make any edits to the generated `.editorconfig` file, you can create an `.editorconfig.overrides` file at
your project root, and the contents of this file will be appended to the generated `.editorconfig` file.
