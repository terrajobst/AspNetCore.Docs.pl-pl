# <a name="custom-model-binding-demo"></a>Pokaz powiązanie modelu niestandardowego

Test `ByteArrayModelBinder` , uruchamiając aplikację i publikując ciągu zakodowanego algorytmem base64, aby `ImageController` punktu końcowego (`/api/image/`). Określ właściwości pliku i nazwa pliku w treści żądania jako dane formularza (przy użyciu [Postman](https://www.getpostman.com/) lub podobnego narzędzia). Możesz użyć [to przykładowy ciąg](Base64String.txt). Wynik zostanie zapisany w *wwwroot/obrazy/przekazywania* folder przy użyciu nazwy pliku określonej.

Aby przetestować przykład niestandardowego powiązania, wypróbuj następujące punkty końcowe:

* /API/authors/1
* /API/authors/2 (nie znaleziono)
* /API/boundauthors/1
* /API/boundauthors/2 (nie znaleziono)
* /api/boundauthors/get/1
* (Brak zawartości) /API/boundauthors/Get/2 &ndash; tej akcji nie sprawdza w przypadku wartości null i zwraca *404 Nie znaleziono*.
