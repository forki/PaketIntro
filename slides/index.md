- title : Paket
- description : Introduction to Paket
- author : Steffen Forkmann
- theme : night
- transition : default

***

### What is Paket?

- Dependency manager for .NET and Mono projects
- Allows to include NuGet packages and GitHub files 

***

### paket.dependencies



    source https://nuget.org/api/v2
    
    nuget Castle.Windsor-log4net ~> 3.2
    nuget NUnit

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