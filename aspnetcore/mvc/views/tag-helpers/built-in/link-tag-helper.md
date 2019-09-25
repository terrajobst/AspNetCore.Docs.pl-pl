---
title: Pomocnik tagu linku w ASP.NET Core
author: rick-anderson
ms.author: riande
description: Odkryj atrybuty pomocnika znacznika łącza ASP.NET Core i rolę, jaką każdy atrybut odgrywa w rozszerzeniu zachowania taga HTML tag.
ms.custom: mvc
ms.date: 09/24/2019
uid: mvc/views/tag-helpers/builtin-th/link-tag-helper
ms.openlocfilehash: e1e2e58b4ab9087e1f9de5b5c03b587feb88f1b9
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256520"
---
# <a name="link-tag-helper-in-aspnet-core"></a>Pomocnik tagu linku w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pomocnik tagu linku](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) generuje łącze do podstawowego lub przywróconego pliku CSS. Zazwyczaj podstawowy plik CSS znajduje się w [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Pomocnik tagu linku umożliwia określenie sieci CDN dla pliku CSS i rezerwę, gdy sieć CDN jest niedostępna. Pomocnik tagu linku zapewnia wydajność sieci CDN z niezawodną obsługą hostingu lokalnego.

Poniższy znacznik Razor pokazuje `head` element pliku układu utworzony za pomocą szablonu aplikacji sieci Web ASP.NET Core:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet)]

Następujące elementy są renderowane HTML z poprzedniego kodu (w środowisku innym niż programowanie):

[!code-csharp[](link-tag-helper/sample/HtmlPage1.html)]

W poprzednim kodzie pomocnik tagu linku wygenerował `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` element i następujący kod JavaScript, który jest używany do weryfikowania żądanego pliku *Bootstrap. min. css* jest dostępny w sieci CDN. W takim przypadku plik CSS był dostępny, aby pomocnik tagów wygenerował `<link />` element z plikiem CSS usługi CDN.

## <a name="commonly-used-link-tag-helper-attributes"></a>Atrybuty pomocnika często używanych tagów linków

Zobacz [pomocnika tagów linków](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) dla wszystkich atrybutów pomocnika tagów linków, właściwości i metod.

### <a name="href"></a>{1&gt;href&lt;1}

Preferowany adres połączonego zasobu. Adres jest przekazaniem do wygenerowanego kodu HTML we wszystkich przypadkach.

### <a name="asp-fallback-href"></a>ASP-Fallback-href

Adres URL arkusza stylów CSS, do którego ma nastąpić powrót w przypadku niepowodzenia podstawowego adresu URL.

### <a name="asp-fallback-test-class"></a>ASP-Fallback-test-Klasa

Nazwa klasy zdefiniowana w arkuszu stylów do użycia w teście Fallback. Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>ASP-Fallback-test-Property

Nazwa właściwości CSS do użycia w teście Fallback. Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>ASP — wartość testowa

Wartość właściwości CSS do użycia dla testu powrotu. Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

### <a name="asp-fallback-test-value"></a>ASP — wartość testowa

Wartość właściwości CSS do użycia dla testu powrotu. Aby uzyskać więcej informacji, zobacz<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
