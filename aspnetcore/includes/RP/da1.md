# <a name="update-the-generated-pages"></a>Aktualizowanie wygenerowanego stron

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
