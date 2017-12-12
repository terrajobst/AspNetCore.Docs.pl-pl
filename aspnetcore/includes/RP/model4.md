Poniższe szczegóły tabeli platformy ASP.NET Core code generatory parametry:

| Parametr               | Opis|
| ----------------- | ------------ |
| -m  | Nazwa modelu. |
| -dc  | Kontekst danych. |
| -udl | Użyj domyślnego układu. |
| outDir — | Ścieżka folderu względna dane wyjściowe do tworzenia widoków. |
| --referenceScriptLibraries | Dodaje `_ValidationScriptsPartial` można edytować i tworzyć strony |

Użyj `h` przełącznik, aby uzyskać pomoc dotyczącą `aspnet-codegenerator razorpage` polecenia:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).
* Test **Utwórz** łącza.

 ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Edytuj**, **szczegóły**, i **usunąć** łącza.

Jeśli komunikat o błędzie podobny do następującego, sprawdź zostało uruchomione migracji i aktualizacji bazy danych:

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
