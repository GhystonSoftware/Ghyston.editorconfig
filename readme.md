# Ghyston.editorconfig

## Overview

The purpose of this package is to distribute a shared `.editorconfig` file, and have a single source of truth for which
rules we use on our projects. It is intended to be used alongside [CSharpier](https://csharpier.com/), so most of the
formatting rules have been disabled.

It's not possible for a `.editorconfig` file to be referenced/extended from a NuGet package, so we take the approach of
copying the `.editorconfig` file into the project at build time.

A lot of the rules come from [StyleCop.Analyzers](https://www.nuget.org/packages/StyleCop.Analyzers/), which needs to be installed separately.

You can view the default `.editorconfig` file [here](https://github.com/GhystonSoftware/Ghyston.editorconfig/blob/main/GhystonEditorconfig/.editorconfig).

## Installation

- Install this package from NuGet
  - Build the project, and you'll get an `.editorconfig` file in the project root (the same folder as your `.csproj`).
    Note that this will overwrite any existing `.editorconfig` file.
- Ignore the generated `.editorconfig` file from git, by adding `/*/.editorconfig` to your `.gitignore`
  (assuming you only want to ignore the `.editorconfig` files one level below the git root, which is usually where your
  `.csproj` files are)
- You also need to install the beta version of [Stylecop.Analyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) from NuGet
- Make sure your build pipeline runs the following two commands, to ensure that all rules are followed:
  - `dotnet format style --verify-no-changes`
  - `dotnet format analyzers --verify-no-changes`

## Overrides

If you need to make any edits to the generated `.editorconfig` file, you can create an `.editorconfig.overrides` file at
your project root, and the contents of this file will be appended to the generated `.editorconfig` file.

## Development

### Making changes
You can find the `.editorconfig` file in the `GhystonEditorconfig` folder.

### Testing
To test this package, you need to build it locally and install it in a project.
- Edit the version in `GhystonEditorconfig/GhystonEditorconfig.csproj` to something like
  `<Version>1.0.0-re-$([System.DateTime]::Now.ToString('yyyyMMddHHmm'))</Version>`
- Run `dotnet build --configuration Release`
- Run `dotnet pack`
  - This will create a`.nupkg` file at `GhystonEditorconfig\bin\Release`
- In your test project (where you're going to install the new version of this package)
  - Add the `GhystonEditorconfig\bin\Release` folder as a NuGet package source
  - Install the version of the package that you just built. Make sure to include prerelease packages in your NuGet search!

### Publishing
- Become a member of the [Ghyston organisation on NuGet](https://www.nuget.org/organization/Ghyston)
- Create an api key with Ghyston as the package owner: https://www.nuget.org/account/apikeys
- Run `dotnet nuget push GhystonEditorconfig\bin\Release\Ghyston.editorconfig.x.y.z.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json`
  - Make sure to put the correct version number in place of the `x.y.z`, and swap `YOUR_API_KEY` for your api key 
