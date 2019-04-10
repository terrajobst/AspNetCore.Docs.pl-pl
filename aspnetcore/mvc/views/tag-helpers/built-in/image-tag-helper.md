---
title: Pomocnik tagu obrazu w programie ASP.NET Core
author: pkellner
description: Pokazuje, jak pracować z Pomocnik tagu obrazu.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 916a68c187cbf516a59d3c5d7578cdb6ada01b86
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468821"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="f2aca-103">Pomocnik tagu obrazu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2aca-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="f2aca-104">Przez [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="f2aca-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="f2aca-105">Pomocnik tagu obrazu zwiększa `<img>` tag, aby zapewnić zachowanie rozrywające pamięci podręcznej na pliki obraz statyczny.</span><span class="sxs-lookup"><span data-stu-id="f2aca-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="f2aca-106">Ciąg rozrywające pamięci podręcznej jest wartością unikatową, reprezentujący skrót pliku obraz statyczny dołączona do adresu URL zasobu.</span><span class="sxs-lookup"><span data-stu-id="f2aca-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="f2aca-107">Unikatowy ciąg wyświetli klientów (i niektóre serwery proxy), aby ponownie załadować obrazu z hosta serwera sieci web, a nie z jego pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2aca-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="f2aca-108">Jeśli źródło obrazu (`src`) jest plikiem statycznym na serwerze sieci web hosta:</span><span class="sxs-lookup"><span data-stu-id="f2aca-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="f2aca-109">Unikatowy ciąg rozrywające pamięci podręcznej jest dołączany jako parametr zapytania do źródła obrazu.</span><span class="sxs-lookup"><span data-stu-id="f2aca-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="f2aca-110">Jeśli zmian w plikach na serwerze sieci web hosta, generowany jest żądania unikatowy adres URL, zawierającą parametr zaktualizowane żądanie.</span><span class="sxs-lookup"><span data-stu-id="f2aca-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="f2aca-111">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="f2aca-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="f2aca-112">Atrybuty Pomocnik tagu obrazu</span><span class="sxs-lookup"><span data-stu-id="f2aca-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="f2aca-113">src</span><span class="sxs-lookup"><span data-stu-id="f2aca-113">src</span></span>

<span data-ttu-id="f2aca-114">Pomocnik tagu obrazu, aktywować `src` atrybut jest wymagany w `<img>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f2aca-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="f2aca-115">Źródło obrazu (`src`) musi wskazywać plik statyczny fizyczny na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f2aca-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="f2aca-116">Jeśli `src` jest zdalny identyfikator URI, nie są generowane przez parametr ciągu zapytania rozrywające pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2aca-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="f2aca-117">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="f2aca-117">asp-append-version</span></span>

<span data-ttu-id="f2aca-118">Gdy `asp-append-version` jest określony za pomocą `true` wartość wraz z `src` wywoływaną atrybutu, Pomocnik tagu obrazu.</span><span class="sxs-lookup"><span data-stu-id="f2aca-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="f2aca-119">Pomocnik tagu obrazu można znaleźć w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f2aca-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

<span data-ttu-id="f2aca-120">Jeśli w katalogu istnieje plik statyczny */wwwroot/obrazy/*, wygenerowany kod HTML jest podobne do następujących (skrót będą się różnić):</span><span class="sxs-lookup"><span data-stu-id="f2aca-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

<span data-ttu-id="f2aca-121">Wartość przypisana do parametru `v` jest wartość skrótu *asplogo.png* pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="f2aca-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="f2aca-122">Jeśli serwer sieci web nie może uzyskać dostęp do odczytu do pliku statycznego nie `v` parametr jest dodawany do `src` atrybut renderowanego kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="f2aca-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="f2aca-123">Skrót zachowanie buforowania</span><span class="sxs-lookup"><span data-stu-id="f2aca-123">Hash caching behavior</span></span>

<span data-ttu-id="f2aca-124">Pomocnik tagu obrazu używa dostawcy pamięci podręcznej na serwerze sieci web w lokalnych do przechowywania obliczony `Sha512` skrótów danego pliku.</span><span class="sxs-lookup"><span data-stu-id="f2aca-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="f2aca-125">Jeśli plik jest wymagane wiele razy, nie jest ponownie obliczyć wartość skrótu.</span><span class="sxs-lookup"><span data-stu-id="f2aca-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="f2aca-126">Pamięć podręczna zostaje unieważniony przez obserwatora pliku, który jest dołączony do pliku podczas pliku `Sha512` wyznaczania wartości skrótu jest obliczana.</span><span class="sxs-lookup"><span data-stu-id="f2aca-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="f2aca-127">Po zmianie pliku na dysku, nowy skrót jest obliczany i pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2aca-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2aca-128">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f2aca-128">Additional resources</span></span>

* <xref:performance/caching/memory>
