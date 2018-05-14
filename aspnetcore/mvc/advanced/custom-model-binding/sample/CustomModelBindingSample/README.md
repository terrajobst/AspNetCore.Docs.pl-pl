# <a name="custom-model-binding-demo"></a>Wersja demonstracyjna powiązania niestandardowego modelu

Test `ByteArrayModelBinder` przez aplikację i przesyłanie ciąg kodowany w formacie base64, aby `ImageController` punktu końcowego (`/api/image/`). Określ proparties pliku i nazwa pliku w treści żądania jako dane formularza (przy użyciu [Postman](https://www.getpostman.com/) lub podobnego narzędzia). Można użyć [ten przykładowy ciąg](Base64String.txt). Wynik jest zapisywany w *wwwroot/obrazów/przekazywania* folder o określono nazwę pliku.

Aby przetestować przykład niestandardowego powiązania, spróbuj następujących punktów końcowych:

* /API/authors/1
* /API/authors/2 (nie znaleziono)
* /API/boundauthors/1
* /API/boundauthors/2 (nie znaleziono)
* /API/boundauthors/Get/1
* (Brak zawartości) /API/boundauthors/Get/2 &ndash; ta akcja nie zostanie zaewidencjonowane dla wartości null i zwraca *404 — Nie znaleziono*.
