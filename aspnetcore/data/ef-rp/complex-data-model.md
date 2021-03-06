---
title: Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8
author: rick-anderson
description: Dans ce tutoriel, vous ajoutez des entités et des relations, et vous personnalisez le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage.
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2cec45afbf08e5dd379a54e780e4218bfc86d13f
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741275"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8

Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Dans les didacticiels précédents, nous avons travaillé avec un modèle de données de base composé de trois entités. Dans ce didacticiel :

* Nous allons ajouter d’autres entités et relations
* Nous allons personnaliser le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données.

Les classes d’entités pour le modèle de données sont illustrées ci-dessous :

![Diagramme des entités](complex-data-model/_static/diagram.png)

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Personnaliser le modèle de données avec des attributs

Dans cette section, nous allons personnaliser le modèle de données à l’aide d’attributs.

### <a name="the-datatype-attribute"></a>Attribut DataType

Actuellement, les pages sur les étudiants affichent l’heure et la date d’inscription. En règle générale, les champs de date ne montrent que la date, et non l’heure.

Mettez à jour *Models/Student.cs* avec le code en surbrillance suivant :

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

L’attribut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) spécifie un type de données qui est plus spécifique que le type intrinsèque de la base de données. Ici, seule la date doit être affichée (pas la date et l’heure). L’énumération [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency, EmailAddress, et ainsi de suite. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Exemple :

* Le lien `mailto:` est créé automatiquement pour `DataType.EmailAddress`.
* Le sélecteur de date est fourni pour `DataType.Date` dans la plupart des navigateurs.

L’attribut `DataType` émet des attributs HTML 5 `data-` utilisés par les navigateurs HTML 5. Les attributs `DataType` ne fournissent aucune validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de date est affiché conformément aux formats par défaut basés sur l’objet [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) du serveur.

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme doit également être appliquée à l’interface utilisateur de modification. Certains champs ne doivent pas utiliser `ApplyFormatInEditMode`. Par exemple, le symbole monétaire ne doit généralement pas être affiché dans une zone de texte d’édition.

L’attribut `DisplayFormat` peut être utilisé seul. Il est généralement préférable d’utiliser l’attribut `DataType` avec l’attribut `DisplayFormat`. L’attribut `DataType` transmet la sémantique des données, plutôt que la manière de l’afficher à l’écran. L’attribut `DataType` offre les avantages suivants qui ne sont pas disponibles dans `DisplayFormat` :

* Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une validation des entrées côté client, et ainsi de suite).
* Par défaut, le navigateur affiche les données à l’aide du format correspondant aux paramètres régionaux.

Pour plus d’informations, consultez la [documentation relative au Tag Helper \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).

Exécutez l’application. Accédez à la page d’index des étudiants. Les heures ne sont plus affichées. Tous les affichages qui utilisent le modèle `Student` affichent la date sans heure.

![Page d’index des étudiants affichant les dates sans les heures](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Attribut StringLength

Vous pouvez également spécifier des règles de validation de données et des messages d’erreur de validation à l’aide d’attributs. L’attribut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) spécifie les longueurs minimale et maximale de caractères autorisées dans un champ de données. L’attribut `StringLength` fournit également la validation côté client et côté serveur. La valeur minimale n’a aucun impact sur le schéma de base de données.

Mettez à jour le modèle `Student` avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Le code précédent limite la longueur des noms à 50 caractères. L’attribut `StringLength` n’empêche pas un utilisateur d’entrer un espace blanc comme nom. L’attribut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) est utilisé pour appliquer des restrictions à l’entrée. Par exemple, le code suivant exige que le premier caractère soit une majuscule et que les autres caractères soient alphabétiques :

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Exécutez l’application :

* Accédez à la page Students.
* Sélectionnez **Create New** et entrez un nom de plus de 50 caractères.
* Sélectionnez **Create**. La validation côté client affiche un message d’erreur.

![Page d’index des étudiants affichant des erreurs de longueur de chaîne](complex-data-model/_static/string-length-errors.png)

Dans **l’Explorateur d’objets SQL Server** (SSOX), ouvrez le concepteur de tables Student en double-cliquant sur la table **Student**.

![Table Students dans SSOX avant les migrations](complex-data-model/_static/ssox-before-migration.png)

L’image précédente montre le schéma pour la table `Student`. Les champs de nom sont de type `nvarchar(MAX)`, car les migrations n’ont pas été exécutées sur la base de données. Quand nous exécuterons les migrations plus loin dans ce didacticiel, les champs de nom deviendront `nvarchar(50)`.

### <a name="the-column-attribute"></a>Attribut Column

Les attributs peuvent contrôler comment les classes et les propriétés sont mappées à la base de données. Dans cette section, nous utilisons l’attribut `Column` pour mapper le nom de la propriété `FirstMidName` « FirstName » dans la base de données.

Lors de la création de la base de données, les noms des propriétés sur le modèle sont utilisés comme noms de colonne (sauf quand l’attribut `Column` est utilisé).

Le modèle `Student` utilise `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom.

Mettez à jour *Student.cs* avec le code en surbrillance suivant :

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Avec la modification précédente, `Student.FirstMidName` dans l’application est mappé à la colonne `FirstName` de la table `Student`.

L’ajout de l’attribut `Column` change le modèle sur lequel repose `SchoolContext`. Le modèle sur lequel repose le `SchoolContext` ne correspond plus à la base de données. Si vous exécutez l’application avant d’appliquer les migrations, l’exception suivante est générée :

```SQL
SqlException: Invalid column name 'FirstName'.
```
Pour mettre à jour la base de données

* Générez le projet.
* Ouvrez une fenêtre de commande dans le dossier du projet. Entrez les commandes suivantes pour créer une migration et mettre à jour la base de données :

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

La commande `dotnet ef migrations add ColumnFirstName` génère le message d’avertissement suivant :

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Cet avertissement est généré, car les champs de nom sont désormais limités à 50 caractères. Si un nom dans la base de données a plus de 50 caractères, tous les caractères au-delà du cinquantième sont perdus.

* Testez l’application.

Ouvrez la table Student dans SSOX :

![Table Students dans SSOX après les migrations](complex-data-model/_static/ssox-after-migration.png)

Avant l’application de la migration, les colonnes de nom étaient de type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Les colonnes de nom sont maintenant `nvarchar(50)`. Le nom de la colonne est passé de `FirstMidName` à `FirstName`.

> [!Note]
> Dans la section suivante, la génération de l’application à certaines étapes génère des erreurs du compilateur. Les instructions indiquent quand générer l’application.

## <a name="student-entity-update"></a>Mise à jour de l’entité Student

![Entité Student](complex-data-model/_static/student-entity.png)

Mettez à jour *Models/Student.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Attribut Required

L’attribut `Required` fait des propriétés de nom des champs obligatoires. L’attribut `Required` n’est pas nécessaire pour les types non nullables tels que les types valeur (`DateTime`, `int`, `double` et ainsi de suite). Les types qui n’acceptent pas les valeurs Null sont traités automatiquement comme des champs obligatoires.

L’attribut `Required` peut être remplacé par un paramètre de longueur minimale dans l’attribut `StringLength` :

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Attribut Display

L’attribut `Display` indique que la légende des zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date ». Les légendes par défaut ne comportaient pas d’espace entre les mots, par exemple « Lastname ».

### <a name="the-fullname-calculated-property"></a>Propriété calculée FullName

`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés. `FullName` ne peut pas être définie. Elle a uniquement un accesseur get. Aucune colonne `FullName` n’est créée dans la base de données.

## <a name="create-the-instructor-entity"></a>Créer l’entité Instructor

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

Créez *Models/Instructor.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Notez que plusieurs propriétés sont identiques dans les entités `Student` et `Instructor`. Dans le didacticiel Implémentation de l’héritage plus loin dans cette série, ce code est refactorisé afin d’éliminer la redondance.

Plusieurs attributs peuvent être sur une seule ligne. Les attributs `HireDate` peuvent être écrits comme suit :

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Propriétés de navigation CourseAssignments et OfficeAssignment

Les propriétés `CourseAssignments` et `OfficeAssignment` sont des propriétés de navigation.

Un formateur pouvant animer un nombre quelconque de cours, `CourseAssignments` est défini comme une collection.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Si une propriété de navigation contient plusieurs entités :

* Elle doit être un type de liste où les entrées peuvent être ajoutées, supprimées et mises à jour.

Les types de propriétés de navigation sont :

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Si `ICollection<T>` est spécifié, EF Core crée une collection `HashSet<T>` par défaut.

L’entité `CourseAssignment` est expliquée dans la section sur les relations plusieurs-à-plusieurs.

Les règles professionnelles de Contoso University stipulent qu’un formateur peut avoir au plus un bureau. La propriété `OfficeAssignment` contient une seule entité `OfficeAssignment`. `OfficeAssignment` a la valeur Null si aucun bureau n’est affecté.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Créer l’entité OfficeAssignment

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Créez *Models/OfficeAssignment.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Attribut Key

L’attribut `[Key]` est utilisé pour identifier une propriété comme clé primaire quand le nom de la propriété est autre que classnameID ou ID.

Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`. Une affectation de bureau existe uniquement relativement au formateur auquel elle est assignée. La clé primaire `OfficeAssignment` est également sa clé étrangère pour l’entité `Instructor`. EF Core ne peut pas reconnaître automatiquement `InstructorID` comme clé primaire de `OfficeAssignment`, car :

* `InstructorID` ne suit pas la convention de nommage ID ou classnameID.

Ainsi, l’attribut `Key` est utilisé pour identifier `InstructorID` comme clé primaire :

```csharp
[Key]
public int InstructorID { get; set; }
```

Par défaut, EF Core traite la clé comme n’étant pas générée par la base de données, car la colonne est utilisée pour une relation d’identification.

### <a name="the-instructor-navigation-property"></a>Propriété de navigation Instructor

La propriété de navigation `OfficeAssignment` pour l’entité `Instructor` est nullable car :

* Les types référence (tels que les classes) sont nullables.
* Un formateur peut ne pas avoir d’affectation de bureau.


L’entité `OfficeAssignment` a une propriété de navigation `Instructor` non-nullable car :

* `InstructorID` n'autorise pas les valeurs Null.
* Une affectation de bureau ne peut pas exister sans un formateur.

Quand une entité `Instructor` a une entité `OfficeAssignment` associée, chaque entité a une référence à l’autre dans sa propriété de navigation.

L’attribut `[Required]` peut être appliqué à la propriété de navigation `Instructor` :

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Le code précédent spécifie qu’il doit y avoir un formateur associé. Le code précédent n’est pas nécessaire, car la clé étrangère `InstructorID` (qui est également la clé primaire) n'autorise pas les valeurs Null.

## <a name="modify-the-course-entity"></a>Modifier l’entité Course

![Entité Course](complex-data-model/_static/course-entity.png)

Mettez à jour *Models/Course.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

L’entité `Course` a une propriété de clé étrangère `DepartmentID`. `DepartmentID` pointe vers l’entité `Department` associée. L’entité `Course` a une propriété de navigation `Department`.

EF Core n’exige pas de propriété de clé étrangère pour un modèle de données quand le modèle a une propriété de navigation pour une entité associée.

EF Core crée automatiquement des clés étrangères dans la base de données partout où elles sont nécessaires. EF Core crée des [propriétés cachées](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pour les clés étrangères créées automatiquement. Le fait d’avoir la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces. Par exemple, considérez un modèle où la propriété de clé étrangère `DepartmentID` n’est *pas* incluse. Quand une entité Course est récupérée en vue d’une modification :

* L’entité `Department` a la valeur Null si elle n’est pas chargée explicitement.
* Pour mettre à jour l’entité Course, vous devez d’abord récupérer l’entité `Department`.

Quand la propriété de clé étrangère `DepartmentID` est incluse dans le modèle de données, il n’est pas nécessaire de récupérer l’entité `Department` avant une mise à jour.

### <a name="the-databasegenerated-attribute"></a>Attribut DatabaseGenerated

L’attribut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indique que la clé primaire est fournie par l’application plutôt que générée par la base de données.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Par défaut, EF Core part du principe que les valeurs de clé primaire sont générées par la base de données. Les valeurs de clé primaire générées par la base de données constituent en général la meilleure approche. Pour les entités `Course`, l’utilisateur spécifie la clé primaire. Par exemple, un numéro de cours comme une série 1000 pour le département Mathématiques et une série 2000 pour le département Anglais.

Vous pouvez aussi utiliser l’attribut `DatabaseGenerated` pour générer des valeurs par défaut. Par exemple, la base de données peut générer automatiquement un champ de date pour enregistrer la date de création ou de mise à jour d’une ligne. Pour plus d’informations, consultez [Propriétés générées](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de clé étrangère et de navigation

Les propriétés de clé étrangère et les propriétés de navigation dans l’entité `Course` reflètent les relations suivantes :

Un cours étant affecté à un seul département, il y a une clé étrangère `DepartmentID` et une propriété de navigation `Department`.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un cours pouvant avoir un nombre quelconque d’étudiants inscrits, la propriété de navigation `Enrollments` est une collection :

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `CourseAssignments` est une collection :

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` est expliquée [plus loin](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Créer l’entité Department

![Entité Department](complex-data-model/_static/department-entity.png)

Créez *Models/Department.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Attribut Column

Précédemment, vous avez utilisé l’attribut `Column` pour changer le mappage des noms de colonne. Dans le code de l’entité `Department`, l’attribut `Column` est utilisé pour changer le mappage des types de données SQL. La colonne `Budget` est définie à l’aide du type monétaire de SQL Server dans la base de données :

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Le mappage de colonnes n’est généralement pas nécessaire. EF Core choisit en général le type de données SQL Server approprié en fonction du type CLR de la propriété. Le type CLR `decimal` est mappé à un type SQL Server `decimal`. `Budget` étant une valeur monétaire, le type de données money est plus approprié.

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de clé étrangère et de navigation

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

* Un département peut avoir ou ne pas avoir un administrateur.
* Un administrateur est toujours un formateur. Ainsi, la propriété `InstructorID` est incluse en tant que clé étrangère de l’entité `Instructor`.

La propriété de navigation se nomme `Administrator`, mais elle contient une entité `Instructor` :

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Le point d’interrogation (?) dans le code précédent indique que la propriété est nullable.

Un département pouvant avoir de nombreux cours, il existe une propriété de navigation Courses :

```csharp
public ICollection<Course> Courses { get; set; }
```

Remarque : Par convention, EF Core autorise la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs. Les suppressions en cascade peuvent engendrer des règles de suppression en cascade circulaires. Les règles de suppression en cascade circulaires provoquent une exception quand une migration est ajoutée.

Par exemple, si la propriété `Department.InstructorID` n’a pas été définie comme nullable :

* EF Core configure une règle de suppression en cascade pour supprimer le formateur quand le département est supprimé.
* Supprimer le formateur quand le département est supprimé n’est pas le comportement souhaité.

Si les règles d’entreprise exigent que la propriété `InstructorID` soit non nullable, utilisez l’instruction d’API Fluent suivante :

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Le code précédent désactive la suppression en cascade sur la relation formateur-département.

## <a name="update-the-enrollment-entity"></a>Mettre à jour l’entité Enrollment

Un enregistrement d’inscription concerne un seul cours suivi par un seul étudiant.

![Entité Enrollment](complex-data-model/_static/enrollment-entity.png)

Mettez à jour *Models/Enrollment.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de clé étrangère et de navigation

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

Un enregistrement d’inscription ne concernant qu’un seul cours, il y a une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` :

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Un enregistrement d’inscription ne concernant qu’un seul étudiant, il y a une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` :

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relations plusieurs-à-plusieurs

Il existe une relation plusieurs-à-plusieurs entre les entités `Student` et `Course`. L’entité `Enrollment` joue le rôle de table de jointure plusieurs-à-plusieurs *avec charge utile* dans la base de données. « Avec charge utile » signifie que la table `Enrollment` contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans le cas présent, la clé primaire et `Grade`).

L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité. (Ce diagramme a été généré à l’aide de EF Power Tools pour EF 6.x. Sa création ne fait pas partie de ce didacticiel.)

![Relation plusieurs à plusieurs Student-Course](complex-data-model/_static/student-course.png)

Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (*) à l’autre, ce qui indique une relation un-à-plusieurs.

Si la table `Enrollment` n’incluait pas d’informations de notes, elle aurait uniquement besoin de contenir les deux clés étrangères (`CourseID` et `StudentID`). Une table de jointure plusieurs-à-plusieurs sans charge utile est parfois appelée « table de jointure pure ».

Les entités `Instructor` et `Course` ont une relation plusieurs-à-plusieurs à l’aide d’une table de jointure pure.

Remarque : EF 6.x prend en charge les tables de jointure implicites pour les relations plusieurs-à-plusieurs, mais pas EF Core. Pour plus d’informations, consultez [Many-to-many relationships in EF Core 2.0 (Relations plusieurs-à-plusieurs dans EF Core 2.0)](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Entité CourseAssignment

![Entité CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Créez *Models/CourseAssignment.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Instructor-Courses

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

La relation plusieurs-à-plusieurs Instructor-Courses :

* Nécessite une table de jointure qui doit être représentée par un jeu d’entités.
* Est une table de jointure pure (table sans charge utile).

Il est courant de nommer une entité de jointure `EntityName1EntityName2`. Par exemple, la table de jointure Instructor-to-Courses utilisant ce modèle est `CourseInstructor`. Toutefois, nous vous recommandons d’utiliser un nom qui décrit la relation.

Les modèles de données sont simples au début, puis ils augmentent en complexité. Les jointures sans charge utile (tables de jointure pures) évoluent fréquemment pour inclure une charge utile. En commençant par un nom d’entité descriptif, vous n’aurez pas besoin de modifier le nom quand la table de jointure changera. Dans l’idéal, l’entité de jointure aura son propre nom (éventuellement un mot unique) naturel dans le domaine d’entreprise. Par exemple, les livres et les clients pourraient être liés avec une entité de jointure nommée Évaluations. Pour la relation plusieurs-à-plusieurs Instructor-Courses, il vaut mieux utiliser `CourseAssignment` que `CourseInstructor`.

### <a name="composite-key"></a>Clé composite

Les clés étrangères ne sont pas nullables. Ensemble, le deux clés étrangères dans `CourseAssignment` (`InstructorID` et `CourseID`) identifient de façon unique chaque ligne de la table `CourseAssignment`. `CourseAssignment` ne nécessite pas de clé primaire dédiée. Les propriétés `InstructorID` et `CourseID` fonctionnent comme une clé primaire composite. Le seul moyen de spécifier des clés primaires composites dans EF Core consiste à faire appel à l’*API Fluent*. La section suivante montre comment configurer la clé primaire composite.

La clé composite garantit que :

* Plusieurs lignes sont autorisées pour un cours.
* Plusieurs lignes sont autorisées pour un formateur.
* Plusieurs lignes pour la même paire formateur-cours ne sont pas autorisées.

Comme l’entité de jointure `Enrollment` définit sa propre clé primaire, des doublons de ce type sont possibles. Pour éviter ces doublons :

* Ajoutez un index unique sur les champs de clé primaire, ou
* Configurez `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`. Pour plus d'informations, consultez [Index](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Mettre à jour le contexte de base de données

Ajoutez le code en surbrillance suivant à *Data/SchoolContext.cs* :

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Le code précédent ajoute les nouvelles entités et configure la clé primaire composite de l’entité `CourseAssignment`.

## <a name="fluent-api-alternative-to-attributes"></a>Alternative d’API Fluent aux attributs

La méthode `OnModelCreating` du code précédent utilise l’*API Fluent* pour configurer le comportement d’EF Core. L’API est appelée « Fluent », car elle est souvent utilisée en enchaînant une série d’appels de méthode en une seule instruction. Le [code suivant](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) est un exemple de l’API Fluent :

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Dans ce didacticiel, l’API Fluent est utilisée uniquement pour le mappage de base de données qui ne peut pas être effectué avec des attributs. Toutefois, l’API Fluent peut spécifier la plupart des règles de mise en forme, de validation et de mappage pouvant être spécifiées à l’aide d’attributs.

Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent. `MinimumLength` ne change pas le schéma. Il applique uniquement une règle de validation de longueur minimale.

Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ». Vous pouvez combiner des attributs et l’API Fluent. Certaines configurations peuvent être effectuées uniquement avec l’API Fluent (spécification d’une clé primaire composite). Certaines autres peuvent être effectuées uniquement avec des attributs (`MinimumLength`). Voici ce que nous recommandons pour l’utilisation des API Fluent ou des attributs :

* Choisissez l’une de ces deux approches.
* Dans la mesure du possible, utilisez l’approche choisie de manière cohérente.

Certains des attributs utilisés dans ce didacticiel sont utilisés pour :

* La validation uniquement (par exemple, `MinimumLength`).
* La configuration d’EF Core uniquement (par exemple, `HasKey`).
* La validation et la configuration d’EF Core (par exemple, `[StringLength(50)]`).

Pour plus d’informations sur les attributs et l’API Fluent, consultez [Méthodes de configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagramme des entités montrant les relations

L’illustration suivante montre le diagramme que les outils EF Power créent pour le modèle School complet.

![Diagramme des entités](complex-data-model/_static/diagram.png)

Le diagramme précédent montre :

* Plusieurs lignes de relations un-à-plusieurs (1 à \*).
* La ligne de relation un-à-zéro-ou-un (1 à 0..1) entre les entités `Instructor` et `OfficeAssignment`.
* La ligne de relation zéro-ou-un-à-plusieurs (0..1 à *) entre les entités `Instructor` et `Department`.

## <a name="seed-the-db-with-test-data"></a>Amorcer la base de données avec des données de test

Mettez à jour le code dans *Data/DbInitializer.cs* :

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Le code précédent fournit des données de valeur initiale pour les nouvelles entités. La majeure partie de ce code crée des objets d’entités et charge des exemples de données. Les exemples de données sont utilisés à des fins de test. Le code précédent crée les relations plusieurs-à-plusieurs suivantes :

* `Enrollments`
* `CourseAssignment`

Remarque : [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) prendra en charge l’[amorçage de données](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Ajouter une migration

Générez le projet. Ouvrez une fenêtre de commande dans le dossier de projet et entrez la commande suivante :

```console
dotnet ef migrations add ComplexDataModel
```

La commande précédente affiche un avertissement concernant les pertes de données possibles.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Si la commande `database update` est exécutée, l’erreur suivante se produit :

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Quand des migrations sont exécutées avec des données existantes, il peut y avoir des contraintes de clé étrangère qui ne sont pas satisfaites avec les données de sortie. Pour ce didacticiel, une nouvelle base de données est créée ; il n’y a donc aucune violation de contrainte de clé étrangère. Pour obtenir des instructions sur la façon de corriger les violations de clé étrangère sur la base de données actuelle, consultez [Correction des contraintes de clé étrangère avec des données héritées](#fk).

## <a name="change-the-connection-string-and-update-the-db"></a>Modifier la chaîne de connexion et mettre à jour la base de données

Le code dans le `DbInitializer` mis à jour ajoute des données de valeur initiale pour les nouvelles entités. Pour forcer EF Core à créer une base de données vide :

* Remplacez le nom de la chaîne de connexion de base de données dans *appsettings.json* par ContosoUniversity3. Le nouveau nom doit être un nom qui n’a pas été utilisé sur l’ordinateur.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Vous pouvez également supprimer la base de données à l’aide de :

  * L’**Explorateur d'objets SQL Server** (SSOX).
  * La commande CLI `database drop` :

    ```console
    dotnet ef database drop
    ```

Exécutez `database update` dans la fenêtre de commande :

```console
dotnet ef database update
```

La commande précédente exécute toutes les migrations.

Exécutez l’application. L’exécution de l’application entraîne l’exécution de la méthode `DbInitializer.Initialize`. La méthode `DbInitializer.Initialize` remplit la nouvelle base de données.

Ouvrez la base de données dans SSOX :

* Développez le nœud **Tables**. Les tables créées sont affichées.
* Si SSOX était déjà ouvert, cliquez sur le bouton **Actualiser**.

![Tables dans SSOX](complex-data-model/_static/ssox-tables.png)

Examinez la table **CourseAssignment** :

* Cliquez avec le bouton droit sur la table **CourseAssignment** et sélectionnez **Afficher les données**.
* Vérifiez que la table **CourseAssignment** contient des données.

![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Correction des contraintes de clé étrangère avec des données héritées

Cette section est facultative.

Quand des migrations sont exécutées avec des données existantes, il peut y avoir des contraintes de clé étrangère qui ne sont pas satisfaites avec les données de sortie. Avec des données de production, vous devez effectuer certaines actions pour migrer les données existantes. Cette section fournit un exemple de correction des violations de contraintes de clé étrangère. N’effectuez pas ces modifications de code sans une sauvegarde. N’effectuez pas ces modifications de code si vous avez terminé la section précédente et mis à jour la base de données.

Le fichier *{horodatage}_ComplexDataModel.cs* contient le code suivant :

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Le code précédent ajoute une clé étrangère `DepartmentID` non-nullable à la table `Course`. La base de données du didacticiel précédent contient des lignes dans `Course` ; cette table ne peut donc pas être mise à jour par des migrations.

Pour faire en sorte que la migration `ComplexDataModel` fonctionne avec des données existantes

* Modifiez le code pour affecter une valeur par défaut à la nouvelle colonne (`DepartmentID`).
* Créez un département fictif nommé « Temp » assumant la fonction de département par défaut.

### <a name="fix-the-foreign-key-constraints"></a>Corriger les contraintes de clé étrangère

Mettez à jour la méthode `Up` de la classe `ComplexDataModel` :

* Ouvrez le fichier *{horodatage}_ComplexDataModel.cs*.
* Commentez la ligne de code qui ajoute la colonne `DepartmentID` à la table `Course`.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Ajoutez le code en surbrillance suivant. Le nouveau code va après le bloc `.CreateTable( name: "Department"` : [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Avec les modifications précédentes, les lignes `Course` existantes seront toutes associées au département « Temp » après l’exécution de la méthode `Up` de `ComplexDataModel`.

Une application de production :

* Comprendrait du code ou des scripts pour ajouter des lignes `Department` et des lignes `Course` associées aux nouvelles lignes `Department`.
* N’utiliserait pas le département « Temp » ou la valeur par défaut pour `Course.DepartmentID`.

Le didacticiel suivant traite des données associées.

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/migrations)
> [Suivant](xref:data/ef-rp/read-related-data)
