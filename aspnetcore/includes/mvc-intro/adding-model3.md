
## <a name="test-the-app"></a><span data-ttu-id="b0fbb-101">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b0fbb-101">Test the app</span></span>

* <span data-ttu-id="b0fbb-102">Uruchom aplikację, a następnie naciśnij przycisk **filmu Mvc** łącza.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="b0fbb-103">Naciśnij pozycję **Utwórz nowy** link i utworzyć filmu.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-103">Tap the **Create New** link and create a movie.</span></span>

  ![Utwórz widok z polami gatunku, ceny, Data wydania i tytuł](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="b0fbb-105">Nie można wprowadzić, kropki i przecinki w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="b0fbb-106">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="b0fbb-107">Zobacz [ https://github.com/aspnet/Docs/issues/4076 ](https://github.com/aspnet/Docs/issues/4076) i [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-107">See [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="b0fbb-108">Teraz po prostu wprowadź liczbami całkowitymi, takich jak 10.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="b0fbb-109">W kilku lokalizacjach, należy określić format daty.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="b0fbb-110">Zobacz wyróżniony kod poniżej.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-110">See the highlighted code below.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="b0fbb-111">Omówimy `DataAnnotations` później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="b0fbb-112">Naciskając **Utwórz** powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="b0fbb-113">Aplikacja przekierowuje do */Movies* adresu URL, w którym zostaną wyświetlone informacje filmu nowo utworzony.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Lista Film przedstawiający widok nowo utworzone filmy](~/tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="b0fbb-115">Utwórz kilka więcej wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-115">Create a couple more movie entries.</span></span> <span data-ttu-id="b0fbb-116">Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="b0fbb-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
