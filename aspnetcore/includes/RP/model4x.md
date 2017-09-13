<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Générer automatiquement le modèle Movie

* Ouvrez une fenêtre de commande dans le répertoire du projet (celui qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).
* Exécutez la commande suivante :

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```