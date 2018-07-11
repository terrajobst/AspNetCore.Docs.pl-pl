# <a name="update-the-generated-pages"></a>Aktualizuj wygenerowanych stron

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy wyświetlić czas (12:00:00 AM na ilustracji poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa słowa).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
