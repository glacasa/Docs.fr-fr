---
title: Injection de dépendances dans les vues dans ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core prend en charge l’injection de dépendances dans les vues MVC.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: cc34b9069ec062f08644c0026c1ccdcd00f667ac
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>Injection de dépendances dans les vues dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

ASP.NET Core prend en charge l’[injection de dépendances](xref:fundamentals/dependency-injection) dans les vues. Cette fonctionnalité peut être utile pour les services spécifiques à une vue, notamment la localisation ou les données requises uniquement pour remplir les éléments de la vue. Vous devez essayer de respecter le principe de [séparation des préoccupations](http://deviq.com/separation-of-concerns/) entre les contrôleurs et les vues. La plupart des données affichées dans vos vues doivent être passées par le contrôleur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Exemple simple

Vous pouvez injecter un service dans une vue à l’aide de la directive `@inject`. L’utilisation de la directive `@inject` revient à ajouter une propriété à la vue, puis à remplir cette propriété à l’aide de l’injection de dépendances.

Syntaxe de la directive `@inject` : `@inject <type> <name>`

Exemple d’exécution de la directive `@inject` :

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Cette vue affiche une liste d’instances `ToDoItem` et un récapitulatif de statistiques générales. Le récapitulatif est rempli avec les données du service `StatisticsService` injecté. Ce service est inscrit pour l’injection de dépendances sous `ConfigureServices` dans *Startup.cs* :

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` effectue des calculs sur l’ensemble des instances `ToDoItem`, auquel il accède par le biais d’un référentiel :

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

Le référentiel de l’exemple utilise une collection en mémoire. L’implémentation illustrée ci-dessus (qui s’applique à toutes les données en mémoire) n’est pas recommandée pour les jeux de données volumineux et accessibles à distance.

L’exemple affiche les données fournies par le modèle lié à la vue et le service injecté dans la vue :

![La vue des tâches To Do affiche le nombre total de tâches et de tâches terminées, la priorité moyenne, et une liste des tâches avec leur niveau de priorité et une valeur booléenne indiquant leur statut.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Remplissage des données de recherche

L’injection dans les vues peut être utile pour remplir certaines options dans les éléments d’interface utilisateur, telles que les listes déroulantes. Prenons l’exemple d’un formulaire de profil utilisateur qui comporte des options permettant de spécifier le sexe, l’État et d’autres préférences. Pour afficher un formulaire de ce type selon une approche MVC standard, il faudrait que le contrôleur demande l’accès des données aux services pour chacune des options du formulaire, puis qu’il remplisse un modèle ou `ViewBag` avec chaque ensemble d’options à lier.

Une autre approche consiste à injecter les services directement dans la vue pour obtenir les options. Cela réduit la quantité de code requis par le contrôleur, car la logique de construction de cet élément de vue est déplacée dans la vue proprement dite. Pour afficher un formulaire de modification de profil, il suffit ainsi au contrôleur de passer l’instance de profil au formulaire :

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Le formulaire HTML utilisé pour mettre à jour ces préférences inclut des listes déroulantes pour trois des propriétés :

![Vue de mise à jour du profil, avec un formulaire de saisie d’informations (nom, sexe, État et couleur préférée).](dependency-injection/_static/updateprofile.png)

Ces listes sont remplies par un service qui a été injecté dans la vue :

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` est un service au niveau de l’interface utilisateur qui est conçu pour fournir uniquement les données nécessaires dans ce formulaire :

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> N’oubliez pas d’inscrire les types à demander par le biais de l’injection de dépendances dans la méthode `ConfigureServices` dans *Startup.cs*.

## <a name="overriding-services"></a>Substitution de services

L’injection de services peut être utilisée pour injecter de nouveaux services, mais également pour substituer des services ayant déjà été injectés dans une page. La capture d’écran ci-dessous montre tous les champs disponibles dans la page utilisée dans le premier exemple :

![Menu contextuel IntelliSense pour un symbole @ entré qui affiche les champs Html, Component, StatsService et Url](dependency-injection/_static/razor-fields.png)

Comme vous pouvez le voir, les champs par défaut incluent `Html`, `Component` et `Url` (mais aussi `StatsService` que nous avons injecté). Si vous souhaitez, par exemple, remplacer les HTML Helpers par défaut par les vôtres, vous pouvez facilement le faire en utilisant `@inject` :

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Si vous souhaitez étendre des services existants, vous pouvez simplement utiliser cette technique en héritant de ou en incluant dans un wrapper l’implémentation existante avec la vôtre.

## <a name="see-also"></a>Voir aussi

* Blog de Simon Timms : [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
