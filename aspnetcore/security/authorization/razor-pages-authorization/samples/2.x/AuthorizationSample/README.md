# <a name="aspnet-core-authorization-sample"></a>Przykładowe autoryzacji platformy ASP.NET Core

Ten przykład przedstawia sposób używania metody autoryzacja stron Razor przy Konwencji. W tym przykładzie przedstawiono funkcje opisane w [konwencje autoryzacja stron Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tematu.

Autoryzacja użytkownika w tym przykładzie używa uwierzytelniania plików cookie, funkcje opisane w [Użyj plików cookie uwierzytelniania bez użycia produktu ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tematu. Aby uzyskać informacje na temat używania tożsamości platformy ASP.NET Core, zobacz <xref:security/authentication/identity>.

Podczas uruchamiania przykładu, należy użyć adresu e-mail **maria.rodriguez@contoso.com** do uwierzytelnienia użytkownika.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

| Funkcja | Opis |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony z określoną ścieżką. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze z określoną ścieżką. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony z określoną ścieżką. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze z określoną ścieżką. |
