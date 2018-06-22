Tworzy domyślny szablon **RazorPagesMovie**, **Home**, **o** i **skontaktuj się z** łącza i strony. W zależności od rozmiaru okna przeglądarki konieczne może być kliknij ikonę nawigacji, aby wyświetlić łącza.

![Strona główna lub indeks](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Przetestuj łącza. **RazorPagesMovie** i **Home** łącza, przejdź do strony indeksu. **o** i **skontaktuj się z** łącza prowadzą do `About` i `Contact` strony odpowiednio.

## <a name="project-files-and-folders"></a>Pliki projektu i folderów

W poniższej tabeli wymieniono pliki i foldery w projekcie. W tym samouczku *Startup.cs* pliku najbardziej ważne jest zrozumienie. Nie musisz przejrzeć każdego łącza poniżej. Jeśli potrzebujesz więcej informacji na temat pliku lub folderu w projekcie, łącza znajdują się jako odwołanie.

| Plik lub folder | Cel |
| -------------- | ------- |
| *wwwroot* | Zawiera zasoby statyczne. Zobacz [pliki statyczne](xref:fundamentals/static-files). |
| *Strony* | Folder [stron Razor](xref:razor-pages/index). |
| *appsettings.json* | [Konfiguracja](xref:fundamentals/configuration/index) |
| *Program.cs* | Konfiguruje [hosta](xref:fundamentals/host/index) aplikacji platformy ASP.NET Core. |
| *Startup.cs* | Konfigurowanie usługi i żądania potoku. Zobacz [uruchamiania](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Strony/Shared folder

*_Layout.cshtml* plik zawiera wspólne elementy HTML (skryptów i stylów łącza) i ustawia układ dla aplikacji. Na przykład po wybraniu **RazorPagesMovie**, **Home**, **o** lub **skontaktuj się z**, zestaw wspólnych elementów jest wyświetlany na stronie sieci Web. Wspólne elementy obejmują menu nawigacji na początku, a nagłówka w dolnej części okna. Aby uzyskać więcej informacji, zobacz [układu](xref:mvc/views/layout).

*_ValidationScriptsPartial.cshtml* plik zawiera odwołanie do [jQuery](https://jquery.com/) skrypty sprawdzania poprawności. Gdy `Create` i `Edit` strony są dodawane później w samouczku *_ValidationScriptsPartial.cshtml* plik jest używany.

*_CookieConsentPartial.cshtml* plik zawiera paska nawigacyjnego i zawartości Podsumowując prywatności i plików cookie przy użyciu zasad. Aby uzyskać więcej informacji na zasoby GDPR dołączony do projektu, zobacz [obsługę interfejsów UE ogólne dane ochrony rozporządzenia (GDPR) w ASP.NET Core)](xref:security/gdpr).

### <a name="the-pages-folder"></a>Folder stron

*_ViewStart.cshtml* ustawia stron Razor `Layout` właściwości do użycia *_Layout.cshtml* pliku. Zobacz [układu](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ViewImports.cshtml* plik zawiera dyrektywy Razor, które są importowane do każdej stronie aparatu Razor. Zobacz [importowanie dyrektywy udostępnionych](xref:mvc/views/layout#importing-shared-directives) Aby uzyskać więcej informacji.

`About`, `Contact` i `Index` strony są stron podstawowych, można użyć, aby uruchomić aplikację. `Error` Strona służy do wyświetlania informacji o błędzie.
