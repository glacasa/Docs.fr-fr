---
title: Tag Helper Partial dans ASP.NET Core
author: scottaddie
description: Découvrez le Tag Helper Partial ASP.NET Core et le rôle de ses attributs dans le rendu d’une vue partielle.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 786c333980db89a9a5a60dc70c0bd1998ca159cd
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962592"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>Tag Helper Partial dans ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Vue d'ensemble

Le Tag Helper Partial est utilisé dans le cadre du rendu d’une [vue partielle](xref:mvc/views/partial) dans les pages Razor et les applications MVC. Tenez compte des points suivants :

* Il nécessite ASP.NET Core 2.1 ou ultérieur.
* Il constitue une alternative à la [syntaxe HTML Helper](xref:mvc/views/partial#referencing-a-partial-view).
* Il affiche la vue partielle de façon asynchrone.

Parmi les options HTML Helper utilisées pour le rendu d’une vue partielle, citons les suivantes :

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

Le modèle *Product* est utilisé dans les exemples tout au long de ce document :

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Voici l’inventaire des attributs du Tag Helper Partial.

## <a name="name"></a>name

L'attribut `name` est obligatoire. Il indique le nom ou le chemin de la vue partielle à afficher. Quand un nom de vue partielle est fourni, le processus de [découverte de vue](xref:mvc/views/overview#view-discovery) est lancé. Ce processus est ignoré quand un chemin explicite est fourni.

Le balisage suivant utilise un chemin explicite indiquant que le fichier *_ProductPartial.cshtml* doit être chargé à partir du dossier *Shared*. À l’aide de l’attribut [for](#for), un modèle est passé à la vue partielle pour liaison.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>for

L’attribut `for` affecte un [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) à évaluer par rapport au modèle actif. `ModelExpression` déduit la syntaxe `@Model.`. Par exemple, `for="Product"` peut être utilisé à la place de `for="@Model.Product"`. Pour substituer ce comportement d’inférence par défaut, utilisez le symbole `@` pour définir une expression inline. L’attribut `for` ne peut pas être utilisé avec l’attribut [model](#model).

Le balisage suivant charge *_ProductPartial.cshtml* :

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

La vue partielle est liée à la propriété `Product` du modèle de page associé :

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>modèle

L’attribut `model` affecte une instance de modèle à passer à la vue partielle. L’attribut `model` ne peut pas être utilisé avec l’attribut [for](#for).

Dans le balisage suivant, un nouvel objet `Product` est instancié et passé à l’attribut `model` pour liaison :

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>view-data

L’attribut `view-data` affecte un [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) à passer à la vue partielle. Le balisage suivant rend l’ensemble de la collection ViewData accessible à la vue partielle :

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

Dans le code précédent, la valeur de clé `IsNumberReadOnly` est `true` et ajoutée à la collection ViewData. `ViewData["IsNumberReadOnly"]` est donc accessible au sein de la vue partielle suivante :

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

Dans cet exemple, la valeur de `ViewData["IsNumberReadOnly"]` détermine si le champ *Number* s’affiche en lecture seule.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Vues partielles](xref:mvc/views/partial)
* [Données faiblement typées (ViewData, attribut ViewData et ViewBag)](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
