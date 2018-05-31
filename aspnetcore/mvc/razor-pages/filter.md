---
title: Méthodes de filtre pour les pages Razor dans ASP.NET Core
author: Rick-Anderson
description: Découvrez comment créer des méthodes de filtre pour les pages Razor dans ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/5/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/filter
ms.openlocfilehash: b04253b9240cb88c4f0d3824a4b9fda947d6da08
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="3974f-103">Méthodes de filtre pour les pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3974f-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3974f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3974f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3974f-105">Les filtres de page Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) et [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permettent aux pages Razor d’exécuter du code avant et après l’exécution d’un gestionnaire de page Razor.</span><span class="sxs-lookup"><span data-stu-id="3974f-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="3974f-106">Les filtres de page Razor sont similaires aux [filtres d’action MVC ASP.NET Core](xref:mvc/controllers/filters#action-filters), à la différence près qu’ils ne peuvent pas être appliqués aux méthodes de gestionnaire de pages individuelles.</span><span class="sxs-lookup"><span data-stu-id="3974f-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="3974f-107">Les filtres de page Razor :</span><span class="sxs-lookup"><span data-stu-id="3974f-107">Razor Page filters:</span></span>

* <span data-ttu-id="3974f-108">Exécutent le code après la sélection d’une méthode de gestionnaire, mais avant la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="3974f-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="3974f-109">Exécutent le code avant l’exécution de la méthode de gestionnaire, une fois la liaison de données terminée.</span><span class="sxs-lookup"><span data-stu-id="3974f-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="3974f-110">Exécutent le code après l’exécution de la méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="3974f-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="3974f-111">Peuvent être implémentés dans une page ou globalement.</span><span class="sxs-lookup"><span data-stu-id="3974f-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="3974f-112">Ne peuvent pas être appliqués à des méthodes de gestionnaire de page spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3974f-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="3974f-113">Le code peut être exécuté avant l’exécution d’une méthode de gestionnaire à l’aide du constructeur de page ou d’un middleware (intergiciel), mais seuls les filtres de page Razor ont accès à [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="3974f-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="3974f-114">Les filtres ont un paramètre dérivé de [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) qui fournit un accès à `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="3974f-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="3974f-115">Par exemple, l’exemple [Implémenter un attribut de filtre](#ifa) ajoute un en-tête à la réponse, ce qu’il est impossible de faire avec des constructeurs ou des middlewares.</span><span class="sxs-lookup"><span data-stu-id="3974f-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="3974f-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3974f-116">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3974f-117">Les filtres de page Razor fournissent les méthodes suivantes que vous pouvez appliquer globalement ou au niveau de la page :</span><span class="sxs-lookup"><span data-stu-id="3974f-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="3974f-118">Méthodes synchrones :</span><span class="sxs-lookup"><span data-stu-id="3974f-118">Synchronous methods:</span></span>

    * <span data-ttu-id="3974f-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : appelée après la sélection d’une méthode de gestionnaire, mais avant la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="3974f-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="3974f-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : appelée avant l’exécution de la méthode de gestionnaire, une fois la liaison de données terminée.</span><span class="sxs-lookup"><span data-stu-id="3974f-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="3974f-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : appelée avant l’exécution de la méthode de gestionnaire, avant le résultat de l’action.</span><span class="sxs-lookup"><span data-stu-id="3974f-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="3974f-122">Méthodes asynchrones :</span><span class="sxs-lookup"><span data-stu-id="3974f-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="3974f-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : appelée de manière asynchrone après la sélection d’une méthode de gestionnaire, mais avant la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="3974f-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="3974f-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : appelée de manière asynchrone avant l’appel de la méthode de gestionnaire, une fois la liaison de modèle terminée.</span><span class="sxs-lookup"><span data-stu-id="3974f-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="3974f-125">Implémentez la version synchrone **ou bien** la version asynchrone d’une interface de filtre, mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="3974f-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="3974f-126">Le framework vérifie d’abord si le filtre implémente l’interface asynchrone et, le cas échéant, il appelle cette interface.</span><span class="sxs-lookup"><span data-stu-id="3974f-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="3974f-127">Dans le cas contraire, il appelle la ou les méthodes de l’interface synchrone.</span><span class="sxs-lookup"><span data-stu-id="3974f-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="3974f-128">Si les deux interfaces sont implémentées, seules les méthodes asynchrones sont appelées.</span><span class="sxs-lookup"><span data-stu-id="3974f-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="3974f-129">La même règle s’applique aux substitutions dans les pages : implémentez la version synchrone ou asynchrone de la substitution, mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="3974f-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="3974f-130">Implémenter des filtres de page Razor globalement</span><span class="sxs-lookup"><span data-stu-id="3974f-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="3974f-131">Le code suivant implémente `IAsyncPageFilter` :</span><span class="sxs-lookup"><span data-stu-id="3974f-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="3974f-132">Dans le code précédent, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="3974f-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="3974f-133">Il est utilisé dans l’exemple pour fournir des informations de trace pour l’application.</span><span class="sxs-lookup"><span data-stu-id="3974f-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="3974f-134">Le code suivant active le filtre `SampleAsyncPageFilter` dans la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="3974f-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="3974f-135">Le code suivant montre l’intégralité de la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="3974f-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="3974f-136">Le code suivant appelle `AddFolderApplicationModelConvention` pour appliquer le filtre `SampleAsyncPageFilter` uniquement aux pages dans */subFolder* :</span><span class="sxs-lookup"><span data-stu-id="3974f-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="3974f-137">Le code suivant implémente le filtre `IPageFilter` synchrone :</span><span class="sxs-lookup"><span data-stu-id="3974f-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="3974f-138">Le code suivant active le filtre `SamplePageFilter` :</span><span class="sxs-lookup"><span data-stu-id="3974f-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="3974f-139">Implémenter des filtres de page Razor en substituant les méthodes de filtre</span><span class="sxs-lookup"><span data-stu-id="3974f-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="3974f-140">Le code suivant se substitue aux filtres de page Razor synchrones :</span><span class="sxs-lookup"><span data-stu-id="3974f-140">The following code overrides the synchronous Razor Page filters:</span></span>

<span data-ttu-id="3974f-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="3974f-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span></span>

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="3974f-142">Implémenter un attribut de filtre</span><span class="sxs-lookup"><span data-stu-id="3974f-142">Implement a filter attribute</span></span>

<span data-ttu-id="3974f-143">Le filtre intégré [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) basé sur un attribut peut être sous-classé.</span><span class="sxs-lookup"><span data-stu-id="3974f-143">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="3974f-144">Le filtre suivant ajoute un en-tête à la réponse :</span><span class="sxs-lookup"><span data-stu-id="3974f-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="3974f-145">Le code suivant applique l’attribut `AddHeader` :</span><span class="sxs-lookup"><span data-stu-id="3974f-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="3974f-146">Pour obtenir des instructions sur le remplacement de l’ordre, consultez [Remplacement de l’ordre par défaut](xref:mvc/controllers/filters#overriding-the-default-order).</span><span class="sxs-lookup"><span data-stu-id="3974f-146">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="3974f-147">Pour obtenir des instructions sur le court-circuitage du pipeline de filtres à partir d’un filtre, consultez [Annulation et court-circuit](xref:mvc/controllers/filters#cancellation-and-short-circuiting).</span><span class="sxs-lookup"><span data-stu-id="3974f-147">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="3974f-148">Attribut de filtre Authorize</span><span class="sxs-lookup"><span data-stu-id="3974f-148">Authorize filter attribute</span></span>

<span data-ttu-id="3974f-149">L’attribut [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) peut être appliqué à `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="3974f-149">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]