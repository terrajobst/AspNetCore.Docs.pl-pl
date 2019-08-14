# <a name="aspnet-core-authorization-sample"></a>Przykład autoryzacji ASP.NET Core

Ten przykład ilustruje użycie Razor Pages autoryzacji według Konwencji. Ten przykład pokazuje funkcje opisane w temacie [Razor Pages Konwencji autoryzacji](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) .

Autoryzacja użytkownika w tym przykładzie korzysta z funkcji uwierzytelniania plików cookie opisanych w temacie [use cookie Authentication bez tożsamości ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) . Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core. Aby uzyskać informacje na temat korzystania z tożsamości ASP.NET Core, zobacz [wprowadzenie do tożsamości na ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Użyj adresu **maria.rodriguez@contoso.com** e-mail, aby uwierzytelnić użytkownika przy użyciu dowolnego hasła. Użytkownik jest uwierzytelniany w `AuthenticateUser` metodzie w pliku *Pages/Account/Login. cshtml. cs* . W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

| Funkcja | Opis |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony z określoną ścieżką. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze z określoną ścieżką. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony z określoną ścieżką. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze z określoną ścieżką. |
