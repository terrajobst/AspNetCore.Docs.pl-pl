<a name="codegenerator"></a>W poniższej tabeli przedstawiono szczegóły ASP.NET Core parametrów generatora kodu:

| Parametr               | Opis|
| ----------------- | ------------ |
| -m  | Nazwa modelu. |
| -DC  | Klasa `DbContext` do użycia. |
| -UDL | Użyj układu domyślnego. |
| -outDir | Ścieżka względna folderu wyjściowego do tworzenia widoków. |
| --referenceScriptLibraries | Dodaje `_ValidationScriptsPartial` do edycji i tworzenia stron |

Użyj przełącznika `h`, aby uzyskać pomoc dotyczącą polecenia `aspnet-codegenerator razorpage`:

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

Aby uzyskać więcej informacji, zobacz [dotnet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
