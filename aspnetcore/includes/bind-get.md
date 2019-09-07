> [!WARNING]
> Ze względów bezpieczeństwa należy zadecydować, aby powiązać `GET` dane żądania z właściwościami modelu strony. Sprawdź dane wejściowe użytkownika przed mapowaniem go na właściwości. W `GET` przypadku scenariuszy, które opierają się na ciągach zapytania lub wartościach trasy, Metoda ta jest przydatna.
>
> Aby powiązać właściwość na `GET` żądania, ustaw `SupportsGet` właściwość atrybutu [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) na: `true`
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Aby uzyskać więcej informacji, [zobacz ASP.NET Core Community standup: Powiąż przy POBIERAniu dyskusji (](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s)YouTube).
