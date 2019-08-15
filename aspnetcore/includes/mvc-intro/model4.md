W poniższej tabeli przedstawiono szczegóły ASP.NET Core parametrów generatora kodu:

| Parametr               | Opis|
| ----------------- | ------------ |
| -m  | Nazwa modelu. |
| -DC  | Kontekst danych. |
| -UDL | Użyj układu domyślnego. |
| --relativeFolderPath | Ścieżka względna folderu wyjściowego do tworzenia widoków. |
| --useDefaultLayout | Dla widoków należy używać układu domyślnego. |
| --referenceScriptLibraries | Dodaje `_ValidationScriptsPartial` do edycji i tworzenia stron |

Użyj przełącznika `h` , aby uzyskać pomoc `aspnet-codegenerator controller` dotyczącą polecenia:

```console
dotnet aspnet-codegenerator controller -h
```

Aby uzyskać więcej informacji, zobacz [dotnet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)