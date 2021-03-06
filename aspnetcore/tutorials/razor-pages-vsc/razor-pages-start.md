---
title: Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code
author: rick-anderson
description: Découvrez les concepts de base de la génération d’applications web de pages Razor ASP.NET Core avec Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 17ab52b80a40f6204e2bf2cf9048071c55c0a708
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252215"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core. Nous vous recommandons d’effectuer l’étape [Présentation des pages Razor](xref:mvc/razor-pages/index) avant de commencer ce didacticiel. L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

À partir d’un terminal, exécutez les commandes suivantes :

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Les commandes précédentes utilisent le [CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor. Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.

![Page d’accueil ou page d’index](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Ouvrir le projet

Appuyez sur Ctrl+C pour arrêter l’application.

Dans Visual Studio Code (VS Code), sélectionnez **Fichier > Ouvrir le dossier**, puis sélectionnez le dossier *RazorPagesMovie*.

- Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans ‘RazorPagesMovie’. Faut-il les ajouter ? »
- Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.

### <a name="launch-the-app"></a>Lancer l’application

Appuyez sur Ctrl+F5 pour démarrer l’application sans débogage. Vous pouvez également aller dans le menu **Déboguer**, puis sélectionner **Exécuter sans débogage**.

Dans le prochain didacticiel, nous allons ajouter un modèle au projet. 

> [!div class="step-by-step"]
> [Suivant : Ajout d’un modèle](xref:tutorials/razor-pages-vsc/model)  
