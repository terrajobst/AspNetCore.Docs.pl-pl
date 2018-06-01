
## <a name="test-the-app"></a><span data-ttu-id="f0401-101">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f0401-101">Test the app</span></span>

* <span data-ttu-id="f0401-102">Uruchom aplikację i wybierz **Mvc Movie** łącza.</span><span class="sxs-lookup"><span data-stu-id="f0401-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="f0401-103">Wybierz **Utwórz nowy** i utworzyć filmu łącza.</span><span class="sxs-lookup"><span data-stu-id="f0401-103">Tap the **Create New** link and create a movie.</span></span>

  ![Utwórz widok z polami genre, cen, Data wydania i tytuł](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="f0401-105">Nie można wprowadzić kropki i przecinki w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="f0401-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="f0401-106">Do obsługi [weryfikacji jQuery](https://jqueryvalidation.org/) dla innych niż angielski, które użyj przecinka (",") dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wykonać kroki, aby globalize aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0401-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="f0401-107">Zobacz [ https://github.com/aspnet/Docs/issues/4076 ](https://github.com/aspnet/Docs/issues/4076) i [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f0401-107">See [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="f0401-108">Teraz wprowadź tylko liczby całkowite, takie jak 10.</span><span class="sxs-lookup"><span data-stu-id="f0401-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="f0401-109">W kilku lokalizacjach należy określić format daty.</span><span class="sxs-lookup"><span data-stu-id="f0401-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="f0401-110">Zobacz wyróżniony kod poniżej.</span><span class="sxs-lookup"><span data-stu-id="f0401-110">See the highlighted code below.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="f0401-111">Będzie omawianiu `DataAnnotations` dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="f0401-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="f0401-112">Naciskając **Utwórz** powoduje, że formularz, aby opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f0401-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="f0401-113">Aplikacja przekierowuje do */Movies* adres URL, w którym wyświetlane są informacje filmu nowo utworzony.</span><span class="sxs-lookup"><span data-stu-id="f0401-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Lista Film przedstawiający widok nowo utworzone filmy](~/tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="f0401-115">Utwórz kilka więcej wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="f0401-115">Create a couple more movie entries.</span></span> <span data-ttu-id="f0401-116">Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="f0401-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
