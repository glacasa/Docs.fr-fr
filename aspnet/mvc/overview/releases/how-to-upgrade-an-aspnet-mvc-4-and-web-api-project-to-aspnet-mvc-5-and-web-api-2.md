---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Comment mettre à niveau un ASP.NET MVC 4 et Web de projet d’API à ASP.NET MVC 5 et Web API 2 | Documents Microsoft
author: Rick-Anderson
description: ASP.NET MVC 5 et Web API 2 mettre un ordinateur hôte de nouvelles fonctionnalités, y compris le routage d’attributs, des filtres de l’authentification et bien plus encore.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874728"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Comment mettre à niveau un ASP.NET MVC 4 et le projet d’API Web ASP.NET MVC 5 et API Web 2
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 et Web API 2 mettre un ordinateur hôte de nouvelles fonctionnalités, y compris le routage d’attributs, des filtres de l’authentification et bien plus encore. Consultez [ https://www.asp.net/vnext ](https://www.asp.net/core) pour plus d’informations.
> 
> Cette procédure pas à pas vous guide avec les étapes requises pour mettre à niveau votre application vers la dernière version.  
> 
> > [!NOTE]
> > Consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) pour plus d’informations sur les modifications avec rupture des API Web et de MVC 4 vers la version suivante.
> 
>   
> 
> Cet article a été écrit par Youngjune Hong et Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Étapes de mise à niveau

1. Sauvegarde de votre projet. Cette procédure pas à pas vous obligent à apporter des modifications à votre fichier projet, les configuration de package et les fichiers web.config.
2. Pour mettre à niveau à partir de l’API Web API Web 2, dans global.asax, modifier :

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   par celle-ci :

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Assurez-vous que tous les packages qui utilisent de vos projets sont compatibles avec MVC 5 et des API Web 2. Le tableau indique le MVC 4 et les API Web suivantes liées packages que doivent être modifiées. Si vous avez un package qui est dépendant d’un des packages répertoriés ci-dessous, veuillez contacter les serveurs de publication pour obtenir les versions plus récentes qui sont compatibles avec MVC 5 et Web API 2. Si vous avez le code source pour ces packages, vous devez les recompiler avec les nouveaux assemblys de MVC 5 et Web API 2.   

    | **Id de package** | **Ancienne version** | **Nouvelle version** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o : p >< / o : p > | Supprimé |
    | Microsoft.AspNet.WebPages.Administration | < o : p >< / o : p > | Supprimé |
    | Microsoft-Web-Helpers | < o : p >< / o : p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Applications auxiliaires Web de Microsoft a été remplacée par Microsoft.AspNet.WebHelpers. Vous devez d’abord supprimer l’ancien package et puis installer le package plus récente.   
    >   
    > Il n’existe aucune compatibilité entre versions parmi les principaux packages d’ASP.NET. Par exemple, MVC 5 est compatible uniquement avec Razor 3 et non de 2 Razor.
4. Ouvrez votre projet dans Visual Studio 2013.
5. Supprimez l’un des packages NuGet ASP.NET suivants qui sont installés. Vous allez supprimer à l’aide de la Console Gestionnaire de Package (PMC). Pour ouvrir le PMC, sélectionnez le **outils** , puis sélectionnez **Gestionnaire de Package de bibliothèque,** puis sélectionnez **Package Manager Console**. Votre projet ne peut pas inclure tous ces éléments.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Ce package est en principe ajouté lors de la mise à niveau à partir de MVC 3 vers MVC 4. Pour le supprimer, exécutez la commande suivante dans le PMC :  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Ce package a été renommé `Microsoft.AspNet.WebHelpers`. Pour le supprimer, exécutez la commande suivante dans le PMC :  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Ce package contient une solution de contournement pour un bogue dans MVC 4 qui a été résolu dans MVC 5. Pour le supprimer, exécutez la commande suivante dans le PMC :  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Mettre à niveau tous les packages NuGet ASP.NET à l’aide de la PMC. Dans le CFP, exécutez la commande suivante :  
    `Update-Package`  
   Le `Update-Package` de commande sans aucun paramètre met à jour chaque package. Vous pouvez mettre à jour individuellement des packages à l’aide de l’argument ID. Pour plus d’informations sur la commande de mise à jour, exécutez `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Mettre à jour de l’Application *web.config* fichier

Veillez à effectuer ces modifications dans l’application *web.config* impossible dans le fichier, le *web.config* de fichiers dans le *vues* dossier.

Recherchez la `<runtime>/<assemblyBinding>` section et apportez les modifications suivantes :

1. Dans les éléments avec l’attribut de nom « System.Web.Mvc », modifiez le numéro de version à partir de « 4.0.0.0 » à « 5.0.0.0 ». (Deux modifications dans cet élément.)
2. Dans les éléments avec l’attribut de nom &quot;System.Web.Helpers » et &quot;System.Web.WebPages&quot; modifier le numéro de version à partir de « 2.0.0.0 » à « 3.0.0.0 ». Quatre modifications se produira, deux dans chacun des éléments.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Recherchez la `<appSettings>` section et mettre à jour le webpages:version de 2.0.0.0.0 à 3.0.0.0, comme indiqué ci-dessous :

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Supprimez tous les niveaux de confiance qu’intégral. Par exemple :

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Mise à jour la *web.config* fichiers sous le dossier Views

Si votre application est à l’aide de zones, vous devez également mettre à jour chacun *web.config* de fichiers dans le *vues* sous-dossier du dossier de chaque zone.

1. Mettre à jour tous les éléments qui contiennent « System.Web.Mvc » à partir de la version « 4.0.0.0 » version « 5.0.0.0 ».  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Mettre à jour tous les éléments qui contiennent « System.Web.WebPages.Razor » à partir de la version « 2.0.0.0 » version « 3.0.0.0 ». Si cette section contient « System.Web.WebPages », mettre à jour ces éléments à partir de la version « 2.0.0.0 » version « 3.0.0.0 »  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Si vous avez supprimé le `Microsoft-Web-Helpers` installation du package NuGet à l’étape précédente, `Microsoft.AspNet.WebHelpers` avec la commande suivante dans le PMC :  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Si votre application utilise le [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) (méthode), ajoutez le code suivant à la *Web.config* fichier.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Dernières étapes

Générez et testez l’application.

Supprimer le type de projet MVC 4 GUID à partir des fichiers projet.

1. Dans l’Explorateur de solutions, cliquez sur le nom du projet, puis sélectionnez **décharger le projet**.
2. Cliquez sur le projet, puis sélectionnez Modifier NomProjet.csproj.
3. Recherchez le `ProjectTypeGuids` élément, puis supprimer le MVC 4 GUID du projet `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Enregistrez et fermez le fichier de projet ouvert.
5. Cliquez sur le projet et sélectionnez **recharger le projet**.
