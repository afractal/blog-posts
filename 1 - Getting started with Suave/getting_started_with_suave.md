### What is Suave?

The definition from [Suave.IO](https://suave.io/) website.

> Suave is a simple web development F\# library providing a lightweight web server and a set of combinators to manipulate route flow and task composition.

### Why Suave?

Why would a .NET developer want to use Suave instead using their beloved ASP.NET framework?

Firstly, Suave is a library not a framework. Unlike ASP.NET which is a full-blown framework with many layers of abstraction and many custom classes, Suave is lightweight and modular, making it easy to write just a thin application layer atop while working directly with the `HttpContext` class.

Secondly, Suave is designed with the benefits of functional programming in mind, using constructs in F\# like records, tuples, pattern matching, higher order functions and many more, you can write code with is more maintainable, easy to read, easy to change and easy to test. In ASP.NET you can write it with F\# but most people use C\#. Writing your code in C\# makes you bound to classes and methods, which are in general harder to maintain.

*Did you know the difference between methods and functions?*
Oh, there is a difference. Methods are bound to objects (classes or structs) and cannot exists without them, also they can be overloaded. Functions on the over hand cannot be overloaded (there is no such thing as function overloading) and can exists as a self contained unit.

Lastly, but more importantly, Suave is easy to get started and learn unlike ASP.NET which has a more steep learning curve.

### Project setup

After creating a folder for your project, you need to add the following files:

-   build.cmd
-   paket.dependencies
-   app.fsx


#### build.cmd

``` csharp
@echo off
cls

if not exist paket.lock (
  @echo "Installing dependencies"
  .paket\paket.exe install
) else (
  @echo "Restoring dependencies"
  .paket\paket.exe restore
)

@echo "Running web server"
packages\FAKE\tools\FAKE.exe %* --fsiargs app.fsx
```

#### paket.dependencies

``` csharp
source http://nuget.org/api/v2

nuget FAKE
nuget Suave
```

#### app.fsx

``` csharp
#r "packages/Suave/lib/net40/Suave.dll"
#r "packages/FAKE/tools/FakeLib.dll"

open Fake
open System
open System.IO
open Suave
open Suave.Http
open Suave.Web
open Suave.Successful

let app = OK "Hello world"

let serverConfig =
    let port = 5000
    { defaultConfig with
          homeFolder = Some __SOURCE_DIRECTORY__
          bindings = [ HttpBinding.createSimple HTTP "127.0.0.1" port ] }

Target "run" (fun _ ->
    startWebServer serverConfig app
)
RunTargetOrDefault "run"
```

Before running your application you must also download these files from the following link [paket files](https://github.com/fsprojects/Paket/releases) and place them in a folder named .paket in the root of the application.

-   paket.bootstrapper.exe
-   paket.exe
-   paket.targets

Project structure should be like like the following image.
![project structure](https://raw.githubusercontent.com/afractal/blog-posts/master/1%20-%20Getting%20started%20with%20Suave/assets/suave_start_project_structure.PNG)

Our project use [paket](https://fsprojects.github.io/Paket/) for installing the nuget packages specified in the paket.dependecies and [FAKE](https://github.com/fsharp/FAKE) for buildinding the project files.
In the build.cmd we specify that we want to install or restore the nuget packages and then we run FAKE.exe executable passing the file app.fsx. The app.fsx is a fsharp script file and it must reference all the dll with the \#r directive before opening them on the next line. In `let app = OK "Hello world"` we specify value named app with the OK webpart and pass "Hello world" as an argument. The OK webpart creates a HttpResponse with a 200OK status code and the response body set to the specified argument, in this case "Hello world".

In the next line we specify some server configuration which are, where we tell the server to run on the loopback ip 127.0.0.1 on port 5000.

To make the server start and listen we call the startWebServer function passing in the desired configuration and app value.

Now with all that done, the only thing to do is run the `build.cmd` executable to build and run it.

![](https://raw.githubusercontent.com/afractal/blog-posts/master/1%20-%20Getting%20started%20with%20Suave/assets/server_started.PNG)
