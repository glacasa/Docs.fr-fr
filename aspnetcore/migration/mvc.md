---
title: Migrer à partir de ASP.NET MVC à cœur d’ASP.NET MVC
author: ardalis
description: Découvrez comment commencer la migration vers ASP.NET MVC de base d’un projet ASP.NET MVC.
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 6fecc820177ca5033cfd00d632904a950c1b29bb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274773"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrer à partir de ASP.NET MVC à cœur d’ASP.NET MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), et [Scott Addie](https://scottaddie.com)

Cet article explique comment commencer la migration d’un projet ASP.NET MVC à [ASP.NET Core MVC](../mvc/overview.md). Dans le processus, il présente la plupart des éléments qui ont été modifiés dans ASP.NET MVC. Migration à partir d’ASP.NET MVC est un processus en plusieurs étapes, et cet article couvre la configuration initiale, contrôleurs de base et les vues, contenu statique et dépendances du côté client. Autres articles couvrent la migration de la configuration et le code d’identité trouvée dans de nombreux projets ASP.NET MVC.

> [!NOTE]
> Les numéros de version dans les exemples ne sont peut-être pas ceux en cours. Vous devrez peut-être mettre à jour vos projets en conséquence.

## <a name="create-the-starter-aspnet-mvc-project"></a>Créer le projet ASP.NET MVC starter

Pour illustrer la mise à niveau, nous allons commencer par la création d’une application ASP.NET MVC. Créer avec le nom *WebApp1* afin de l’espace de noms correspond au projet ASP.NET Core nous créer à l’étape suivante.

![Boîte de dialogue Visual Studio nouveau projet](mvc/_static/new-project.png)

![Boîte de dialogue nouvelle Application Web : modèle de projet MVC sélectionné dans le panneau de configuration de modèles ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Facultatif :* Changez le nom de la solution de *WebApp1* en *Mvc5*. Visual Studio affiche le nouveau nom de solution (*Mvc5*), ce qui facilite l’indiquer à ce projet à partir du projet suivant.

## <a name="create-the-aspnet-core-project"></a>Créer le projet ASP.NET Core

Créez une nouvelle application web ASP.NET Core *vide* avec le même nom que le projet précédent (*WebApp1*) afin de faire correspondre les espaces de noms dans les deux projets. Avoir le même espace de noms rend plus facile la copie de code entre les deux projets. Vous devez créer ce projet dans un autre répertoire que le projet précédent pour utiliser le même nom.

![Boîte de dialogue Nouveau projet](mvc/_static/new_core.png)

![Boîte de dialogue nouvelle Application Web ASP.NET : modèle de projet vide sélectionnée dans le panneau de configuration de modèles de base ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Facultatif :* créer une nouvelle application ASP.NET Core à l’aide du *Application Web* modèle de projet. Nommez le projet *WebApp1*, puis sélectionnez une option d’authentification de **comptes d’utilisateur individuels**. Renommer cette application *FullAspNetCore*. Création de projet pouvez gagner du temps lors de la conversion. Vous pouvez examiner le code généré de modèle pour afficher le résultat final ou copiez le code dans le projet de conversion. Il est également utile lorsque vous êtes bloqué sur une étape de conversion à comparer avec le projet de modèle généré.

## <a name="configure-the-site-to-use-mvc"></a>Configurer le site pour utiliser MVC

* Lorsque vous ciblez .NET Core, la metapackage ASP.NET Core est ajouté au projet, appelé `Microsoft.AspNetCore.All` par défaut. Ce package contient des packages comme `Microsoft.AspNetCore.Mvc` et `Microsoft.AspNetCore.StaticFiles`. Si vous ciblez .NET Framework, les références de package doivent être répertoriée individuellement dans le fichier *.csproj.

`Microsoft.AspNetCore.Mvc` est le framework ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles` est le Gestionnaire de fichiers statiques. Le runtime ASP.NET Core est modulaire, et vous devez explicitement s’abonner à des fichiers statiques (consultez [fichiers statiques](xref:fundamentals/static-files)).

* Ouvrez le fichier *Startup.cs* et modifiez le code comme suit :

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

La méthode d’extension `UseStaticFiles` ajoute le Gestionnaire de fichiers statiques. Comme mentionné précédemment, le runtime ASP.NET est modulaire, et vous devez explicitement autoriser les fichiers statiques. La méthode d’extension `UseMvc` ajoute le routage. Pour plus d’informations, consultez [démarrage de l’Application](xref:fundamentals/startup) et [routage](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Ajouter un contrôleur et une vue

Dans cette section, vous allez ajouter un contrôleur minimal et la vue comme des espaces réservés pour le contrôleur ASP.NET MVC et les vues que vous allez migrer dans la section suivante.

* Ajoutez un dossier *contrôleurs*.

* Ajouter un **classe de contrôleur** nommé *HomeController.cs* à la *contrôleurs* dossier.

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/add_mvc_ctl.png)

* Ajoutez un dossier *Views*.

* Ajoutez un dossier *Views/Home*.

* Ajouter un **vue Razor** nommé *Index.cshtml* à la *Views/Home* dossier.

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/view.png)

La structure de projet est indiquée ci-dessous :

![Explorateur de solutions affichant les fichiers et dossiers de WebApp1](mvc/_static/project-structure-controller-view.png)

Remplacez le contenu du fichier *Views/Home/Index.cshtml* avec les éléments suivants :

```html
<h1>Hello world!</h1>
```

Exécuter l’application.

![Application Web ouverte dans Microsoft Edge](mvc/_static/hello-world.png)

Consultez [contrôleurs](xref:mvc/controllers/actions) et [vues](xref:mvc/views/overview) pour plus d’informations.

Maintenant que nous avons un projet ASP.NET Core minimal, nous pouvons commencer la migration des fonctionnalités à partir du projet ASP.NET MVC. Nous devons déplacer les éléments suivants :

* Contenu côté client (CSS, polices et scripts)

* Contrôleurs

* Vues

* modèles

* Regroupement

* filtres

* Ouvrez une session en entrée/sortie, identité (cela dans l’étape suivante du didacticiel.)

## <a name="controllers-and-views"></a>Contrôleurs et vues

* Chacune des méthodes copier à partir de l’ASP.NET MVC `HomeController` au nouveau `HomeController`. Notez que dans ASP.NET MVC, type de retour de méthode de modèle prédéfini contrôleur action est [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); dans ASP.NET MVC de base, les méthodes d’action retournés `IActionResult` à la place. `ActionResult` implémente `IActionResult`, il est donc inutile de modifier le type de retour de vos méthodes d’action.

* Copiez les fichiers de vues Razor *About.cshtml*, *Contact.cshtml* et *Index.cshtml* du projet ASP.NET MVC dans le projet ASP.NET Core.

* Exécutez l’application ASP.NET Core et chaque méthode de test. Nous n’avons pas encore effectué la migration l’ou les styles de mise en page, par conséquent, les vues de rendu contient uniquement le contenu dans les fichiers de vue. Comme vous ne disposez pas des liens générés du fichier de mise en page pour les vues `About` et `Contact`, vous devez les appeler à partir du navigateur (remplacez **4492** par le numéro de port utilisé dans votre projet).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Page de contact](mvc/_static/contact-page.png)

Notez l’absence de styles et d’éléments de menu. Nous le corrigerons dans la section suivante.

## <a name="static-content"></a>Contenu statique

Dans les versions précédentes d’ASP.NET MVC, contenu statique a été hébergé à partir de la racine du projet web et a été inséré avec fichiers côté serveur. Dans ASP.NET Core, le contenu statique est hébergé dans le dossier *wwwroot*. Vous devez copier le contenu statique de votre ancienne application ASP.NET MVC dans le dossier *wwwroot* de votre projet ASP.NET Core. Dans cette exemple de conversion :

* Copie le *favicon.ico* fichier à partir de l’ancien projet MVC dans le *wwwroot* dossier dans le projet ASP.NET Core.

L’ancien ASP.NET MVC project utilise [Bootstrap](https://getbootstrap.com/) pour que son style et le programme d’amorçage des fichiers dans le *contenu* et *Scripts* dossiers. Le modèle, ce qui a généré l’ancien projet ASP.NET MVC, fait référence à l’amorçage dans le fichier de disposition (*Views/Shared/_Layout.cshtml*). Vous pouvez copier le *bootstrap.js* et *bootstrap.css* à partir de l’ASP.NET MVC, les fichiers de projet pour le *wwwroot* dossier dans le nouveau projet. Au lieu de cela, nous allons ajouter la prise en charge pour le démarrage (et autres bibliothèques côté client) à l’aide du CDN dans la section suivante.

## <a name="migrate-the-layout-file"></a>Migrer le fichier de disposition

* Copiez le fichier *_ViewStart.cshtml* à partir du dossier *Views* de l’ancien projet ASP.NET MVC dans le dossier *Views* du projet de ASP.NET Core. Le fichier *_ViewStart.cshtml* n’a pas changé dans ASP.NET Core MVC.

* Créez un dossier *Views/Shared*.

* *Facultatif :* copie *_ViewImports.cshtml* à partir de la *FullAspNetCore* du projet MVC *vues* dossier dans le projet de ASP.NET Core  *Vues* dossier. Supprimez toute déclaration d’espace de noms dans le *_ViewImports.cshtml* fichier. Le *_ViewImports.cshtml* fournit des espaces de noms pour tous les fichiers de l’affichage de fichiers et permet de bénéficier de [programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro). Programmes d’assistance de balise sont utilisés dans le nouveau fichier de disposition. Le *_ViewImports.cshtml* fichier est une nouveauté pour ASP.NET Core.

* Copiez le fichier *_Layout.cshtml* à partir du dossier *Views/Shared* de l’ancien projet ASP.NET MVC dans le dossier *Views/Shared* du projet ASP.NET Core.

Ouvrez *_Layout.cshtml* et apportez les modifications suivantes (le code complet est indiqué ci-dessous) :

* Remplacez `@Styles.Render("~/Content/css")` par un élément `<link>` pour charger *bootstrap.css* (voir ci-dessous).

* Supprimez `@Scripts.Render("~/bundles/modernizr")`.

* Commentez la ligne `@Html.Partial("_LoginPartial")` (en la plaçant entre `@*...*@`). Nous y reviendrons dans un prochain didacticiel.

* Remplacez `@Scripts.Render("~/bundles/jquery")` par un élément `<script>` (voir ci-dessous).

* Remplacez `@Scripts.Render("~/bundles/bootstrap")` par un élément `<script>` (voir ci-dessous).

Le balisage de remplacement pour l’inclusion d’amorçage CSS :

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Le balisage de remplacement de jQuery et d’inclusion de programme d’amorçage JavaScript :

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

La mise à jour *_Layout.cshtml* fichier est présenté ci-dessous :

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Afficher le site dans le navigateur. Il doit maintenant charger correctement, avec les styles attendus en place.

* *Facultatif :* Vous pouvez essayer d’utiliser le nouveau fichier de mise en page. Pour ce projet, copiez le fichier de mise en page à partir du projet *FullAspNetCore*. Le nouveau fichier de mise en page utilise les [Tags Helpers](xref:mvc/views/tag-helpers/intro) et comprend d’autres améliorations.

## <a name="configure-bundling-and-minification"></a>Configurer le regroupement et minimisation

Pour plus d’informations sur la configuration du regroupement et de la minimisation, consultez [Regroupement et Minimisation](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Résoudre les erreurs HTTP 500

De nombreux problèmes peuvent provoquer un message d’erreur HTTP 500 qui ne donne aucune information sur la source du problème. Par exemple, si le fichier *Views/_ViewImports.cshtml* contient un espace de noms qui n’existe pas dans votre projet, vous obtenez une erreur HTTP 500. Par défaut dans les applications ASP.NET Core, la `UseDeveloperExceptionPage` extension est ajoutée à la `IApplicationBuilder` et exécutées lorsque la configuration est *développement*. Cela est détaillée dans le code suivant :

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core convertit les exceptions non gérées dans une application web des réponses d’erreur HTTP 500. Normalement, les détails de l’erreur ne sont pas inclus dans ces réponses pour éviter la divulgation d’informations sensibles sur le serveur. Consultez **à l’aide de la Page d’Exception Developer** dans [gérer les erreurs](../fundamentals/error-handling.md) pour plus d’informations.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Développement côté client](xref:client-side/index)
* [Les Tag Helpers](xref:mvc/views/tag-helpers/intro)
