# .NET REST API
https://github.com/PrimeAcademy/solinas-dotnet-api


Microsoft Tutorials:
https://docs.microsoft.com/en-us/learn/modules/build-web-api-aspnet-core/3-exercise-create-web-api

[Tutorial](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-8.0&preserve-view=true&tabs=visual-studio-code)

This is a solve of the assignment for the REST API assignment.
## Startup
`dotnet new webapi --use-controllers --no-https -o GlassesApi` -- make new boiler plate.
 Namespace will be GlassesApi. Project will have demo WeatherForecast in it.

Inside that project:
`dotnet new gitignore` -- make new .gitignore file for c# dotnet


### Using Postman / HTTPS
The Tutorial has https and asks students to use httprepl. Postman should work, but there may be http/https issues. Especially if you didn't use `--no-https` or havent set up the certs per the instructions. `no-https` should avoid all ssl issues.


## Model
First lets make a Model for our data.
`Models/Glasses.cd`

```csharp
//dotnet namespace can just be declared. Dotnotation for sub name spaces...
namespace GlassesLecture.Models;
public class Glasses {

    public int Id {get; set;}

    public string? Name {get; set;}

    public string? Color {get; set;}

    public string Shape {get; set;}

    // private int Id;

    //custom get and set
    // public void setId (int id) {
    //     Id = id;
    // }

    // public int getId () {
    //     return Id;
    // }

    //constructor
    //public Glasses() {...}


}
```

## REST Controller

-- Great time to talk about inheritance with `ControllerBase`

`Controllers/GlassesController.cs`

```csharp
using Microsoft.AspNetCore.Mvc; //Needed for annotations
using GlassesLecture.Models; // Needed for Model we made

namespace GlassesLecture.Controllers;

[ApiController]
[Route("/api/[controller]")]
public class GlassesController : ControllerBase {

    //In memory, like an array.
    private static List<Glasses> GlassesList = new List<Glasses>();

    // GET
    [HttpGet]
    public ActionResult<List<Glasses>> Get() {
        return GlassesList;
    } 


    // POST -- newGlasses comes from BODY! 
    [HttpPost]
    public IActionResult Post(Glasses newGlasses) {
        GlassesList.Add(newGlasses);

        return CreatedAtAction(nameof(Post), new {Id = newGlasses.Id}, newGlasses);
    }


    // PUT /api/Glasses/4
    // id comes from url params, glassesToChange Body
    [HttpPut("{id}")]
    public IActionResult Update(int id, Glasses glassesToChange ) {
        var itemToUpdate = GlassesList.Single(glasses => glasses.Id == id); 

       //Update the fields in that item

        return Ok();
    }

    // DELETE
    [HttpDelete("{id}")]
    public Delete(int id) {
        var itemToRemove = GlassesList.Single(glasses => glasses.Id == id);
        
        GlassesList.Remove(itemToRemove);
        return NoContent();
    }
}
```






