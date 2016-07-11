<properties
	pageTitle="Azure Active Directory B2C preview | Microsoft Azure"
	description="How to build a web application that has sign-up, sign-in, and password reset using Azure Active Directory B2C."
	services="active-directory-b2c"
	documentationCenter=".net"
	authors="dstrockis"
	manager="msmbaldwin"
	editor=""/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/07/2016"
	ms.author="dastrock"/>

# Azure AD B2C preview: Sign-Up & Sign-In in a ASP.NET Web App

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features to your web app in a few short steps. This article will discuss how to create an ASP.NET web app that includes user sign-up, sign-in, and password reset. The app will include support for sign-up and sign-in by using a user name or email, and by using social accounts such as Facebook and Google.

This tutorial differs from [our other .NET web tutorial](active-directory-b2c-devquickstarts-web-dotnet.md) in that it uses a [sign up or sign in policy](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) to provide user registration & sign-in using a single button, instead of two (one for sign-up and one for sign-in).  In a nutshell, a sign up or sign in policy allows users to sign-in with an existing account if they have one, or create a new one if it is their first time using the app.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Get an Azure AD B2C directory

Before you can use Azure AD B2C, you must create a directory, or tenant. A directory is a container for all of your users, apps, groups, and more.  If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.

## Create an application

Next, you need to create an app in your B2C directory. This gives Azure AD information that it needs to securely communicate with your app. To create an app, follow [these instructions](active-directory-b2c-app-registration.md).  Be sure to:

- Include a **web app/web API** in the application.
- Enter `https://localhost:44316/` as a **Redirect URI**. It is the default URL for this code sample.
- Copy down the **Application ID** that is assigned to your app.  You will need it later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Create your policies

In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md). This code sample contains two identity experiences: **sign-up & sign-in**, and **password reset**.  You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md). When you create the two policies, be sure to:

- Choose **User ID sign-up** or **Email sign-up** in the identity providers blade.
- Choose the **Display name** and other sign-up attributes in your sign-up & sign-in policy.
- Choose the **Display name** claim as an application claim in every policy. You can choose other claims as well.
- Copy the **Name** of each policy after you create it. You'll need those policy names later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

After you create your two policies, you're ready to build your app.

## Download the code and configure authentication

The code for this sample [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). To build the sample as you go, you can [download the skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). You can also clone the skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

The completed sample is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) or on the `complete` branch of the same repository.

After you download the sample code, open the Visual Studio .sln file to get started.

Your app communicates with Azure AD B2C by sending HTTP authentication requests that specify the policy it wants to execute as part of the request. For .NET web applications, you can use Microsoft's OWIN library to send OpenID Connect authentication requests, execute policies, manage user sessions, and more.

To begin, add the OWIN middleware NuGet packages to the project by using the Visual Studio Package Manager Console.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Intsall-Package System.IdentityModel.Tokens.Jwt
```

Next, open the `web.config` file in the root of the project and enter your app's configuration values in the `<appSettings>` section, replacing the values below with your own.  You may leave the `ida:RedirectUri` and the `ida:AadInstance` values as is, unchanged.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Next, add an OWIN startup class to the project called `Startup.cs`. Right-click on the project, select **Add** and **New Item**, and then Search for "OWIN." Change the class declaration to `public partial class Startup`. We implemented part of this class for you in another file. The OWIN middleware will invoke the `Configuration(...)` method when your app starts. In this method, make a call to `ConfigureAuth(...)`, where you set up authentication for your app.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.  The parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app to communicate with Azure AD. You also need to set up cookie authentication. The OpenID Connect middleware uses cookies to maintain user sessions, among other things.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // The ACR claim is used to indicate which policy was executed
    public const string AcrClaimType = "http://schemas.microsoft.com/claims/authnclassreference";
    public const string PolicyKey = "b2cpolicy";
    public const string OIDCMetadataSuffix = "/.well-known/openid-configuration";

    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        OpenIdConnectAuthenticationOptions options = new OpenIdConnectAuthenticationOptions
        {
            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            { 
                AuthenticationFailed = AuthenticationFailed,
                RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // The PolicyConfigurationManager takes care of getting the correct Azure AD authentication
            // endpoints from the OpenID Connect metadata endpoint.  It is included in the PolicyAuthHelpers folder.
            ConfigurationManager = new PolicyConfigurationManager(
                String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, "/v2.0", OIDCMetadataSuffix),
                new string[] { SusiPolicyId, PasswordResetPolicyId }),

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {  
                NameClaimType = "name",
            },
        };

        app.UseOpenIdConnectAuthentication(options);
            
    }
    
...
```

## Send authentication requests to Azure AD
Your app is now properly configured to communicate with Azure AD B2C by using the OpenID Connect authentication protocol.  OWIN has taken care of all of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.  All that remains is to initiate each user's flow.

When a user selects **Login** or  **Forgot your password?** in the web app, the associated action is invoked in `Controllers\AccountController.cs`. In each case, you can use built-in OWIN methods to trigger the right policy:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by adding it to the AuthenticationProperties using the PolicyKey provided.

        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties (
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.SusiPolicyId}
                })
            { 
                RedirectUri = "/", 
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

public void ResetPassword()
{
    HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties(
            new Dictionary<string, string>
            {
                {Startup.PolicyKey, Startup.PasswordResetPolicyId}
            })
        {
            RedirectUri = "/",
        }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
}
```

You can also use a custom `PolicyAuthorize` tag in your controllers that requires the execution of a certain policy if the user is not signed in. Open `Controllers\HomeController.cs` and add the `[PolicyAuthorize]` tag to the claims controller. Replace the sample policy with your own sign up or sign-in policy.

```C#
// Controllers\HomeController.cs

// You can use the PolicyAuthorize decorator to execute a certain policy if the user is not already signed into the app.
[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
  ...
```

You can also use OWIN to sign out the user from the app. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request using the last policy that the user executed.
    // This is as easy as looking up the current value of the ACR claim, adding it to the AuthenticationProperties, and making an OWIN SignOut call.

    HttpContext.GetOwinContext().Authentication.SignOut(
        new AuthenticationProperties(
            new Dictionary<string, string> 
            { 
                {Startup.PolicyKey, ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value}
            }), OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

By default, OWIN will not send the policies you specify in `AuthenticationProperties` to Azure AD.  However, you can edit the requests that OWIN generates in the `RedirectToIdentityProvider` notification. Use this notification in `App_Start\Startup.Auth.cs` to fetch the right endpoint for each policy from the policy's metadata. This ensures that the correct request is sent to Azure AD for each policy that your app wants to execute.   

```C#
// App_Start\Startup.Auth.cs

private async Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    PolicyConfigurationManager mgr = notification.Options.ConfigurationManager as PolicyConfigurationManager;
    if (notification.ProtocolMessage.RequestType == OpenIdConnectRequestType.LogoutRequest)
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseRevoke.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.EndSessionEndpoint;
    }
    else
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseChallenge.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.AuthorizationEndpoint;
    }
}
```

## Display user information
When you authenticate users by using OpenID Connect, Azure AD returns an ID token to the app that contains **claims**. These are assertions about the user. You can use claims to personalize your app.  

Open the `Controllers\HomeController.cs` file. You can access user claims in your controllers via the `ClaimsPrincipal.Current` security principal object.

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
	Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
	ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

You can access any claim that your application receives in the same way.  A list of all the claims the app receives is available for you on the **Claims** page.

## Run the sample app

Finally, you can build and run your app. Sign up for the app by using an email address or user name. Sign out and sign back in as the same user. Edit that user's profile. Sign out and sign up as a different user. Note that the information displayed on the **Claims** tab corresponds to the information that you configured on your policies.

## Add social IDPs

Currently, the app supports only user sign-up and sign-in by using **local accounts**. These are accounts stored in your B2C directory that use a user name and password. By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.

To add social IDPs to your app, begin by following the detailed instructions in these articles. For each IDP you want to support, you need to register an application in that system and obtain a client ID.

- [Set up Facebook as an IDP](active-directory-b2c-setup-fb-app.md)
- [Set up Google as an IDP](active-directory-b2c-setup-goog-app.md)
- [Set up Amazon as an IDP](active-directory-b2c-setup-amzn-app.md)
- [Set up LinkedIn as an IDP](active-directory-b2c-setup-li-app.md)

After you add the identity providers to your B2C directory, you need to edit each of your three policies to include the new IDPs, as described in the [policy reference article](active-directory-b2c-reference-policies.md). After you save your policies, run the app again.  You should see the new IDPs added as sign-in and sign-up options in each of your identity experiences.

You can experiment with your policies and observe the effect on your sample app. Add or remove IDPs, manipulate application claims, or change sign-up attributes. Experiment until you can see how policies, authentication requests, and OWIN tie together.

For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). You can also clone it from GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
