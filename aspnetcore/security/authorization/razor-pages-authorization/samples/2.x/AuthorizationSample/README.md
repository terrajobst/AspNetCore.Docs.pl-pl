# <a name="aspnet-core-authorization-sample"></a>Przykładowe autoryzacji platformy ASP.NET Core

Ten przykład przedstawia sposób używania metody autoryzacja stron Razor przy Konwencji. W tym przykładzie przedstawiono funkcje opisane w [konwencje autoryzacja stron Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tematu.

Autoryzacja użytkownika w tym przykładzie używa uwierzytelniania plików cookie, funkcje opisane w [Użyj plików cookie uwierzytelniania bez użycia produktu ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tematu. Pojęcia i przykłady przedstawione w tym temacie stosuje się jednakowo do aplikacji, które używają tożsamości platformy ASP.NET Core. Aby uzyskać informacje na temat używania tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości programu ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Użyj adresu e-mail **maria.rodriguez@contoso.com** do uwierzytelnienia użytkownika w dowolnym hasłem. Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku. Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

| Funkcja | Opis |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony z określoną ścieżką. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze z określoną ścieżką. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony z określoną ścieżką. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze z określoną ścieżką. |
