# <a name="aspnet-core-authorization-sample"></a>Przykładowe autoryzacji platformy ASP.NET Core

W tym przykładzie przedstawiono użycie autoryzacji stron Razor przy użyciu Konwencji. W tym przykładzie pokazano funkcje opisane w [konwencje autoryzacji stron Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tematu.

Autoryzacja użytkownika w tym przykładzie używa uwierzytelniania plików cookie funkcje opisane w [Użyj plików cookie uwierzytelniania bez ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tematu. Informacje na temat używania ASP.NET Core Identity, zobacz Tematy w [uwierzytelniania](https://docs.microsoft.com/aspnet/core/security/authentication/index) sekcji dokumentacji.

W przypadku uruchamiania próbki, należy użyć adresu e-mail **maria.rodriguez@contoso.com** do uwierzytelnienia użytkownika.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

| Funkcja | Opis |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony z określoną ścieżką. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Dodaje [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze z określoną ścieżką. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony z określoną ścieżką. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Dodaje [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze z określoną ścieżką. |
