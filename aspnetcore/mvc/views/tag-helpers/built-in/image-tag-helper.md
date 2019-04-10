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
# <a name="image-tag-helper-in-aspnet-core"></a>Pomocnik tagu obrazu w programie ASP.NET Core

Przez [Peter Kellner](http://peterkellner.net)

Pomocnik tagu obrazu zwiększa `<img>` tag, aby zapewnić zachowanie rozrywające pamięci podręcznej na pliki obraz statyczny.

Ciąg rozrywające pamięci podręcznej jest wartością unikatową, reprezentujący skrót pliku obraz statyczny dołączona do adresu URL zasobu. Unikatowy ciąg wyświetli klientów (i niektóre serwery proxy), aby ponownie załadować obrazu z hosta serwera sieci web, a nie z jego pamięci podręcznej.

Jeśli źródło obrazu (`src`) jest plikiem statycznym na serwerze sieci web hosta:

* Unikatowy ciąg rozrywające pamięci podręcznej jest dołączany jako parametr zapytania do źródła obrazu.
* Jeśli zmian w plikach na serwerze sieci web hosta, generowany jest żądania unikatowy adres URL, zawierającą parametr zaktualizowane żądanie.

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

## <a name="image-tag-helper-attributes"></a>Atrybuty Pomocnik tagu obrazu

### <a name="src"></a>src

Pomocnik tagu obrazu, aktywować `src` atrybut jest wymagany w `<img>` elementu.

Źródło obrazu (`src`) musi wskazywać plik statyczny fizyczny na serwerze. Jeśli `src` jest zdalny identyfikator URI, nie są generowane przez parametr ciągu zapytania rozrywające pamięci podręcznej.

### <a name="asp-append-version"></a>asp-append-version

Gdy `asp-append-version` jest określony za pomocą `true` wartość wraz z `src` wywoływaną atrybutu, Pomocnik tagu obrazu.

Pomocnik tagu obrazu można znaleźć w poniższym przykładzie:

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

Jeśli w katalogu istnieje plik statyczny */wwwroot/obrazy/*, wygenerowany kod HTML jest podobne do następujących (skrót będą się różnić):

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

Wartość przypisana do parametru `v` jest wartość skrótu *asplogo.png* pliku na dysku. Jeśli serwer sieci web nie może uzyskać dostęp do odczytu do pliku statycznego nie `v` parametr jest dodawany do `src` atrybut renderowanego kodu znaczników.

## <a name="hash-caching-behavior"></a>Skrót zachowanie buforowania

Pomocnik tagu obrazu używa dostawcy pamięci podręcznej na serwerze sieci web w lokalnych do przechowywania obliczony `Sha512` skrótów danego pliku. Jeśli plik jest wymagane wiele razy, nie jest ponownie obliczyć wartość skrótu. Pamięć podręczna zostaje unieważniony przez obserwatora pliku, który jest dołączony do pliku podczas pliku `Sha512` wyznaczania wartości skrótu jest obliczana. Po zmianie pliku na dysku, nowy skrót jest obliczany i pamięci podręcznej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
