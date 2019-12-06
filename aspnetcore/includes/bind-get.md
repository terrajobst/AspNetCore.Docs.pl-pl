> [!WARNING]
> Ze względów bezpieczeństwa musisz zrezygnować z, aby powiązać `GET` dane żądania z właściwościami modelu strony. Sprawdź dane wejściowe użytkownika przed mapowaniem go na właściwości. Wyznaczanie do powiązania `GET` jest przydatne podczas rozwiązywania scenariuszy, które opierają się na ciągach zapytania lub wartościach tras.
>
> Aby powiązać właściwość na żądaniach `GET`, ustaw właściwość `SupportsGet` atrybutu [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) na `true`:
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Aby uzyskać więcej informacji, zobacz [ASP.NET Core Community standup: bind on Get Discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).
