---
title: Pomocnik tagu obrazu w programie ASP.NET Core
author: pkellner
description: Pokazuje, jak pracować z Pomocnik tagu obrazu.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 5eb74a6698911a1c594d11573192cb1b9ed53b49
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325838"
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

### <a name="asp-append-version"></a>ASP Dołącz version

Gdy `asp-append-version` jest określony za pomocą `true` wartość wraz z `src` wywoływaną atrybutu, Pomocnik tagu obrazu.

Pomocnik tagu obrazu można znaleźć w poniższym przykładzie:

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true" />
```

Jeśli w katalogu istnieje plik statyczny */wwwroot/obrazy/*, wygenerowany kod HTML jest podobne do następujących (skrót będą się różnić):

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM" />
```

Wartość przypisana do parametru `v` jest wartość skrótu *asplogo.png* pliku na dysku. Jeśli serwer sieci web nie może uzyskać dostęp do odczytu do pliku statycznego nie `v` parametr jest dodawany do `src` atrybut renderowanego kodu znaczników.

## <a name="hash-caching-behavior"></a>Skrót zachowanie buforowania

Pomocnik tagu obrazu używa dostawcy pamięci podręcznej na serwerze sieci web w lokalnych do przechowywania obliczony `Sha512` skrótów danego pliku. Jeśli plik jest wymagane wiele razy, nie jest ponownie obliczyć wartość skrótu. Pamięć podręczna zostaje unieważniony przez obserwatora pliku, który jest dołączony do pliku podczas pliku `Sha512` wyznaczania wartości skrótu jest obliczana. Po zmianie pliku na dysku, nowy skrót jest obliczany i pamięci podręcznej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
