# <a name="custom-model-binding-demo"></a>Pokaz niestandardowego powiązania modelu

Przetestuj `ByteArrayModelBinder`, uruchamiając aplikację i ogłaszając ciąg szyfrowany algorytmem Base64 w punkcie końcowym `ImageController` (`/api/image/`). Określ właściwości plik i nazwa pliku w treści żądania jako dane formularza (przy użyciu programu [Poster](https://www.getpostman.com/) lub podobnego narzędzia). Możesz użyć [tego przykładowego ciągu](Base64String.txt). Wynik jest zapisywany w folderze *wwwroot/images/upload* z określoną nazwą pliku.

Aby przetestować przykład niestandardowego powiązania, wypróbuj następujące punkty końcowe:

* /api/authors/1
* /API/Authors/2 (nie znaleziono)
* /api/boundauthors/1
* /API/boundauthors/2 (nie znaleziono)
* /api/boundauthors/get/1
* /API/boundauthors/Get/2 (brak zawartości) &ndash; ta akcja nie sprawdza wartości null i zwraca *404 nie znaleziono*.
