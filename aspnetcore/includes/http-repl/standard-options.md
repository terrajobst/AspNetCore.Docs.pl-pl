* `-F|--no-formatting`

  <span data-ttu-id="902a2-101">Flaga, w których obecność pomija formatowanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="902a2-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="902a2-102">Ustawia nagłówek żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="902a2-102">Sets an HTTP request header.</span></span> <span data-ttu-id="902a2-103">Obsługiwane są następujące formaty dwóch wartości:</span><span class="sxs-lookup"><span data-stu-id="902a2-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="902a2-104">Określa plik, do którego mają być zapisywane całej odpowiedzi HTTP (w tym nagłówki i treść).</span><span class="sxs-lookup"><span data-stu-id="902a2-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="902a2-105">Na przykład `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="902a2-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="902a2-106">Plik jest tworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="902a2-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="902a2-107">Określa plik, do którego mają być zapisywane w treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="902a2-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="902a2-108">Na przykład `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="902a2-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="902a2-109">Plik jest tworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="902a2-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="902a2-110">Określa plik, do którego powinien być zapisywany nagłówków odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="902a2-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="902a2-111">Na przykład `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="902a2-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="902a2-112">Plik jest tworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="902a2-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="902a2-113">Flaga, w których obecność umożliwia strumieniowe przesyłanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="902a2-113">A flag whose presence enables streaming of the HTTP response.</span></span>
