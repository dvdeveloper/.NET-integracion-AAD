# .NET-integracion-AAD
Ejemplo utilizando AUTH2

Configuración Webconfig
```
 <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="6e608be1-33d4-48eb-9e51-0a664ea89b4c" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
    <add key="ida:Domain" value="XXXXXXXX.onmicrosoft.com" />
    <add key="ida:TenantId" value="XXXX-7f46-41d8-XXXX-4a23c47a5427" />
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44346/" />
  </appSettings>
  ```
  
Controller -> Home
```
public ActionResult Index()
{
    //https://docs.microsoft.com/en-us/azure/active-directory/develop/tutorial-v2-asp-webapp
    var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;
    //You get the user's first and last name below:
    ViewBag.Name = userClaims?.FindFirst("name")?.Value;
    // The 'preferred_username' claim can be used for showing the username
    ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;
    // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
    ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
    // TenantId is the unique Tenant Id - which represents an organization in Azure AD
    ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;

    return View();
}
  ```
  
Listar variables entregadas por AAD - Views -> Home

```
<div class="jumbotron">
    <h1>ASP.NET</h1>
    <p class="lead">ASP.NET is a free web framework for building great Web sites and Web applications using HTML, CSS and JavaScript.</p>
    <p><a href="https://asp.net" class="btn btn-primary btn-lg">Learn more &raquo;</a></p>
</div>

@if (User.Identity.IsAuthenticated)
{
<div class="row">

    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
    </table>
    <br />
    <br />
</div>
}
  ```
