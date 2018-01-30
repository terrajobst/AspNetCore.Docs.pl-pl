# <a name="update-the-generated-pages"></a><span data-ttu-id="a8286-101">Aktualizowanie wygenerowanego stron</span><span class="sxs-lookup"><span data-stu-id="a8286-101">Update the generated pages</span></span>

<span data-ttu-id="a8286-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a8286-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a8286-103">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="a8286-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="a8286-104">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="a8286-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="a8286-106">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="a8286-106">Update the generated code</span></span>

<span data-ttu-id="a8286-107">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="a8286-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
