# asp-best-practices

# web api end point best practices
Always Use Nouns

When creating endpoints, always use nouns instead of verbs. See below the wrong way and the right way.
❌ Wrong:
/api/GetOffer/{offer}

✅ Correct:
/api/offers/{offer}

❌ Wrong:
/Api/Orders/AdditionalFields
✅ Correct:
/api/orders/additional_fields

👌 Good (singular):
/api/order/additional_field
🚀 Better (plural):
/api/orders/additional_fields

# pagination and other similar filtering options should be query
🚀 Better:
GET v1/products?limit=100&category=headphone

# asp.net webapi best practices
1. keep the ConfigureServices method clean and readable as much as possible. Of course, we need to write the code inside that method to register the services, but we can do that in a more readable and maintainable way by using the Extension methods.
2. keep the number of altributs to maximam of 3
3. Controller should always be clean and simple. Their responsibilities should only be handling HTTP requests and returning responses:
4. error should be handled globally.

~~~
app.UseExceptionHandler(config =>
{
     config.Run(async context =>
     {
         context.Response.StatusCode = <span class="enlighter-g1">(</span><span class="enlighter-k5">int</span><span class="enlighter-g1">)</span><span class="enlighter-text">HttpStatusCode.</span><span class="enlighter-m3">InternalServerError</span>;
         context.Response.ContentType = "application/json";

         var error = context.Features.Get<IExceptionHandlerFeature>();
         if (error != null)
         {
             var ex = error.Error;

             await context.Response.WriteAsync(new ErrorModel()
             {
                 StatusCode = <span class="enlighter-text">context.</span><span class="enlighter-m3">Response</span><span class="enlighter-text">.</span><span class="enlighter-m3">StatusCode</span>,
                 ErrorMessage = ex.Message 
             }.ToString()); //ToString() is overridden to Serialize object
         }
     });
 });
 ...
~~~
5. Filters in ASP.NET Core allow us to run some code prior to or after the specific stage in a request pipeline. Therefore, we can use them to execute validation actions that we need to repeat in our action methods. for example
 ~~~~
if (!ModelState.IsValid)
{
}
// instead of doing the above on every controller  


//We can create our filter
public class ModelValidationAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState); // returns 400 with error
        }
    }
}
builder.Services.AddScoped<ModelValidationAttribute>();
~~~~
    
# tools to research on more

* docfx - Tools for building and publishing API documentation for .NET projects http://dotnet.github.io/docfx
* Bogus - Simple and sane fake data generator for C#. Based on and ported from the famed faker.js.
* ArchUnitNET — A C# architecture test library to specify and assert architecture rules in C# for automated testing.
* code-cracker — An analyzer library for C# and VB that uses Roslyn to produce refactorings, code analysis, and other niceties.
* Roslynator — A collection of 190+ analyzers and 190+ refactorings for C#, powered by Roslyn.
* DevSkim - A set of IDE plugins and rules that provide security "linting" capabilities.
* AppMetrics - App Metrics is an open-source and cross-platform .NET library used to record and report metrics within an application and reports it's health.
* Telegram.Bot - C# Telegram Bot API library.
* <a href="https://github.com/ardalis/CleanArchitecture/tree/main/tests">CleanArchitecture</a> - A starting point for Clean Architecture with ASP.NET Core. Clean Architecture is just the latest in a series of names for the same loosely-coupled, dependency-inverted architecture. You will also find it  named hexagonal, ports-and-adapters, or onion architecture.
