* `-F|--no-formatting`

  Flaga, w których obecność pomija formatowanie odpowiedzi HTTP.

* `-h|--header`

  Ustawia nagłówek żądania HTTP. Obsługiwane są następujące formaty dwóch wartości:

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  Określa plik, do którego mają być zapisywane całej odpowiedzi HTTP (w tym nagłówki i treść). Na przykład `--response "C:\response.txt"`. Plik jest tworzony, jeśli nie istnieje.

* `--response:body`

  Określa plik, do którego mają być zapisywane w treści odpowiedzi HTTP. Na przykład `--response:body "C:\response.json"`. Plik jest tworzony, jeśli nie istnieje.

* `--response:headers`

  Określa plik, do którego powinien być zapisywany nagłówków odpowiedzi HTTP. Na przykład `--response:headers "C:\response.txt"`. Plik jest tworzony, jeśli nie istnieje.

* `-s|--streaming`

  Flaga, w których obecność umożliwia strumieniowe przesyłanie odpowiedzi HTTP.
