---
title: Pomocy Tag obrazu, platformy ASP.NET Core
author: pkellner
description: Pokazuje, jak pracować z obrazu pomocnika tagów
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072265"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="61bc9-103">Pomocy Tag obrazu, platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61bc9-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="61bc9-104">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="61bc9-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="61bc9-105">Zwiększa pomocnika Tag obrazu `img` (`<img>`) tagu.</span><span class="sxs-lookup"><span data-stu-id="61bc9-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="61bc9-106">Wymaga on `src` tag, jak również `boolean` atrybutu `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="61bc9-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="61bc9-107">Jeśli źródło obrazu (`src`) jest plikiem statycznym na serwerze sieci web hosta unikatowy pamięci podręcznej rozrywające ciąg zostaje dołączony jako parametr zapytania do źródła obrazu.</span><span class="sxs-lookup"><span data-stu-id="61bc9-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="61bc9-108">Dzięki temu, że w przypadku zmiany pliku na serwerze sieci web hosta, adres URL żądania unikatowy jest generowany zawierającą parametr zaktualizowane żądanie.</span><span class="sxs-lookup"><span data-stu-id="61bc9-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="61bc9-109">Pamięć podręczna, rozrywające ciągu jest unikatową wartość reprezentującą skrót pliku obrazu statycznego.</span><span class="sxs-lookup"><span data-stu-id="61bc9-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="61bc9-110">Jeśli źródło obrazu (`src`) nie jest plik statyczny (na przykład zdalnego adresu URL lub plik nie istnieje na serwerze), `<img>` znacznika `src` atrybutu jest generowany z pamięci podręcznej, nie rozrywające parametru ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="61bc9-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="61bc9-111">Atrybuty pomocnika znacznika obrazu</span><span class="sxs-lookup"><span data-stu-id="61bc9-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="61bc9-112">ASP Dołącz version</span><span class="sxs-lookup"><span data-stu-id="61bc9-112">asp-append-version</span></span>

<span data-ttu-id="61bc9-113">Jeśli określona wraz z programem `src` atrybut pomocnika tagów obrazu jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="61bc9-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="61bc9-114">Przykład prawidłowego `img` pomocnika tagów jest:</span><span class="sxs-lookup"><span data-stu-id="61bc9-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="61bc9-115">Jeśli w katalogu istnieje plik statyczny *... Wwwroot/images/asplogo.PNG* wygenerowanego kodu html jest podobny do następującego (skrót będzie inny):</span><span class="sxs-lookup"><span data-stu-id="61bc9-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="61bc9-116">Wartość przypisana do parametru `v` jest wartość skrótu pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="61bc9-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="61bc9-117">Jeśli serwer sieci web nie może uzyskać dostęp do odczytu do plików statycznych odwołać się do nie `v` parametrów zostanie dodany do `src` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="61bc9-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="61bc9-118">src</span><span class="sxs-lookup"><span data-stu-id="61bc9-118">src</span></span>

<span data-ttu-id="61bc9-119">Aby aktywować pomocnika Tag obrazu, atrybut src jest wymagany dla `<img>` elementu.</span><span class="sxs-lookup"><span data-stu-id="61bc9-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="61bc9-120">Używa pomocnika Tag obrazu `Cache` dostawcy na serwerze sieci web w lokalnej do przechowywania obliczony `Sha512` określonego pliku.</span><span class="sxs-lookup"><span data-stu-id="61bc9-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="61bc9-121">Jeśli plik jest ponownie żądanie `Sha512` nie wymaga ponownego obliczenia.</span><span class="sxs-lookup"><span data-stu-id="61bc9-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="61bc9-122">Pamięć podręczna jest unieważnienie obserwatora pliku, który jest dołączony do pliku podczas pliku `Sha512` jest obliczana.</span><span class="sxs-lookup"><span data-stu-id="61bc9-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61bc9-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="61bc9-123">Additional resources</span></span>

* <xref:performance/caching/memory>
