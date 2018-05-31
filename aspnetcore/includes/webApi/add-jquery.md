## <a name="call-the-web-api-with-jquery"></a><span data-ttu-id="b4f4b-101">Appeler l’API web avec jQuery</span><span class="sxs-lookup"><span data-stu-id="b4f4b-101">Call the Web API with jQuery</span></span>

<span data-ttu-id="b4f4b-102">Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-102">In this section, an HTML page is added that uses jQuery to call the Web API.</span></span> <span data-ttu-id="b4f4b-103">jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-103">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="b4f4b-104">Configurez le projet pour traiter les fichiers statiques et activer le mappage de fichier par défaut.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-104">Configure the project to serve static files and to enable default file mapping.</span></span> <span data-ttu-id="b4f4b-105">Pour cela, appelez les méthodes d’extension [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-105">This is accomplished by invoking the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) extension methods in *Startup.Configure*.</span></span> <span data-ttu-id="b4f4b-106">Pour plus d’informations, consultez [Fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b4f4b-106">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

<span data-ttu-id="b4f4b-107">Ajoutez un fichier HTML, nommé *index.html*, au répertoire *wwwroot* du projet.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-107">Add an HTML file, named *index.html*, to the project's *wwwroot* directory.</span></span> <span data-ttu-id="b4f4b-108">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b4f4b-108">Replace its contents with the following markup:</span></span>

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="b4f4b-109">Ajoutez un fichier JavaScript, nommé *site.js*, au répertoire *wwwroot* du projet.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-109">Add a JavaScript file, named *site.js*, to the project's *wwwroot* directory.</span></span> <span data-ttu-id="b4f4b-110">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b4f4b-110">Replace its contents with the following code:</span></span>

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="b4f4b-111">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-111">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally.</span></span> <span data-ttu-id="b4f4b-112">Ouvrez *launchSettings.json* dans le répertoire *Propriétés* du projet.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-112">Open *launchSettings.json* in the *Properties* directory of the project.</span></span> <span data-ttu-id="b4f4b-113">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-113">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="b4f4b-114">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-114">There are several ways to get jQuery.</span></span> <span data-ttu-id="b4f4b-115">Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-115">In the preceding snippet, the library is loaded from a CDN.</span></span> <span data-ttu-id="b4f4b-116">Cet exemple illustre une procédure CRUD complète qui appelle l’API avec jQuery.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-116">This sample is a complete CRUD example of calling the API with jQuery.</span></span> <span data-ttu-id="b4f4b-117">Cet exemple comprend des fonctionnalités supplémentaires qui permettent d’améliorer l’expérience.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-117">There are additional features in this sample to make the experience richer.</span></span> <span data-ttu-id="b4f4b-118">Les explications ci-dessous traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-118">Below are explanations around the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="b4f4b-119">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="b4f4b-119">Get a list of to-do items</span></span>

<span data-ttu-id="b4f4b-120">Pour obtenir une liste de tâches, envoyez une requête HTTP GET à */api/todo*.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-120">To get a list of to-do items, send an HTTP GET request to */api/todo*.</span></span>

<span data-ttu-id="b4f4b-121">La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête AJAX à l’API, qui retourne du code JSON représentant un objet ou un tableau.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-121">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends an AJAX request to the API, which returns JSON representing an object or array.</span></span> <span data-ttu-id="b4f4b-122">Cette fonction peut gérer toutes les formes de l’interaction HTTP en envoyant une requête HTTP à l’`url` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-122">This function can handle all forms of HTTP interaction, sending an HTTP request to the specified `url`.</span></span> <span data-ttu-id="b4f4b-123">`GET` est utilisé comme `type`.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-123">`GET` is used as the `type`.</span></span> <span data-ttu-id="b4f4b-124">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-124">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="b4f4b-125">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-125">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="b4f4b-126">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="b4f4b-126">Add a to-do item</span></span>

<span data-ttu-id="b4f4b-127">Pour ajouter une tâche, envoyez une requête HTTP POST à */api/todo/*.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-127">To add a to-do item, send an HTTP POST request to */api/todo/*.</span></span> <span data-ttu-id="b4f4b-128">Le corps de la requête doit contenir un objet de tâche.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-128">The request body should contain a to-do object.</span></span> <span data-ttu-id="b4f4b-129">La fonction [ajax](https://api.jquery.com/jquery.ajax/) utilise `POST` pour appeler l’API.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-129">The [ajax](https://api.jquery.com/jquery.ajax/) function is using `POST` to call the API.</span></span> <span data-ttu-id="b4f4b-130">Pour les requêtes `POST` et `PUT`, le corps de requête représente les données envoyées à l’API.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-130">For `POST` and `PUT` requests, the request body represents the data sent to the API.</span></span> <span data-ttu-id="b4f4b-131">L’API attend un corps de requête JSON.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-131">The API is expecting a JSON request body.</span></span> <span data-ttu-id="b4f4b-132">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour classer le type de média qui est reçu et envoyé (respectivement).</span><span class="sxs-lookup"><span data-stu-id="b4f4b-132">The `accepts` and `contentType` options are set to `application/json` to classify the media type being received and sent, respectively.</span></span> <span data-ttu-id="b4f4b-133">Les données sont converties en objet JSON avec [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="b4f4b-133">The data is converted to a JSON object using [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="b4f4b-134">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-134">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="b4f4b-135">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="b4f4b-135">Update a to-do item</span></span>

<span data-ttu-id="b4f4b-136">Les opérations de mise à jour et d’ajout d’une tâche s’appuient toutes les deux sur un corps de requête et sont donc très similaires.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-136">Updating a to-do item is very similar to adding one, since both rely on a request body.</span></span> <span data-ttu-id="b4f4b-137">Voici toutefois ce qui les distingue dans ce cas : l’`url` change pour ajouter l’identificateur unique de l’élément, et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-137">The only real difference between the two in this case is that the `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="b4f4b-138">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="b4f4b-138">Delete a to-do item</span></span>

<span data-ttu-id="b4f4b-139">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="b4f4b-139">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifing the item's unique identifier in the URL.</span></span>

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]