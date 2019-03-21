---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210462"
---
```console
npm run release
```

To polecenie daje zasoby po stronie klienta, który ma być obsługiwana podczas uruchamiania aplikacji. Zasoby są umieszczane w *wwwroot* folderu.

Webpack wykonane następujące zadania:

* Przeczyścić zawartość *wwwroot* katalogu.
* Przekonwertowany TypeScript w kodzie JavaScript&mdash;proces ten jest znany jako *transpilation*.
* Wygenerowany język JavaScript, aby zmniejszyć rozmiar pliku zniekształcone&mdash;proces ten jest znany jako *minimalizację*.
* Skopiowane przetworzone pliki JavaScript, CSS i HTML z *src* do *wwwroot* katalogu.
* Dodane następujące elementy do *wwwroot/index.html* pliku:
  * A `<link>` tag, odwołuje się do *wwwroot/main.\< skrót\>.css* pliku. Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</head>` tagu.
  * A `<script>` tagu, odwołuje się do zminimalizowany *wwwroot/main.\< skrót\>js* pliku. Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</body>` tagu.
