<!-- THIS INCLUDE USED BY MVC AND RP -->
<span data-ttu-id="a8b9e-101">Dodaj następujące właściwości do `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="a8b9e-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="a8b9e-102">`Movie` Klasa zawiera:</span><span class="sxs-lookup"><span data-stu-id="a8b9e-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="a8b9e-103">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a8b9e-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="a8b9e-104">`[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data).</span><span class="sxs-lookup"><span data-stu-id="a8b9e-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="a8b9e-105">Z tego atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a8b9e-105">With this attribute:</span></span>

  * <span data-ttu-id="a8b9e-106">Użytkownik nie musi wprowadzić informacje o czasie w polu daty.</span><span class="sxs-lookup"><span data-stu-id="a8b9e-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="a8b9e-107">Tylko data jest wyświetlany, nie czas informacji.</span><span class="sxs-lookup"><span data-stu-id="a8b9e-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="a8b9e-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="a8b9e-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>