```console
npm run release
```

To polecenie zwraca zasobów po stronie klienta, który ma być obsługiwana podczas uruchamiania aplikacji. Zasoby są umieszczane w *wwwroot* folderu.

Webpack wykonane następujące zadania:

* Wyczyszczone zawartość *wwwroot* katalogu.
* Przekonwertować maszynie JavaScript&mdash;proces znany jako *transpilation*.
* Wygenerowany kod JavaScript w celu zmniejszenie rozmiaru pliku zniekształcone&mdash;proces znany jako *minimalizację*.
* Skopiowane przetworzonych plików JavaScript, CSS i HTML z *src* do *wwwroot* katalogu.
* Dodane następujące elementy do *wwwroot/index.html* pliku:
    * A `<link>` tagu odwołujące się do *wwwroot/main.\< skrót\>.css* pliku. Ten tag znajduje się bezpośrednio przed tagiem zamykającym `</head>` tagu.
    * A `<script>` tag odwołujące się do zminimalizowany *wwwroot/main.\< skrót\>js* pliku. Ten tag znajduje się bezpośrednio przed tagiem zamykającym `</body>` tagu.