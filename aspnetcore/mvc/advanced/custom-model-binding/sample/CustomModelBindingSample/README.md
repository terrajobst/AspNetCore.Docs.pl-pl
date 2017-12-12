# <a name="custom-model-binding-demo"></a>Wersja demonstracyjna powiązania niestandardowego modelu

Możesz przetestować `ByteArrayModelBinder` działania aplikacji i przesyłanie ciąg kodowany w formacie base64 do punktu końcowego ImageController (/ api/obrazu /). Należy określić proparties pliku i nazwa pliku w żądaniu treści jako dane formularza (przy użyciu Postman lub podobnego narzędzia). Można użyć [ten przykładowy ciąg](Base64String.txt). Wynik zostanie zapisany w folderze wwwroot/obrazów/przekazywania wskazana nazwa pliku.

Aby przetestować przykład niestandardowego powiązania, spróbuj następujących punktów końcowych: /api/authors/1 /api/authors/2 (nie znaleziono) /api/boundauthors/1 /api/boundauthors/2 (nie znaleziono) /api/boundauthors/get/2 /api/boundauthors/get/1 (bez zawartości) — Ta akcja nie zostanie zaewidencjonowane dla wartości null i zwracać nie znaleziono
