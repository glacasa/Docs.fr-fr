---
title: Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core
author: shirhatti
description: Découvrez la prise en charge du débogage des applications ASP.NET Core pendant l’exécution derrière IIS sur Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233077"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core

De [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Luke Latham](https://github.com/guardrex)

Cet article décrit la prise en charge de [Visual Studio](https://www.visualstudio.com/vs/) pour le débogage des applications ASP.NET Core s’exécutant derrière IIS sur Windows Server. Cette rubrique présente l’activation de cette fonctionnalité et la configuration d’un projet.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Activer IIS

1. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).
1. Cochez la case **Services IIS (Internet Information Services)**.

![Fonctionnalités Windows montrant la case à cocher Services IIS (Internet Information Services) avec un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activées](development-time-iis-support/_static/enable_iis.png)

L’installation d’IIS peut nécessiter un redémarrage du système.

## <a name="configure-iis"></a>Configurer IIS

IIS doit avoir un site web configuré avec les éléments suivants :

* Un nom d’hôte qui correspond au nom d’hôte de l’URL du profil de lancement de l’application
* Une liaison pour le port 443 avec un certificat attribué

Par exemple, le **nom d’hôte** pour un site web ajouté est défini sur « localhost » (le profil de lancement utilise également « localhost » plus loin dans cette rubrique). Le port est défini sur « 443 » (HTTPS). Le **certificat de développement IIS Express** est attribué au site web, mais tout certificat valide convient :

![Fenêtre Ajouter un site web dans IIS, montrant la liaison définie pour localhost sur le port 443 avec un certificat attribué.](development-time-iis-support/_static/add-website-window.png)

Si l’installation d’IIS a déjà un **Site web par défaut** avec un nom d’hôte qui correspond au nom d’hôte de l’URL du profil de lancement de l’application :

* Ajoutez une liaison de port pour le port 443 (HTTPS).
* Attribuez un certificat valide au site web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Activer la prise en charge d’IIS pendant le développement dans Visual Studio

1. Lancez le programme d’installation de Visual Studio.
1. Sélectionnez le composant **Prise en charge d’IIS pendant le développement**. Le composant apparaît comme facultatif dans le panneau **Résumé** pour la charge de travail **Développement web et ASP.NET**. Le composant installe le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif nécessaire pour exécuter des applications ASP.NET Core derrière IIS dans une configuration de proxy inverse.

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné. Dans la section Web et cloud, le panneau Développement web et ASP.NET est sélectionné. Sur la droite, dans la zone Facultatif du panneau Résumé, vous voyez une case à cocher Prise en charge d’IIS pendant le développement.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurer le projet

### <a name="https-redirection"></a>Redirection HTTPS

Pour un nouveau projet, cochez la case **Configurer pour HTTPS** dans la fenêtre **Nouvelle application web ASP.NET Core** :

![Fenêtre Nouvelle application web ASP.NET Core avec la case Configurer pour HTTPS cochée.](development-time-iis-support/_static/new-app.png)

Dans un projet existant, utilisez le middleware (intergiciel) de redirection HTTPS dans `Startup.Configure` en appelant la méthode d’extension [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) :

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Profil de lancement IIS

Créez un profil de lancement pour ajouter la prise en charge d’IIS pendant le développement :

1. Pour **Profil**, sélectionnez le bouton **Nouveau**. Nommez le profil « IIS » dans la fenêtre contextuelle. Sélectionnez **OK** pour créer le profil.
1. Pour le paramètre **Lancer**, sélectionnez **IIS** dans la liste.
1. Cochez la case **Lancer le navigateur** et indiquez l’URL du point de terminaison. Utilisez le protocole HTTPS. Cet exemple utilise `https://localhost/WebApplication1`.
1. Dans la section **Variables d’environnement**, sélectionnez le bouton **Ajouter**. Indiquez une variable d’environnement ayant pour clé `ASPNETCORE_ENVIRONMENT` et pour valeur `Development`.
1. Dans la zone **Paramètres du serveur web**, définissez **l’URL de l’application**. Cet exemple utilise `https://localhost/WebApplication1`.
1. Enregistrez le profil.

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné. Les paramètres de profil et de lancement sont définis sur IIS. La fonctionnalité Lancer le navigateur est activée avec l’adresse https://localhost/WebApplication1. La même adresse est également fournie dans le champ URL de l’application de la zone Paramètres du serveur web.](development-time-iis-support/_static/project_properties.png)

Vous pouvez aussi ajouter manuellement un profil de lancement au fichier [launchSettings.json](http://json.schemastore.org/launchsettings) dans l’application :

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Exécuter le projet

Dans l’interface utilisateur de Visual Studio, définissez le bouton Exécuter sur le profil **IIS** et sélectionnez le bouton pour démarrer l’application :

![Bouton Exécuter dans la barre d’outils de Visual Studio défini sur le profil «IIS »](development-time-iis-support/_static/toolbar.png)

Visual Studio peut demander un redémarrage si vous ne l’exécutez pas en tant qu’administrateur. Si vous y êtes invité, redémarrez Visual Studio.

Si vous utilisez un certificat de développement non approuvé, le navigateur peut vous amener à créer une exception pour ce certificat.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Introduction au Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Appliquer HTTPS](xref:security/enforcing-ssl)
