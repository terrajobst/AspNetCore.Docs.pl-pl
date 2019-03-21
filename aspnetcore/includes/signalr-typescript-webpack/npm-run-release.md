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

<span data-ttu-id="e3a1e-101">To polecenie daje zasoby po stronie klienta, który ma być obsługiwana podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="e3a1e-102">Zasoby są umieszczane w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="e3a1e-103">Webpack wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="e3a1e-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="e3a1e-104">Przeczyścić zawartość *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="e3a1e-105">Przekonwertowany TypeScript w kodzie JavaScript&mdash;proces ten jest znany jako *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="e3a1e-106">Wygenerowany język JavaScript, aby zmniejszyć rozmiar pliku zniekształcone&mdash;proces ten jest znany jako *minimalizację*.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="e3a1e-107">Skopiowane przetworzone pliki JavaScript, CSS i HTML z *src* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="e3a1e-108">Dodane następujące elementy do *wwwroot/index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="e3a1e-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
  * <span data-ttu-id="e3a1e-109">A `<link>` tag, odwołuje się do *wwwroot/main.\< skrót\>.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="e3a1e-110">Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</head>` tagu.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
  * <span data-ttu-id="e3a1e-111">A `<script>` tagu, odwołuje się do zminimalizowany *wwwroot/main.\< skrót\>js* pliku.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="e3a1e-112">Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</body>` tagu.</span><span class="sxs-lookup"><span data-stu-id="e3a1e-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
