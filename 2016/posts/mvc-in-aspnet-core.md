### A quick write-up on my last demo
This week's Fixxup was all about IoT with Windows and MVC in ASP.NET Core. We had a blast!

In my demo about MVC in ASP.NET Core, there wasn't enough time to properly take you thru building a simple Web API from scratch, only using the bare essentials. In the demo I showed how easy it to quickly create a Greeter API with a `GET` and a `POST` to showcase Attribute Routing, basic model binding and configuring the HTTP request middleware.

Here's a quick write up on what I wanted to demo:

#### Requirements
As far as requirements are concerned, you would only need:

- an installation of the latest ASP.NET Runtime (head to http://get.asp.net to get started quickly on your platform of choice)
- your favorite text editor of choice. For this demo I used [Visual Studo Code](https://code.visualstudio.com), but you could literally use anything, even good o'l Notepad.

#### Project.json
Start creating a new JSON file called `project.json` and add the following dependencies:

- `Microsoft.AspNet.Server.Kestrel` the new blazing fast managed HTTP server
- `Microsoft.AspNet.Mvc.Core` the core bits of ASP.NET MVC
- `Microsoft.AspNet.Mvc.Formatters.Json` which is the JSON formatter

Furthermore, we need to configure a command to run when everything starts up. This can be *anything* and you can have more than one command. For the demo I've decided to go with a command named `api` that will simply fire up the Kestrel server. Lastly, we target the full .NET Framework (dnx46) as well as .NET Core:

```json
{
    "dependencies": {
        "Microsoft.AspNet.Server.Kestrel":"1.0.0-rc1-final",
        "Microsoft.AspNet.Mvc.Core":"6.0.0-rc1-final",
        "Microsoft.AspNet.Mvc.Formatters.Json":"6.0.0-rc1-final"
    },
    "commands": {
        "api":"Microsoft.AspNet.Server.Kestrel"
    },
    
    "frameworks": {
        "dnx46":{},
        "dnxcore50":{}
    }
}
```

#### Startup.cs
The next step is to configure our dependencies and wire up the request pipline middleware. We create a new class called `Startup.cs` and add the `void ConfigureServices(...)` and `void Configure(...)` methods to it, where we configure the dependency insjection for the needed services and use them respectively. 

```csharp
using Microsoft.AspNet.Builder;
using Microsoft.Extensions.DependencyInjection;

public class Startup
{
    public void ConfigureServices(IServiceCollection services){
        services
            .AddMvcCore()
            .AddJsonFormatters();
    }
    public void Configure(IApplicationBuilder app){
        app.UseMvc();
    }
}
```

You will notice that we have integrated support for DI from the start, making it possible to inject host-level dependencies into our program.

#### The Greet Controller
Now that all the plumbing is done, the last thing we need to do is implement a Controller to do some work. Here we have a basic controller called `GreetController` that has two endpoints: One `GET` action that will greet the given user name, and one `POST` that will create a greeting, accepting a `GreetingDTO` data transfer object.

Here we can see this class is a plain class, with no explicit dependencies on the base `Controller` class. MVC is clever enough to wire this up for us using convention.

```csharp
using Microsoft.AspNet.Mvc;

[Route("[controller]")]
public class GreetController
{
    [Route("{name}")]
    public string Get(string name){
        return $"Hello {name}";
    }
    
    [HttpPost]
    public IActionResult CreateGreeting([FromBody]GreetDTO greeting){
        return new CreatedAtActionResult("Get","Greet", new { name = greeting.Message }, "Greeting created ya'll!");
    }
}

public class GreetDTO
{
    public string Message { get; set; }
}
```

#### That's it!
Now all that is left to do is to take it for a spin. From Command Prompt, simply run `dnx api` and the server will start hosting on `http://localhost:5000`. 

The whole talk with the slides are available above this page.
