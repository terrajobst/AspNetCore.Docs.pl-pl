* `-F|--no-formatting`

  <span data-ttu-id="b4359-101">Flaga, której obecność pomija formatowanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4359-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="b4359-102">Ustawia nagłówek żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4359-102">Sets an HTTP request header.</span></span> <span data-ttu-id="b4359-103">Obsługiwane są następujące dwa formaty wartości:</span><span class="sxs-lookup"><span data-stu-id="b4359-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="b4359-104">Określa plik, do którego należy zapisać całą odpowiedź HTTP (łącznie z nagłówkami i treścią).</span><span class="sxs-lookup"><span data-stu-id="b4359-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="b4359-105">Na przykład `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="b4359-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="b4359-106">Plik zostanie utworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="b4359-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="b4359-107">Określa plik, do którego powinna zostać zapisywana treść odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4359-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="b4359-108">Na przykład `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="b4359-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="b4359-109">Plik zostanie utworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="b4359-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="b4359-110">Określa plik, do którego mają być zapisywane nagłówki odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4359-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="b4359-111">Na przykład `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="b4359-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="b4359-112">Plik zostanie utworzony, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="b4359-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="b4359-113">Flaga, której obecność umożliwia przesyłanie strumieniowe odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4359-113">A flag whose presence enables streaming of the HTTP response.</span></span>
