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

--- 

### Why another package manager?

- NuGet has no global view of your dependencies
- `packages.config` files are spread over all project folders
- As a sample [MassTransit](https://github.com/MassTransit/MassTransit):


<br /><br />
<img style="border: none" src="images/MassTransit.png" alt="packages.config everywhere" /> 

--- 

### Why another package manager?

- NuGet has no concept of indirect dependencies
- Which packages do we really need?



--- 

### Why another package manager?

- If versions are conflicting, NuGet will [silently take the latest version](http://fsprojects.github.io/Paket/controlling-nuget-resolution.html)



--- 

### Why another package manager?

- If versions are conflicting, 
    NuGet will [silently take latest](http://fsprojects.github.io/Paket/controlling-nuget-resolution.html)
- NuGet puts the version no. in the path => updates are a nightmare
- NuGet doesn't allow to reference GitHub files


***

### Why don't you contribute to NuGet?

- NuGet is open source, but managed by Microsoft
- Most changes are breaking 
- NuGet team made clear they won't accept these changes 

***

### paket.dependencies

- specifies rules regarding your application's dependencies:



   source https://nuget.org/api/v2
   
   nuget Castle.Windsor-log4net ~> 3.2
   nuget NUnit
    
---

### Strict references

    references strict
    source https://nuget.org/api/v2
    
    nuget Newtonsoft.Json ~> 6.0
    nuget UnionArgParser ~> 0.7

***

### paket.lock

    NUGET
      remote: https://nuget.org/api/v2
      specs:
        Castle.Core (3.3.1)
        Castle.Core-log4net (3.3.1)
          Castle.Core (>= 3.3.1)
          log4net (1.2.10)
        Castle.LoggingFacility (3.3.0)
          Castle.Core (>= 3.3.0)
          Castle.Windsor (>= 3.3.0)
        Castle.Windsor (3.3.0)
          Castle.Core (>= 3.3.0)
        Castle.Windsor-log4net (3.3.0)
          Castle.Core-log4net (>= 3.3.0)
          Castle.LoggingFacility (>= 3.3.0)
        log4net (1.2.10)
        NUnit (2.6.3)

***

### paket.references

    Newtonsoft.Json
    UnionArgParser
    DotNetZip
    RestSharp


***

#### Convert from NuGet

- Automatic converter available

    $ paket convert-from-nuget

- finds all packages.config files
  - converts them to paket.references files
  - generates a paket.dependencies file with all dependencies
  - generates paket.lock file
- package restore process will be converted
- installs all packages

***

- Slides are made with [FsReveal](http://fsprojects.github.io/FsReveal/)
- Send corrections to https://github.com/forki/PaketIntro