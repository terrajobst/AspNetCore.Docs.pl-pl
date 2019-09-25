---
title: Pomocnik tagu skryptu w ASP.NET Core
author: rick-anderson
ms.author: riande
description: Odkryj atrybuty pomocnika tagów ASP.NET Core i rolę, jaką każdy atrybut odgrywa w rozszerzeniu zachowania tagu skryptu HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256499"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Pomocnik tagu skryptu w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pomocnik tagu skryptu](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generuje link do pliku skryptu podstawowego lub odwracania. Zazwyczaj podstawowy plik skryptu znajduje się w [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Pomocnik tagu skryptu pozwala określić sieć CDN dla pliku skryptu i rezerwę, gdy sieć CDN nie jest dostępna. Pomocnik tagów skryptu zapewnia wydajność sieci CDN z niezawodną obsługą hostingu lokalnego.

Poniższy znacznik Razor pokazuje `script` element pliku układu utworzony za pomocą szablonu aplikacji sieci Web ASP.NET Core:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

Poniżej przedstawiono podobne do renderowanego kodu HTML z poprzedniego kodu (w środowisku nieprogramistycznym):

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

W poprzednim kodzie pomocnik tagów skryptu wygenerował drugi element skryptu ( `<script>  (window.jQuery || document.write(`), który `window.jQuery`testuje. Jeśli `window.jQuery` nie zostanie znaleziona `document.write(` , uruchamia i tworzy skrypt 

## <a name="commonly-used-script-tag-helper-attributes"></a>Atrybuty pomocnika często używanych tagów skryptu

Zobacz [pomocnik tagów skryptów](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) dla wszystkich atrybutów pomocnika tagów skryptu, właściwości i metod.

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
