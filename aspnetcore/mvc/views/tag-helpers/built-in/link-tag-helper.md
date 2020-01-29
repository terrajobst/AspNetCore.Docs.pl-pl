---
title: Pomocnik tagu linku w ASP.NET Core
author: rick-anderson
ms.author: riande
description: Odkryj atrybuty pomocnika znacznika łącza ASP.NET Core i rolę, jaką każdy atrybut odgrywa w rozszerzeniu zachowania taga HTML tag.
ms.custom: mvc
ms.date: 09/24/2019
uid: mvc/views/tag-helpers/builtin-th/link-tag-helper
ms.openlocfilehash: d7514433bee8a138cd7d75bfd15c9798d4fd31a3
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809110"
---
# <a name="link-tag-helper-in-aspnet-core"></a>Pomocnik tagu linku w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pomocnik tagu linku](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) generuje łącze do podstawowego lub przywróconego pliku CSS. Zazwyczaj podstawowy plik CSS znajduje się w [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Pomocnik tagu linku umożliwia określenie sieci CDN dla pliku CSS i rezerwę, gdy sieć CDN jest niedostępna. Pomocnik tagu linku zapewnia wydajność sieci CDN z niezawodną obsługą hostingu lokalnego.

Poniższy znacznik Razor przedstawia element `head` pliku układu utworzonego za pomocą szablonu ASP.NET Core aplikacji sieci Web:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet)]

Następujące elementy są renderowane HTML z poprzedniego kodu (w środowisku innym niż programowanie):

[!code-csharp[](link-tag-helper/sample/HtmlPage1.html)]

W poprzednim kodzie pomocnik tagu linku wygenerował element `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` i następujący kod JavaScript, który jest używany do weryfikowania żądanego pliku *Bootstrap. min. css* jest dostępny w sieci CDN. W takim przypadku plik CSS był dostępny, aby pomocnik tagów wygenerował `<link />` element z plikiem CSS usługi CDN.

## <a name="commonly-used-link-tag-helper-attributes"></a>Atrybuty pomocnika często używanych tagów linków

Zobacz [pomocnika tagów linków](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) dla wszystkich atrybutów pomocnika tagów linków, właściwości i metod.

### <a name="href"></a>{1&gt;href&lt;1}

Preferowany adres połączonego zasobu. Adres jest przekazaniem do wygenerowanego kodu HTML we wszystkich przypadkach.

### <a name="asp-fallback-href"></a>ASP-Fallback-href

Adres URL arkusza stylów CSS, do którego ma nastąpić powrót w przypadku niepowodzenia podstawowego adresu URL.

### <a name="asp-fallback-test-class"></a>ASP-Fallback-test-Klasa

Nazwa klasy zdefiniowana w arkuszu stylów do użycia w teście Fallback. Aby uzyskać więcej informacji, zobacz temat <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>ASP-Fallback-test-Property

Nazwa właściwości CSS do użycia w teście Fallback. Aby uzyskać więcej informacji, zobacz temat <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>ASP — wartość testowa

Wartość właściwości CSS do użycia dla testu powrotu. Aby uzyskać więcej informacji, zobacz temat <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
