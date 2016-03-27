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
