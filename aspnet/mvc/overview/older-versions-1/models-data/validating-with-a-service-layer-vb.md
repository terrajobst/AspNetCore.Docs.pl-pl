---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: "Sprawdzanie poprawności z warstwy usług (VB) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Dowiedz się, jak przenieść logiki sprawdzania poprawności poza akcji kontrolera i do warstwy oddzielne usługi. W tym samouczku Stephen Walther wyjaśniono, jak można..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a8f1dd888c7fa6a3353b7b748a0ffa30b94149c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-vb"></a>Sprawdzanie poprawności z warstwy usług (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Dowiedz się, jak przenieść logiki sprawdzania poprawności poza akcji kontrolera i do warstwy oddzielne usługi. W tym samouczku Stephen Walther wyjaśniono, jak można zachować sharp separacji przez izolowanie z warstwy usług z warstwą kontrolera.


Celem tego samouczka jest do opisywania jedną metodę przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak przenieść logiki sprawdzania poprawności z kontrolerami poza i do warstwy oddzielne usługi.

## <a name="separating-concerns"></a>Oddzielanie problemy

Podczas tworzenia aplikacji platformy ASP.NET MVC, nie należy umieszczać logiki bazy danych wewnątrz akcji kontrolera. Mieszanie logiki bazy danych i kontrolera utrudnia aplikacji do obsługi wraz z upływem czasu. Zaleca się umieścić wszystkie logiki bazy danych w warstwie oddzielne repozytorium.

Na przykład 1 Lista zawiera prostego repozytorium o nazwie ProductRepository. Repozytorium produktu zawiera wszystkie dane kod dostępu dla aplikacji. Lista zawiera również interfejs IProductRepository, który implementuje repozytorium produktu.

**Wyświetlanie listy 1 - Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Kontroler wyświetlania 2 używa warstwy repozytorium zarówno jego indeks() i Create() akcje. Zwróć uwagę, czy ten kontroler nie zawiera wszelka logika bazy danych. Tworzenie warstwy repozytorium umożliwia zachowanie separacji czyste. Kontrolery są odpowiedzialne za logiki kontroli przepływu aplikacji i repozytorium jest odpowiedzialny za logika dostępu do danych.

**Wyświetlanie listy 2 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

Tak logiki kontroli przepływu aplikacji należy w kontrolerze i logika dostępu do danych, należy w repozytorium. W takim przypadku, gdy opracować logiki sprawdzania poprawności? Jedną z opcji jest umieszczenie logiki sprawdzania poprawności w *warstwy usług*.

Warstwy usług to dodatkowa warstwa w aplikacji ASP.NET MVC, która przekazuje komunikację między kontrolerem i warstwy repozytorium. Warstwy usługi zawiera logiki biznesowej. W szczególności zawiera logikę weryfikacji.

Na przykład warstwy usług produktu w wersji 3 wyświetlania ma metodę CreateProduct(). Metoda CreateProduct() wywołuje metodę ValidateProduct() do sprawdzania poprawności nowego produktu przed przekazaniem produktu do repozytorium produktu.

**Wyświetlanie listy 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Kontroler produktu został zaktualizowany w listę 4, aby używać warstwy usług zamiast warstwy repozytorium. Warstwa kontrolera komunikuje się warstwy usług. Warstwy usług komunikuje się z warstwą repozytorium. Każda warstwa ma oddzielne odpowiedzialności.

**Wyświetlanie listy 4 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Zwróć uwagę, czy usługa produktu został utworzony w Konstruktorze kontroler produktu. Po utworzeniu usługi produktów słownik stanów modelu jest przekazywany do usługi. Usługa produktu używa stan modelu do przekazywania komunikatów o błędach weryfikacji do kontrolera.

## <a name="decoupling-the-service-layer"></a>Rozdzielenie warstwy usług

Firma Microsoft nie izolować kontrolera i warstwy usługi, w odniesieniu do jednego. Kontroler i warstwy usługi do komunikacji za pośrednictwem stanu modelu. Innymi słowy warstwy usług ma zależność na poszczególnych funkcji platformę ASP.NET MVC.

Chcemy warstwy usług z naszych możliwie warstwy kontrolera Izoluj. Teoretycznie możemy należy używać warstwy usług z dowolnego typu aplikacji i nie tylko aplikacji platformy ASP.NET MVC. Na przykład w przyszłości może chcemy kompilacji frontonu dla aplikacji WPF. Firma Microsoft stwierdzi, sposób, aby usunąć zależności na platformie ASP.NET MVC stan modelu z naszych warstwy usług.

Wyświetlanie listy 5 warstwy usług został zaktualizowany tak, aby nie używał już stan modelu. Zamiast tego używa dowolnej klasy, która implementuje interfejs IValidationDictionary.

**Wyświetlanie listy 5 - Models\ProductService.vb (całkowicie niezależna)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

Interfejs IValidationDictionary jest zdefiniowany w 6 wyświetlania. Ten prosty interfejs ma jedną metodę i jednej właściwości.

**Wyświetlanie listy 6 - Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

Klasa w 7 wyświetlania o nazwie klasy ModelStateWrapper implementuje interfejs IValidationDictionary. Można utworzyć wystąpienia klasy ModelStateWrapper przez przekazanie słownik stanów modelu do konstruktora.

**Wyświetlanie listy 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Ponadto zaktualizowane kontrolera w wyświetlania 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jego konstruktora.

**Wyświetlanie listy 8 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

Używanie IValidationDictionary interfejsu i klasa ModelStateWrapper umożliwia całkowicie rozdzielanie naszych warstwy usług z naszych warstwy kontrolera. Warstwy usług nie jest już zależny od stanu modelu. Można przekazać każda klasa implementująca interfejs IValidationDictionary do warstwy usług. Na przykład w aplikacji WPF mogą implementować interfejs IValidationDictionary z klasa prostych kolekcji.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka został omówimy jeden ze sposobów przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. W tym samouczku przedstawiono sposób Przenieś wszystkie logiki sprawdzania poprawności z kontrolerami poza i do warstwy oddzielne usługi. Przedstawiono również sposób wyizolować z warstwy usług z warstwą kontrolera, tworząc klasę ModelStateWrapper.

>[!div class="step-by-step"]
[Poprzednie](validating-with-the-idataerrorinfo-interface-vb.md)
[dalej](validation-with-the-data-annotation-validators-vb.md)
