# <a name="aspnet-background-tasks-sample-generic-host"></a><span data-ttu-id="ba1aa-101">Exemples de tâches d’arrière-plan ASP.NET (hôte générique)</span><span class="sxs-lookup"><span data-stu-id="ba1aa-101">ASP.NET Background Tasks Sample (Generic Host)</span></span>

<span data-ttu-id="ba1aa-102">Cet exemple montre l’utilisation de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="ba1aa-102">This sample illustrates the use of [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span></span> <span data-ttu-id="ba1aa-103">Cet exemple montre les fonctionnalités décrites dans la rubrique [Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="ba1aa-103">This sample demonstrates the features described in the [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="ba1aa-104">Lors de l’exemple de l’exécution [Visual Studio Code](https://code.visualstudio.com/), définissez la valeur **console** de la configuration de la console dans *.vscode/launch.json* sur `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="ba1aa-104">When running the sample in [Visual Studio Code](https://code.visualstudio.com/), set the **console** value of the console configuration in *.vscode/launch.json* to either `externalTerminal` or `integratedTerminal`.</span></span> <span data-ttu-id="ba1aa-105">L’utilisation de la `internalConsole` n’est pas compatible avec l’entrée de la séquence de touches de la console que l’application utilise pour mettre en file d’attente des éléments de travail en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="ba1aa-105">Use of the `internalConsole` is incompatible with console keystroke input that the app uses to enqueue background work items.</span></span>