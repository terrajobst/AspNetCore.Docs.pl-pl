---
title: "Obraz pomocnika tagów | Dokumentacja firmy Microsoft"
author: pkellner
description: "Pokazuje, jak pracować z obrazu pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

Przez [Kellner Peterowi](http://peterkellner.net) 

Zwiększa pomocnika Tag obrazu `img` (`<img>`) tagu. Wymaga on `src` tag, jak również `boolean` atrybutu `asp-append-version`.

Jeśli źródło obrazu (`src`) jest plikiem statycznym na serwerze sieci web hosta unikatowy pamięci podręcznej rozrywające ciąg zostaje dołączony jako parametr zapytania do źródła obrazu. Dzięki temu, że w przypadku zmiany pliku na serwerze sieci web hosta, adres URL żądania unikatowy jest generowany zawierającą parametr zaktualizowane żądanie. Pamięć podręczna, rozrywające ciągu jest unikatową wartość reprezentującą skrót pliku obrazu statycznego.

Jeśli źródło obrazu (`src`) nie jest plik statyczny (na przykład zdalnego adresu URL lub plik nie istnieje na serwerze), `<img>` znacznika `src` atrybutu jest generowany z pamięci podręcznej, nie rozrywające parametru ciągu zapytania.

## <a name="image-tag-helper-attributes"></a>Atrybuty pomocnika znacznika obrazu


### <a name="asp-append-version"></a>ASP Dołącz version

Jeśli określona wraz z programem `src` atrybut pomocnika tagów obrazu jest wywoływany.

Przykład prawidłowego `img` pomocnika tagów jest:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Jeśli w katalogu istnieje plik statyczny *... Wwwroot/images/asplogo.PNG* wygenerowanego kodu html jest podobny do następującego (skrót będzie inny):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Wartość przypisana do parametru `v` jest wartość skrótu pliku na dysku. Jeśli serwer sieci web nie może uzyskać dostęp do odczytu do plików statycznych odwołać się do nie `v` parametrów zostanie dodany do `src` atrybutu.

- - -

### <a name="src"></a>src

Aby aktywować pomocnika Tag obrazu, atrybut src jest wymagany dla `<img>` elementu. 

> [!NOTE]
> Używa pomocnika Tag obrazu `Cache` dostawcy na serwerze sieci web w lokalnej do przechowywania obliczony `Sha512` określonego pliku. Jeśli plik jest ponownie żądanie `Sha512` nie wymaga ponownego obliczenia. Pamięć podręczna jest unieważnienie obserwatora pliku, który jest dołączony do pliku podczas pliku `Sha512` jest obliczana.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
