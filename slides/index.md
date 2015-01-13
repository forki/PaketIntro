- title : Paket
- description : Introduction to Paket
- author : Steffen Forkmann
- theme : sky
- transition : default

***

### What is Paket?

- Dependency manager for .NET and Mono projects
- Allows to include NuGet packages and GitHub files

<br /><br />
<img style="border: none" src="images/logo.png" alt="Paket logo" /> 

***

### Why another package manager?

- .NET ecosystem has already NuGet
- Integrated in Visual Studio and Xamarin Studio
- [nuget.org](https://www.nuget.org/) is etablished package feed

<br /><br />
<img style="border: none" src="images/nuget.png" alt="Nuget logo" /> 

*** 

### Why another package manager?

- NuGet has no global view of your dependencies
- `packages.config` files are spread over all project folders
- As a sample [MassTransit](https://github.com/MassTransit/MassTransit):


<br /><br />
<img style="border: none" src="images/MassTransit.png" alt="packages.config everywhere" /> 

*** 

### Why another package manager?
    
- NuGet has no concept of indirect dependencies
- Which packages do we really need?


    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Accord" version="2.14.0" targetFramework="net45" />
      <package id="Accord.Math" version="2.14.0" targetFramework="net45" />
      <package id="Accord.Neuro" version="2.14.0" targetFramework="net45" />
      <package id="AForge" version="2.2.5" targetFramework="net45" />
      <package id="AForge.Genetic" version="2.2.5" targetFramework="net45" />
      <package id="AForge.Math" version="2.2.5" targetFramework="net45" />
      <package id="AForge.Neuro" version="2.2.5" targetFramework="net45" />
      <package id="Deedle" version="1.0.1" targetFramework="net45" />
      <package id="Deedle.RPlugin" version="1.0.1" targetFramework="net45" />
      <package id="FSharp.Charting" version="0.90.6" targetFramework="net45" />
      <package id="FSharp.Data" version="2.0.9" targetFramework="net45" />
      <package id="FsLab" version="0.0.19" targetFramework="net45" />
      <package id="MathNet.Numerics" version="3.0.0" targetFramework="net45" />
      <package id="MathNet.Numerics.FSharp" version="3.0.0" targetFramework="net45" />
      <package id="R.NET.Community" version="1.5.15" targetFramework="net45" />
      <package id="R.NET.Community.FSharp" version="0.1.8" targetFramework="net45" />
      <package id="RProvider" version="1.0.13" targetFramework="net45" />
    </packages>

*** 

### Why another package manager?

- NuGet puts the version no. in the path
- Updates requires manual work (at least if you use .fsx):


    #I "packages/Deedle.1.0.1/lib/net40"
    #I "packages/Deedle.RPlugin.1.0.1/lib/net40"
    #I "packages/FSharp.Charting.0.90.6/lib/net40"
    #I "packages/FSharp.Data.2.0.9/lib/net40"
    #I "packages/MathNet.Numerics.3.0.0/lib/net40"
    #I "packages/MathNet.Numerics.FSharp.3.0.0/lib/net40"
    #I "packages/RProvider.1.0.13/lib/net40"
    #I "packages/R.NET.Community.1.5.15/lib/net40"
    #I "packages/R.NET.Community.FSharp.0.1.8/lib/net40"

***

### Why another package manager?

- NuGet doesn't allow to reference plain source files
- If you want to reuse code you have to create a package


***

### Why don't you contribute to NuGet?

- NuGet is open source, but managed by Microsoft
- Most changes are breaking (e.g. version no. in path) 
- NuGet team made clear they won't accept these changes

***

### Paket - Project Principles

- Integrate well into the existing NuGet ecosystem
- Make things work with minimal tooling (plain text files)
- Automate everything
- Create a nice community

***

### Paket file structure

- `paket.dependencies`: Global definition of dependencies
- `paket.lock`: List of used versions for all dependencies
- `paket.references`: Dependency definition per project


<br /><br />
<img style="border: none" src="images/structure.png" alt="Basic structure" /> 

***

### paket.dependencies

- Specifies all direct dependencies
- Manually editable (or via paket.exe commands)


     source https://nuget.org/api/v2
           
     nuget Newtonsoft.Json         // any version
     nuget UnionArgParser >= 0.7   // x >= 0.7
     nuget log4net ~> 1.2          // 1.2 <= x < 2     
     nuget NUnit prerelease        // any version incl. prereleases
    

***

### paket.lock

- Graph of used versions for all dependencies
- Compareable to a unified view of all `packages.config`
- Automatically computed from `paket.dependencies`:


    NUGET
      remote: https://nuget.org/api/v2
      specs:
        log4net (1.2.10)
        Microsoft.Bcl (1.1.9)
          Microsoft.Bcl.Build (>= 1.0.14)
        Microsoft.Bcl.Async (1.0.168) - >= net40 < net45
          Microsoft.Bcl (>= 1.1.8)
        Microsoft.Bcl.Build (1.0.21)
        Newtonsoft.Json (6.0.8)
        NUnit (3.0.0-alpha-4)
          Microsoft.Bcl.Async (>= 1.0.165) - >= net40 < net45
        UnionArgParser (0.8.2)

***

### paket.references

- Specifies which dependencies are used in a given project
- Compareable to `packages.config`, but without versions
- Only direct dependencies need to  be listed
- Manually editable


    Newtonsoft.Json
    UnionArgParser
    NUnit

***

### Installing packages


    $ paket install

- Computes `paket.lock` based on `paket.dependencies`
- Restores all direct and indirect dependencies
- Processes all projects and adds references to the libraries

***

### Checking for updates


    $ paket outdated

- Lists all dependencies that have newer versions available:

<br /><br />
<img style="border: none" src="images/paket-outdated.png" alt="Paket outdated" /> 

***

### Updating packages


    $ paket update

- Recomputes `paket.lock` based on `paket.dependencies`
- Updates all versions to the latest matching all restrictions 
- Runs `paket install`

***

### Restoring packages


    $ paket restore

- Restores all direct and indirect dependencies
- Will not change `paket.lock` file
- Can be used for CI build or from inside Visual Studio

***

### Convert from NuGet


    $ paket convert-from-nuget

- Finds all `packages.config` files
  - Converts them to `paket.references` files
  - Generates `paket.dependencies` file
  - Computes `paket.lock` file
- Visual Studio package restore process will be converted
- Runs `paket install`

***

### Simplify dependencies


    $ paket simplify

- Computes indirect dependencies from `paket.lock` file  
  - Removes these from `paket.dependencies`
  - Removes these `paket.references`
- Especially useful after conversion from NuGet ([Sample](http://fsprojects.github.io/Paket/paket-simplify.html#Sample))

***

### Bootstrapping

- Don't commit `paket.exe` to your repository
- Bootstrapper is available for [download](https://github.com/fsprojects/Paket/releases/latest)
- Bootstrapper allows to download latest `paket.exe`
- Can be used for CI build or from inside Visual Studio

***

### Source code dependencies

- Allow to reference plain source code files
- Available for: 
  - [GitHub](https://www.github.com)
  - [GitHub gists](https://gist.github.com/)
  - HTTP resources
  
***

### Source code dependencies
#### GitHub sample (1)

- Add dependency to the `paket.dependencies` file 


    github forki/FsUnit FsUnit.fs
    
- Also add a file reference to a `paket.references` file 


    File:FsUnit.fs

***

### Source code dependencies 
#### GitHub sample (2)

- `paket install` will add a new section to `paket.lock`:


    GITHUB
      remote: forki/FsUnit
      specs:
        FsUnit.fs (7623fc13439f0e60bd05c1ed3b5f6dcb937fe468)

- `paket install` will also add a reference to the project:


<br /><br />
<img style="border: none" src="images/github_ref_default_link.png" alt="Source reference" /> 
       

***

### Getting help


    $ paket [command] --help

- Visit the [online documentation](http://fsprojects.github.io/Paket/)
- Create a [GitHub issue](https://github.com/fsprojects/Paket/issues)
- Follow [PaketManager](https://twitter.com/PaketManager) on Twitter

***

### Paket.VisualStudio

- Alpha version of [VisualStudio plugin](https://github.com/hmemcpy/Paket.VisualStudio)
- Takes contributions


<br /><br />
<img style="border: none" src="images/Paket.VisualStudio.png" alt="Paket.VisualStudio" /> 

***

### Thank you

- Slides are made using [FsReveal](http://fsprojects.github.io/FsReveal/)
- Send corrections to https://github.com/forki/PaketIntro