---
title: Hôte web ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ced2a766359894b9b83164c12a3ab69aa13c93a0
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="3ad28-103">Hôte web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ad28-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="3ad28-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3ad28-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3ad28-105">Les applications ASP.NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="3ad28-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="3ad28-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="3ad28-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3ad28-107">Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="3ad28-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="3ad28-108">Cette rubrique traite de l’hôte web ASP.NET Core ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), qui est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="3ad28-108">This topic covers the ASP.NET Core Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="3ad28-109">Pour découvrir l’hôte générique .NET ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="3ad28-109">For coverage of the .NET Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3ad28-110">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="3ad28-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3ad28-112">Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3ad28-112">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="3ad28-113">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="3ad28-114">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="3ad28-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="3ad28-115">Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :</span><span class="sxs-lookup"><span data-stu-id="3ad28-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="3ad28-116">`CreateDefaultBuilder` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="3ad28-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="3ad28-117">Configure [Kestrel](xref:fundamentals/servers/kestrel) en tant que serveur web.</span><span class="sxs-lookup"><span data-stu-id="3ad28-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server.</span></span> <span data-ttu-id="3ad28-118">Pour connaître les options Kestrel par défaut, consultez la [section sur les options Kestrel dans Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="3ad28-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="3ad28-119">Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="3ad28-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="3ad28-120">Charge la configuration facultative à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3ad28-120">Loads optional configuration from:</span></span>
  * <span data-ttu-id="3ad28-121">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="3ad28-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="3ad28-122">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="3ad28-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="3ad28-123">Les [secrets utilisateur](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development`</span><span class="sxs-lookup"><span data-stu-id="3ad28-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="3ad28-124">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="3ad28-124">Environment variables.</span></span>
  * <span data-ttu-id="3ad28-125">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="3ad28-125">Command-line arguments.</span></span>
* <span data-ttu-id="3ad28-126">Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage.</span><span class="sxs-lookup"><span data-stu-id="3ad28-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="3ad28-127">La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="3ad28-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="3ad28-128">Active [l’intégration IIS](xref:host-and-deploy/iis/index) quand il est exécuté derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="3ad28-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="3ad28-129">Configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3ad28-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="3ad28-130">Le module crée un proxy inverse entre IIS et Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ad28-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="3ad28-131">Il configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="3ad28-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="3ad28-132">Pour connaître les options IIS par défaut, consultez la [section sur les options IIS dans Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="3ad28-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="3ad28-133">Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="3ad28-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="3ad28-134">Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="3ad28-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="3ad28-135">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="3ad28-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3ad28-136">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="3ad28-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="3ad28-137">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="3ad28-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3ad28-138">Pour plus d’informations sur la configuration d’une application, consultez [Configuration dans ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3ad28-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="3ad28-139">Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="3ad28-140">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3ad28-142">Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3ad28-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="3ad28-143">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="3ad28-144">Dans les modèles de projet, `Main` se trouve dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="3ad28-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="3ad28-145">`WebHostBuilder` nécessite un serveur [ qui implémente IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3ad28-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3ad28-146">Les serveurs intégrés sont [Kestrel](xref:fundamentals/servers/kestrel) et [HTTP.sys](xref:fundamentals/servers/httpsys) (dans les versions antérieures à ASP.NET Core 2.0, HTTP.sys était appelé [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="3ad28-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="3ad28-147">Dans cet exemple, la [méthode d’extension UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) spécifie le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ad28-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="3ad28-148">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="3ad28-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3ad28-149">La racine de contenu par défaut pour `UseContentRoot` est retournée par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="3ad28-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="3ad28-150">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="3ad28-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="3ad28-151">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="3ad28-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3ad28-152">Pour utiliser IIS comme proxy inverse, appelez [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) dans le cadre de la création de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="3ad28-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="3ad28-153">`UseIISIntegration` ne configure pas un *serveur*, contrairement à ce que fait [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="3ad28-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="3ad28-154">`UseIISIntegration` configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour créer un proxy inverse entre Kestrel et IIS.</span><span class="sxs-lookup"><span data-stu-id="3ad28-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="3ad28-155">Pour utiliser IIS avec ASP.NET Core, `UseKestrel` et `UseIISIntegration` doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="3ad28-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="3ad28-156">`UseIISIntegration` est activé uniquement dans le cadre d’une exécution derrière IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3ad28-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="3ad28-157">Pour plus d’informations, consultez [Présentation du module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3ad28-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="3ad28-158">Une implémentation minimale qui configure un hôte (et une application ASP.NET Core) consiste à spécifier un serveur et la configuration du pipeline des requêtes de l’application :</span><span class="sxs-lookup"><span data-stu-id="3ad28-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="3ad28-159">Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="3ad28-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="3ad28-160">Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="3ad28-161">Pour plus d’informations, consultez [Démarrage de l’application dans ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="3ad28-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="3ad28-162">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="3ad28-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="3ad28-163">Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="3ad28-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="3ad28-164">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="3ad28-164">Host configuration values</span></span>

<span data-ttu-id="3ad28-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="3ad28-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="3ad28-166">Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="3ad28-167">Par exemple, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="3ad28-168">Méthodes explicites, telles que [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="3ad28-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="3ad28-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="3ad28-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="3ad28-170">Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.</span><span class="sxs-lookup"><span data-stu-id="3ad28-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="3ad28-171">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="3ad28-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="3ad28-172">Pour plus d’informations, consultez [Remplacer une configuration](#override-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="3ad28-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="3ad28-173">Capture des erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="3ad28-173">Capture Startup Errors</span></span>

<span data-ttu-id="3ad28-174">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="3ad28-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="3ad28-175">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="3ad28-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="3ad28-176">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="3ad28-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3ad28-177">**Valeur par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="3ad28-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="3ad28-178">**Définition avec** : `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="3ad28-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="3ad28-179">**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="3ad28-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="3ad28-180">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="3ad28-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="3ad28-181">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="3ad28-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="3ad28-184">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="3ad28-184">Content Root</span></span>

<span data-ttu-id="3ad28-185">Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="3ad28-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="3ad28-186">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="3ad28-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="3ad28-187">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-187">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-188">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3ad28-189">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3ad28-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3ad28-190">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="3ad28-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="3ad28-191">La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="3ad28-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="3ad28-192">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="3ad28-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="3ad28-195">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="3ad28-195">Detailed Errors</span></span>

<span data-ttu-id="3ad28-196">Détermine si les erreurs détaillées doivent être capturées.</span><span class="sxs-lookup"><span data-stu-id="3ad28-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="3ad28-197">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="3ad28-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="3ad28-198">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="3ad28-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3ad28-199">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="3ad28-199">**Default**: false</span></span>  
<span data-ttu-id="3ad28-200">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3ad28-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3ad28-201">**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="3ad28-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="3ad28-202">Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="3ad28-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="3ad28-205">Environnement</span><span class="sxs-lookup"><span data-stu-id="3ad28-205">Environment</span></span>

<span data-ttu-id="3ad28-206">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-206">Sets the app's environment.</span></span>

<span data-ttu-id="3ad28-207">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="3ad28-207">**Key**: environment</span></span>  
<span data-ttu-id="3ad28-208">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-208">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-209">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="3ad28-209">**Default**: Production</span></span>  
<span data-ttu-id="3ad28-210">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3ad28-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3ad28-211">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="3ad28-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="3ad28-212">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="3ad28-212">The environment can be set to any value.</span></span> <span data-ttu-id="3ad28-213">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3ad28-214">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="3ad28-214">Values aren't case sensitive.</span></span> <span data-ttu-id="3ad28-215">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="3ad28-216">Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3ad28-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="3ad28-217">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3ad28-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="3ad28-220">Assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="3ad28-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="3ad28-221">Définit les assemblys d’hébergement au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="3ad28-222">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="3ad28-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="3ad28-223">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-223">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-224">**Valeur par défaut** : une chaîne vide</span><span class="sxs-lookup"><span data-stu-id="3ad28-224">**Default**: Empty string</span></span>  
<span data-ttu-id="3ad28-225">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3ad28-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3ad28-226">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="3ad28-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="3ad28-227">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="3ad28-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="3ad28-228">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="3ad28-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="3ad28-229">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="3ad28-230">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="3ad28-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-233">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="3ad28-234">URL d’hébergement préférées</span><span class="sxs-lookup"><span data-stu-id="3ad28-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="3ad28-235">Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="3ad28-236">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="3ad28-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="3ad28-237">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="3ad28-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3ad28-238">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="3ad28-238">**Default**: true</span></span>  
<span data-ttu-id="3ad28-239">**Définition avec** : `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="3ad28-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="3ad28-240">**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="3ad28-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="3ad28-241">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="3ad28-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-244">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="3ad28-245">Bloquer les assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="3ad28-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="3ad28-246">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="3ad28-247">Pour plus d’informations, consultez [Améliorer une application à partir d’un assembly externe avec IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3ad28-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="3ad28-248">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="3ad28-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="3ad28-249">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="3ad28-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3ad28-250">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="3ad28-250">**Default**: false</span></span>  
<span data-ttu-id="3ad28-251">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3ad28-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3ad28-252">**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="3ad28-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="3ad28-253">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="3ad28-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-256">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="3ad28-257">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="3ad28-257">Server URLs</span></span>

<span data-ttu-id="3ad28-258">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="3ad28-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="3ad28-259">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="3ad28-259">**Key**: urls</span></span>  
<span data-ttu-id="3ad28-260">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-260">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-261">**Par défaut** : http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="3ad28-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="3ad28-262">**Définition avec** : `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="3ad28-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="3ad28-263">**Variable d’environnement** : `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3ad28-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="3ad28-264">Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="3ad28-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="3ad28-265">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="3ad28-266">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="3ad28-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="3ad28-267">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="3ad28-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="3ad28-268">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="3ad28-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="3ad28-270">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="3ad28-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="3ad28-271">Pour plus d’informations, consultez [Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3ad28-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="3ad28-273">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="3ad28-273">Shutdown Timeout</span></span>

<span data-ttu-id="3ad28-274">Spécifie la durée d’attente avant l’arrêt de l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="3ad28-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="3ad28-275">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="3ad28-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="3ad28-276">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="3ad28-276">**Type**: *int*</span></span>  
<span data-ttu-id="3ad28-277">**Valeur par défaut** : 5</span><span class="sxs-lookup"><span data-stu-id="3ad28-277">**Default**: 5</span></span>  
<span data-ttu-id="3ad28-278">**Définition avec** : `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="3ad28-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="3ad28-279">**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="3ad28-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="3ad28-280">La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="3ad28-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="3ad28-281">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="3ad28-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="3ad28-282">Pendant la période du délai d’attente, l’hébergement :</span><span class="sxs-lookup"><span data-stu-id="3ad28-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="3ad28-283">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="3ad28-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="3ad28-284">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="3ad28-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="3ad28-285">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="3ad28-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="3ad28-286">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="3ad28-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="3ad28-287">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="3ad28-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-290">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ad28-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="3ad28-291">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="3ad28-291">Startup Assembly</span></span>

<span data-ttu-id="3ad28-292">Détermine l’assembly à rechercher pour la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="3ad28-293">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="3ad28-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="3ad28-294">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-294">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-295">**Valeur par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="3ad28-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="3ad28-296">**Définition avec** : `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="3ad28-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="3ad28-297">**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="3ad28-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="3ad28-298">L’assembly peut être référencé par nom (`string`) ou type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="3ad28-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="3ad28-299">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="3ad28-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="3ad28-302">Racine Web</span><span class="sxs-lookup"><span data-stu-id="3ad28-302">Web Root</span></span>

<span data-ttu-id="3ad28-303">Définit le chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="3ad28-304">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="3ad28-304">**Key**: webroot</span></span>  
<span data-ttu-id="3ad28-305">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="3ad28-305">**Type**: *string*</span></span>  
<span data-ttu-id="3ad28-306">**Valeur par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe.</span><span class="sxs-lookup"><span data-stu-id="3ad28-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="3ad28-307">Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="3ad28-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="3ad28-308">**Définition avec** : `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="3ad28-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="3ad28-309">**Variable d’environnement** : `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="3ad28-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="3ad28-312">Remplacer la configuration</span><span class="sxs-lookup"><span data-stu-id="3ad28-312">Override configuration</span></span>

<span data-ttu-id="3ad28-313">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="3ad28-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="3ad28-314">Dans l’exemple suivant, la configuration de l’hôte est facultativement spécifiée dans un fichier *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="3ad28-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="3ad28-315">Une configuration chargée à partir du fichier *hosting.json* peut être remplacée par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3ad28-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="3ad28-316">La configuration intégrée (dans `config`) est utilisée pour configurer l’hôte avec `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ad28-318">*hosting.json* :</span><span class="sxs-lookup"><span data-stu-id="3ad28-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="3ad28-319">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="3ad28-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-321">*hosting.json* :</span><span class="sxs-lookup"><span data-stu-id="3ad28-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="3ad28-322">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="3ad28-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="3ad28-323">La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3ad28-324">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="3ad28-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="3ad28-325">La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="3ad28-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="3ad28-326">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="3ad28-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3ad28-327">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="3ad28-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3ad28-328">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="3ad28-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="3ad28-329">Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="3ad28-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="3ad28-330">L’argument de ligne de commande se substitue à la valeur `urls` fournie par le fichier *hosting.json*, et le serveur écoute le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="3ad28-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="3ad28-331">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="3ad28-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ad28-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ad28-333">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="3ad28-333">**Run**</span></span>

<span data-ttu-id="3ad28-334">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="3ad28-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3ad28-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="3ad28-335">**Start**</span></span>

<span data-ttu-id="3ad28-336">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="3ad28-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3ad28-337">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="3ad28-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="3ad28-338">L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique.</span><span class="sxs-lookup"><span data-stu-id="3ad28-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="3ad28-339">Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="3ad28-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="3ad28-340">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="3ad28-341">Commencez par un `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-342">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="3ad28-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3ad28-343">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3ad28-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3ad28-344">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="3ad28-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3ad28-345">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="3ad28-346">Commencez par une URL et `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-347">Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="3ad28-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3ad28-349">Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :</span><span class="sxs-lookup"><span data-stu-id="3ad28-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-350">Utilisez les requêtes de navigateur suivantes avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="3ad28-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="3ad28-351">Demande</span><span class="sxs-lookup"><span data-stu-id="3ad28-351">Request</span></span>                                    | <span data-ttu-id="3ad28-352">Réponse</span><span class="sxs-lookup"><span data-stu-id="3ad28-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="3ad28-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="3ad28-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="3ad28-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="3ad28-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="3ad28-355">Lève une exception avec la chaîne « ooops! »</span><span class="sxs-lookup"><span data-stu-id="3ad28-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="3ad28-356">Lève une exception avec la chaîne « Uh oh! »</span><span class="sxs-lookup"><span data-stu-id="3ad28-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="3ad28-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="3ad28-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="3ad28-358">Hello World!</span><span class="sxs-lookup"><span data-stu-id="3ad28-358">Hello World!</span></span>                             |

<span data-ttu-id="3ad28-359">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3ad28-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3ad28-360">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="3ad28-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3ad28-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3ad28-362">Utilisez une URL et une instance de `IRouteBuilder` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-363">Produit le même résultat que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="3ad28-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3ad28-365">Fournissez un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-366">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="3ad28-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3ad28-367">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3ad28-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3ad28-368">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="3ad28-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3ad28-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="3ad28-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3ad28-370">Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3ad28-371">Produit le même résultat que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ad28-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ad28-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ad28-373">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="3ad28-373">**Run**</span></span>

<span data-ttu-id="3ad28-374">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="3ad28-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3ad28-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="3ad28-375">**Start**</span></span>

<span data-ttu-id="3ad28-376">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="3ad28-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3ad28-377">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="3ad28-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3ad28-378">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="3ad28-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="3ad28-379">[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="3ad28-380">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="3ad28-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="3ad28-381">Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#startup-conventions) pour configurer l’application au démarrage en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="3ad28-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="3ad28-382">Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="3ad28-383">En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="3ad28-384">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3ad28-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="3ad28-385">Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="3ad28-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="3ad28-386">`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/index#writing-middleware) personnalisé :</span><span class="sxs-lookup"><span data-stu-id="3ad28-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3ad28-387">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="3ad28-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="3ad28-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt.</span><span class="sxs-lookup"><span data-stu-id="3ad28-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="3ad28-389">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="3ad28-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3ad28-390">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="3ad28-390">Cancellation Token</span></span>    | <span data-ttu-id="3ad28-391">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="3ad28-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="3ad28-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="3ad28-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="3ad28-393">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="3ad28-393">The host has fully started.</span></span> |
| [<span data-ttu-id="3ad28-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="3ad28-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="3ad28-395">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="3ad28-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3ad28-396">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="3ad28-396">All requests should be processed.</span></span> <span data-ttu-id="3ad28-397">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="3ad28-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="3ad28-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="3ad28-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="3ad28-399">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="3ad28-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3ad28-400">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="3ad28-400">Requests may still be processing.</span></span> <span data-ttu-id="3ad28-401">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="3ad28-401">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="3ad28-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ad28-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="3ad28-403">La classe suivante utilise `StopApplication` pour arrêter normalement une application lors de l’appel de la méthode `Shutdown` de la classe :</span><span class="sxs-lookup"><span data-stu-id="3ad28-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="3ad28-404">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="3ad28-404">Scope validation</span></span>

<span data-ttu-id="3ad28-405">Dans ASP.NET Core 2.0 ou ultérieur, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) définit [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="3ad28-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="3ad28-406">Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :</span><span class="sxs-lookup"><span data-stu-id="3ad28-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="3ad28-407">Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="3ad28-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="3ad28-408">Les services délimités ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="3ad28-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="3ad28-409">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="3ad28-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="3ad28-410">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="3ad28-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="3ad28-411">Les services délimités sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="3ad28-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="3ad28-412">Si un service délimité est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="3ad28-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="3ad28-413">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="3ad28-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="3ad28-414">Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="3ad28-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="3ad28-415">Dépannage de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="3ad28-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="3ad28-416">**S’applique à ASP.NET Core 2.0 uniquement**</span><span class="sxs-lookup"><span data-stu-id="3ad28-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="3ad28-417">Vous pouvez créer un hôte en injectant `IStartup` directement dans le conteneur d’injection de dépendances au lieu d’appeler `UseStartup` ou `Configure` :</span><span class="sxs-lookup"><span data-stu-id="3ad28-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="3ad28-418">Dans ce cas, l’erreur suivante peut se produire :</span><span class="sxs-lookup"><span data-stu-id="3ad28-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="3ad28-419">Cette erreur se produit, car [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l’assembly actuel) est requis pour analyser `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="3ad28-420">Si l’application injecte `IStartup` manuellement dans le conteneur d’injection de dépendances, ajoutez l’appel suivant à `WebHostBuilder` en spécifiant le nom de l’assembly :</span><span class="sxs-lookup"><span data-stu-id="3ad28-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="3ad28-421">Vous pouvez également ajouter un `Configure` factice à `WebHostBuilder`, qui définit automatiquement la valeur `applicationName`(`ApplicationKey`) :</span><span class="sxs-lookup"><span data-stu-id="3ad28-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="3ad28-422">**REMARQUE** : Cela est nécessaire uniquement dans ASP.NET Core 2.0 et si l’application n’appelle pas `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3ad28-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="3ad28-423">Pour plus d’informations, consultez le commentaire [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) et [l’exemple StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="3ad28-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ad28-424">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3ad28-424">Additional resources</span></span>

* [<span data-ttu-id="3ad28-425">Héberger sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="3ad28-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="3ad28-426">Héberger sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="3ad28-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="3ad28-427">Héberger sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="3ad28-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="3ad28-428">Héberger dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="3ad28-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)