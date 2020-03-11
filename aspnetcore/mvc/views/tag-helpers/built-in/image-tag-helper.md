---
title: Pomocnik tagu obrazu w ASP.NET Core
author: pkellner
description: Pokazuje, jak korzystać z pomocnika tagów obrazu.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 964072ad276f7e3e411ee41cb03a2efb9d05c585
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663997"
---
# <a name="image-tag-helper-in-aspnet-core"></a>Pomocnik tagu obrazu w ASP.NET Core

Według [Peterowi Kellner](https://peterkellner.net)

Pomocnik tagów obrazu ulepsza tag `<img>`, aby zapewnić zachowanie Busting pamięci podręcznej dla statycznych plików obrazu.

Busting pamięci podręcznej jest unikatową wartością reprezentującą skrót pliku obrazu statycznego dołączonego do adresu URL zasobu. Unikatowy ciąg będzie monitował klientów (i niektórych serwerów proxy) do ponownego załadowania obrazu z serwera hosta sieci Web, a nie z pamięci podręcznej klienta.

Jeśli źródło obrazu (`src`) jest plikiem statycznym na serwerze sieci Web hosta:

* Unikatowy ciąg Busting jest dołączany jako parametr zapytania do źródła obrazu.
* Jeśli plik na serwerze sieci Web hosta ulegnie zmianie, generowany jest unikatowy adres URL żądania, który obejmuje zaktualizowany parametr żądania.

Aby zapoznać się z omówieniem pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

## <a name="image-tag-helper-attributes"></a>Atrybuty pomocnika tagów obrazu

### <a name="src"></a>SRC

Aby uaktywnić pomocnika tagów obrazu, atrybut `src` jest wymagany dla elementu `<img>`.

Źródło obrazu (`src`) musi wskazywać fizyczny plik statyczny na serwerze. Jeśli `src` jest zdalnym identyfikatorem URI, parametr ciągu zapytania cache-Busting nie jest generowany.

### <a name="asp-append-version"></a>ASP — dołączanie wersji

Gdy `asp-append-version` jest określony z wartością `true` wraz z atrybutem `src`, zostanie wywołana pomocnika znacznika obrazu.

Poniższy przykład używa pomocnika tagu obrazu:

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

Jeśli plik statyczny istnieje w katalogu */wwwroot/images/* , wygenerowany kod HTML jest podobny do następującego (skrót będzie różny):

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

Wartość przypisana do parametru `v` jest wartością skrótu pliku *asplogo. png* na dysku. Jeśli serwer sieci Web nie może uzyskać dostępu do odczytu do pliku statycznego, nie zostanie dodany parametr `v` do atrybutu `src` w renderowanej adjustacji.

## <a name="hash-caching-behavior"></a>Zachowanie buforowania wartości skrótu

Pomocnik tagu obrazu używa dostawcy pamięci podręcznej na lokalnym serwerze sieci Web do przechowywania obliczonego skrótu `Sha512` danego pliku. Jeśli plik jest żądany wielokrotnie, skrót nie jest obliczany ponownie. Pamięć podręczna jest unieważniona przez obserwatora plików, który jest dołączony do pliku, gdy zostanie obliczony skrót `Sha512` pliku. Gdy plik zostanie zmieniony na dysku, zostanie obliczony i zbuforowany nowy skrót.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
