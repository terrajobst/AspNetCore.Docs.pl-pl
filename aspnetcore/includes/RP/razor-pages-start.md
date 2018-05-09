Tworzy domyślny szablon **RazorPagesMovie**, **Home**, **o** i **skontaktuj się z** łącza i strony. W zależności od rozmiaru okna przeglądarki konieczne może być kliknij ikonę nawigacji, aby wyświetlić łącza.

![Strona główna lub indeks](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Przetestuj łącza. **RazorPagesMovie** i **Home** łącza, przejdź do strony indeksu. **o** i **skontaktuj się z** łącza prowadzą do `About` i `Contact` strony odpowiednio.

## <a name="project-files-and-folders"></a>Pliki projektu i folderów

W poniższej tabeli wymieniono pliki i foldery w projekcie. W tym samouczku *Startup.cs* pliku najbardziej ważne jest zrozumienie. Nie musisz przejrzeć każdego łącza poniżej. Jeśli potrzebujesz więcej informacji na temat pliku lub folderu w projekcie, łącza znajdują się jako odwołanie.

| Plik lub folder              | Cel |
| ----------------- | ------------ | 
| wwwroot | Zawiera pliki statyczne. Zobacz [pliki statyczne](xref:fundamentals/static-files). |
| Strony | Folder [stron Razor](xref:mvc/razor-pages/index). | 
| *appsettings.json* | [Konfiguracja](xref:fundamentals/configuration/index) |
| *Program.cs* | [Hosty](xref:fundamentals/hosting) aplikacji platformy ASP.NET Core.|
| *Startup.cs* | Konfigurowanie usługi i żądania potoku. Zobacz [uruchamiania](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Folder stron

*_Layout.cshtml* plik zawiera wspólne elementy HTML (skrypty i arkusze stylów) i ustawia układ dla aplikacji. Na przykład po kliknięciu **RazorPagesMovie**, **Home**, **o** lub **skontaktuj się z**, zobacz te same elementy. Wspólne elementy obejmują menu nawigacji w górnym i nagłówek w dolnej części okna. Zobacz [układu](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ViewStart.cshtml* ustawia stron Razor `Layout` właściwości do użycia *_Layout.cshtml* pliku. Zobacz [układu](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ViewImports.cshtml* plik zawiera dyrektywy Razor, które są importowane do każdej stronie aparatu Razor. Zobacz [importowanie dyrektywy udostępnionych](xref:mvc/views/layout#importing-shared-directives) Aby uzyskać więcej informacji.

*_ValidationScriptsPartial.cshtml* plik zawiera odwołanie do [jQuery](https://jquery.com/) skrypty sprawdzania poprawności. Gdy dodamy `Create` i `Edit` stron później w samouczku *_ValidationScriptsPartial.cshtml* plik będzie używany.

`About`, `Contact` i `Index` strony są stron podstawowych, można użyć, aby uruchomić aplikację. `Error` Strona służy do wyświetlania informacji o błędzie.