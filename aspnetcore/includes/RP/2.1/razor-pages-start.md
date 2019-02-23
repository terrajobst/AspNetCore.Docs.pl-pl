Tworzy domyślny szablon **RazorPagesMovie**, **Home**, **o** i **skontaktuj się z pomocą** łącza i strony. W zależności od rozmiaru okna przeglądarki może być konieczne kliknięcie ikony nawigacji w celu wyświetlenia łącza.

![Strona główna lub indeks](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Przetestuj linki. **RazorPagesMovie** i **Home** łącza, przejdź do strony indeksu. **o** i **skontaktuj się z pomocą** łącza przejść do `About` i `Contact` stron, odpowiednio.

## <a name="project-files-and-folders"></a>Pliki projektu i foldery

W poniższej tabeli wymieniono pliki i foldery w projekcie. W tym samouczku *Startup.cs* plik jest najważniejsze informacje. Nie potrzebujesz zapoznać się z każdym linku podanego poniżej. Linki są dostarczane jako odwołanie, jeśli potrzebujesz więcej informacji na temat pliku lub folderu w projekcie.

| Plik lub folder | Cel |
| -------------- | ------- |
| *wwwroot* | Zawiera zasoby statyczne. Zobacz [pliki statyczne](xref:fundamentals/static-files). |
| *Strony* | Folder [stron Razor](xref:razor-pages/index). |
| *appsettings.json* | [Konfiguracja](xref:fundamentals/configuration/index) |
| *Program.cs* | Konfiguruje [hosta](xref:fundamentals/index#host) aplikacji platformy ASP.NET Core. |
| *Startup.cs* | Umożliwia skonfigurowanie usług i potok żądań. Zobacz [uruchamiania](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Strony/Shared folder

*_Layout.cshtml* plik zawiera wspólne elementy HTML (skryptów i stylów łącza) i ustawia układ dla aplikacji. Na przykład po wybraniu **RazorPagesMovie**, **Home**, **o** lub **skontaktuj się z pomocą**, wspólny zbiór elementów pojawia się na stronie sieci Web. Wspólne elementy obejmują menu nawigacji na początku, a nagłówek w dolnej części okna. Aby uzyskać więcej informacji, zobacz [układ](xref:mvc/views/layout).

*_ValidationScriptsPartial.cshtml* plik zawiera odwołanie do [jQuery](https://jquery.com/) skrypty sprawdzania poprawności. Gdy `Create` i `Edit` strony są dodawane w dalszej części tego samouczka *_ValidationScriptsPartial.cshtml* plik jest używany.

*_CookieConsentPartial.cshtml* plik zawiera pasek nawigacji i zawartości do podsumowania ochrony prywatności i plików cookie użyj zasad. Aby uzyskać więcej informacji na temat zasoby GDPR zawarty w projekcie, zobacz [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core)](xref:security/gdpr).

### <a name="the-pages-folder"></a>Folder stron

*_ViewStart.cshtml* ustawia stron Razor `Layout` właściwości, aby korzystała *_Layout.cshtml* pliku. Zobacz [układ](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ViewImports.cshtml* plik zawiera dyrektywy Razor, które są importowane do każdej strony Razor. Zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives) Aby uzyskać więcej informacji.

`About`, `Contact` i `Index` strony są stron podstawowych, można użyć, aby uruchomić aplikację. `Error` Strona służy do wyświetlania informacji o błędzie.
