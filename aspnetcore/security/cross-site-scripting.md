---
title: Empêcher intersites script (XSS) dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur les scripts entre sites (XSS) et techniques pour traiter cette vulnérabilité dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: ce6bb273034c56890e0cd98b890436602b5acc69
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272446"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Empêcher intersites script (XSS) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

L'écriture de scripts entre sites (XSS) est une faille de sécurité qui permet à une personne malveillante de placer les scripts côté client (généralement JavaScript) dans les pages web. Lorsque les autres utilisateurs chargent les pages concernées les scripts des personnes malveillantes s’exécutent, donnant ainsi la possibilité à la personne malveillante de voler des cookies et des jetons, modifier le contenu de la page web via la manipulation du modèle DOM ou de rediriger le navigateur vers une autre page. Les vulnérabilités XSS se produisent généralement lorsqu’une application accepte l’entrée d'un utilisateur et lui en donne en retour une page sans validation, encodage ou échappement.

## <a name="protecting-your-application-against-xss"></a>Protection de votre application par rapport à XSS

À un niveau de base, XSS fonctionne par ruses dans votre application en insérant une balise `<script>` dans le rendu de la page ou en insérant un événement `On*` dans un élément. Les développeurs doivent utiliser les étapes de prévention suivantes pour éviter d’introduire des failles XSS dans leur application.

1. Ne placez jamais des données non fiables dans votre entrée HTML, sauf si vous suivez le reste des étapes ci-dessous. Les données non approuvées sont toutes les données qui peuvent être contrôlées par une personne malveillante, des entrées de formulaire HTML, chaînes de requête, les en-têtes HTTP, même les données provenant d’une base de données, car une personne malveillante peut être en mesure d'attaquer votre base de données même si elle ne cause pas des failles dans votre application.

2. Avant de placer des données non fiables à l’intérieur d’un élément HTML, assurez-vous qu’il est encodé au format HTML. L’encodage HTML accepte les caractères comme &lt; et les transforme en un formulaire sécurisé comme &amp;lt ;

3. Avant de placer des données non fiables dans un attribut HTML, assurez-vous qu’il est l’attribut HTML encodé. L'Encodage d’attribut HTML est un sur-ensemble de l’encodage HTML et encode les caractères supplémentaires tels que « et ».

4. Avant de mettre des données non fiables en JavaScript placez les données dans un élément HTML dont le contenu sera récupéré lors de son exécution. Si ce n’est pas possible, vérifiez que les données sont encodées en JavaScript. L'encodage de JavaScript prend des caractères dangereux pour le JavaScript et les remplace par leur valeur hexadécimale, par exemple &lt; serait codée en tant que `\u003C`.

5. Avant de placer des données non fiables dans une chaîne de requête URL assurez-vous qu'elles soient encodées URL.

## <a name="html-encoding-using-razor"></a>Encodage HTML à l’aide de Razor

Le moteur Razor automatiquement utilisé dans MVC encode toute sortie provenant de variables, sauf si vous travaillez dur pour empêcher cette opération. Il utilise les règles d'encodage d’attribut HTML chaque fois que vous utilisez la directive *@*. L'encodage d’attribut HTML est un sur-ensemble de l'encodage HTML et cela signifie que vous n’êtes pas obligé de vous préoccuper si vous devez utiliser l’encodage HTML ou l'encodage d’attribut HTML. Vous devez vous assurer que vous utilisez @ uniquement dans un contexte HTML, et non pour tenter d’insérer des entrées non approuvées directement dans JavaScript. Les programmes d’assistance de balises encoderont également m’entrée que vous utilisez dans les paramètres de la balise.

Prenez l’affichage Razor suivant ;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Cette vue renvoie le contenu de la variable *untrustedInput*. Cette variable inclut certains caractères utilisés dans les attaques XSS, à savoir &lt;, « et &gt;. L'examen de la source présente la sortie rendue encodée sous la forme :

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC fournit une classe `HtmlString` qui n’est pas encodée automatiquement lors de la sortie. Cela ne doit jamais être utilisé en association avec une entrée non approuvée car cela pourrait exposer une vulnérabilité XSS.

## <a name="javascript-encoding-using-razor"></a>Encodage de JavaScript à l’aide de Razor

Il peut arriver que vous souhaitiez insérer une valeur JavaScript à traiter dans votre affichage. Il existe deux manières de procéder. Le moyen le plus sûr pour insérer des valeurs consiste à placer la valeur dans un attribut de données d’une balise et de récupérer dans JavaScript. Exemple :

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Cela génère le code HTML suivant

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Qui, lorsqu’il s’exécute, apparaîtra comme suit.

```none
<"123">
   <"123">
   ```

Vous pouvez également appeler l’encodeur JavaScript directement,

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

Cela s'affichera dans le navigateur comme suit :

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Ne concaténez pas d'entrées non fiables dans le code JavaScript pour créer des éléments DOM. Vous devez utiliser `createElement()` et attribuer des valeurs de propriété correctement comme `node.TextContent=`, ou utilisez `element.SetAttribute()` / `element[attribute]=` sinon vous exposez à un XSS basé sur DOM.

## <a name="accessing-encoders-in-code"></a>L’accès à des encodeurs dans le code

Les encodeurs HTML, JavaScript et URL sont disponibles pour votre code de deux manières, vous pouvez les injecter via [injection de dépendance](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) ou vous pouvez utiliser les encodeurs par défaut contenus dans l'espace de noms `System.Text.Encodings.Web`. Si vous utilisez les encodeurs par défaut, alors les encodeurs que vous avez appliqués aux plages de caractères considérés comme sécurisées ne prendront pas effet - les encodeurs par défaut utilisent les règles d'encodage les plus sûres possible.

Pour utiliser les encodeurs configurables via DI votre constructeur doit prendre un paramètre *HtmlEncoder*, *JavaScriptEncoder* et *UrlEncoder* selon les besoins. Par exemple :

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>Paramètres d’encodage URL

Si vous souhaitez générer une chaîne de requête URL avec une entrée non approuvée en tant que valeur, utilisez la classe `UrlEncoder` pour encoder la valeur. Par exemple :

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Après l'encodage la variable 'encodedValue' contiendra la valeur `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Des espaces, des guillemets, des signes de ponctuation et autres caractères non sécurisés sont encodés en pourcentage dans leur valeur hexadécimale, par exemple, un caractère d’espace deviendra %20.

>[!WARNING]
> N’utilisez pas d’entrée non approuvée dans le cadre d’un chemin d’accès URL. Transmettez toujours les entrée non fiables en tant que valeurs de chaîne de requête.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personnaliser les encodeurs

Par défaut les encodeurs utilisent une liste sécurisée, limitée à la plage de base Unicode Latin, et encodent tous les caractères en dehors de cette plage selon leurs équivalents de code de caractère. Ce comportement affecte également le rendu Razor TagHelper et HtmlHelper car il utilise les encodeurs pour vos chaînes de sortie.

Le raisonnement sous-jacent vise à vous protéger des bogues de navigateur inconnus ou ultérieurs (les bogues de navigateur antérieurs ont gêné l’analyse basée sur le traitement de caractères non anglais). Si votre site web utilise de façon intensive les caractères non latins, comme le chinois, le cyrillique, ou autre, cela n'est probablement pas le comportement que vous souhaitez.

Vous pouvez personnaliser les listes sécurisées d'encodeurs pour inclure des plages Unicode appropriées à votre application lors du démarrage, dans `ConfigureServices()`.

Par exemple, à l’aide de la configuration par défaut, vous pouvez utiliser un HtmlHelper Razor comme ;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Lorsque vous affichez la source de la page web vous verrez quelle a été restituée comme suit, avec le texte chinois encodé ;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Pour élargir les caractères traités comme sécurisés par l’encodeur vous pouvez insérer la ligne suivante dans la méthode `ConfigureServices()` dans `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Cet exemple étend la liste sécurisée pour inclure la plage Unicode CjkUnifiedIdeographs. La sortie rendue deviendrait alors

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Les plages de liste verte sont spécifiées sous forme de graphiques de code Unicode, pas les langues. Le [norme Unicode](http://unicode.org/) possède une liste de [code graphiques](http://www.unicode.org/charts/index.html) que vous pouvez utiliser pour rechercher le graphique qui contient les caractères. Chaque encodeur, Html, JavaScript et Url, doit être configuré séparément.

> [!NOTE]
> La personnalisation de la liste n’affecte que les encodeurs provenant de DI. Si vous accédez directement à un encodeur via `System.Text.Encodings.Web.*Encoder.Default` ealors seule la liste fiable par défaut, Basic Latin sera utilisée.

## <a name="where-should-encoding-take-place"></a>Où l'encodage doit s'effectuer ?

Les pratiques générales acceptées est que l'encodage intervienne au point de rendu et les valeurs encodées ne doivent jamais être stockées dans une base de données. L'encodage dans la phase de rendu vous permet de modifier l’utilisation des données, par exemple, à partir de HTML à une valeur de chaîne de requête. Aussi, il vous permet de rechercher facilement vos données sans avoir à encoder les valeurs avant de les rechercher et vous permet de tirer parti des modifications ou des correctifs de bogues apportés aux encodeurs.

## <a name="validation-as-an-xss-prevention-technique"></a>Validation comme une technique de prévention XSS

La validation peut être un outil utile dans limitation des attaques XSS. Par exemple, un type numérique string contenant uniquement les caractères 0-9 ne sont pas déclencher une attaque XSS. La validation devient plus complexe si vous souhaitez accepter HTML dans l’entrée d’utilisateur - l’analyse d’entrée HTML est difficile, voire impossible. Le MarkDown et les autres formats de texte sont une option plus sûre pour l’entrée riche. Vous ne devez jamais vous appuyer sur la validation uniquement. Encodez toujours une entrée non approuvée avant le rendu, quel que soit le quel type de validation que vous avez effectué.
