Poniższe szczegóły tabeli platformy ASP.NET Core kodu generatorów parametry:

| Parametr               | Opis|
| ----------------- | ------------ |
| -m  | Nazwa modelu. |
| -dc  | Kontekst danych. |
| -udl | Użyj domyślnego układu. |
| -outDir | Ścieżka folderu wyjściowego względne do tworzenia widoków. |
| --referenceScriptLibraries | Dodaje `_ValidationScriptsPartial` to edytowanie i tworzenie stron |

Użyj `h` przełącznika, aby uzyskać pomoc dotyczącą `aspnet-codegenerator razorpage` polecenia:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).
* Test **Utwórz** łącza.

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Edytuj**, **szczegóły**, i **Usuń** łącza.

Jeśli komunikat o błędzie podobny do poniższego, sprawdź mają Uruchom migracje i aktualizacji bazy danych:

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
