---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Sprawdzanie poprawności w składniku ASP.NET Web API modelu | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
<a name="model-validation-in-aspnet-web-api"></a>Weryfikacja modelu w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Gdy klient wysyła dane do interfejsu API sieci web, często chcesz sprawdzić poprawność danych przed wykonaniem jakiegokolwiek przetwarzania. W tym artykule pokazano, jak dodawać adnotacje do modeli, użyj adnotacji do sprawdzania poprawności danych i obsługi błędów sprawdzania poprawności w interfejsie API sieci web.

## <a name="data-annotations"></a>Adnotacji danych

W interfejsie API sieci Web ASP.NET, można użyć atrybutów z [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, aby ustawić reguły sprawdzania poprawności dla właściwości w modelu. Należy wziąć pod uwagę następujące modelu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Weryfikacja modelu przypadku użycia w aplikacji ASP.NET MVC, to powinna wyglądać znajomo. **Wymagane** atrybutu informacją, że `Name` właściwość nie może mieć wartości null. **Zakres** atrybutu informacją, że `Weight` musi należeć do zakresu od 0 do 999.

Załóżmy, że klient wysyła żądanie POST z następujących reprezentacja JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Widać, że klient nie zawiera `Name` właściwość, która jest oznaczona jako wymagana. Gdy interfejs API sieci Web konwertuje JSON do `Product` wystąpienia, weryfikuje `Product` atrybutów sprawdzania poprawności. W akcji kontrolera można sprawdzić, czy model jest prawidłowy:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Weryfikacja modelu nie gwarantuje, że dane klienta są bezpieczne. W innych warstwy aplikacji mogą być wymagane dodatkowe sprawdzenie poprawności. (Na przykład warstwa danych może wymuszać ograniczenia klucza obcego). Samouczek [przy użyciu interfejsu API sieci Web z programu Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) Eksploruje niektórych z tych problemów.

**"Niepełnej publikowanie"**: publikowanie niepełnego się stanie, gdy klient powoduje, że niektóre właściwości. Na przykład załóżmy, że klient wysyła następujące czynności:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

W tym miejscu klienta nie określono wartości dla `Price` lub `Weight`. Program formatujący JSON przypisuje wartość domyślną równą zero, aby brakuje właściwości.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Stan modelu jest prawidłowa, ponieważ zero jest nieprawidłową wartością dla tych właściwości. Czy jest to problem, zależy od danego scenariusza. Na przykład w przypadku operacji aktualizacji możesz chcieć rozróżnienia "zero" i "nieustawiona." Wymuszanie klientów, aby ustawić wartość, aby właściwość nullable, ustaw **wymagane** atrybutu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Zbyt księgowej"**: klient może także wysłać *więcej* danych niż oczekiwano. Na przykład:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

W tym miejscu JSON zawiera właściwość ("Color"), która nie istnieje w `Product` modelu. W takim przypadku program formatujący JSON po prostu ignoruje tę wartość. (Element formatujący XML ma taki sam). Publikowanie uwierzytelniając powoduje występowanie problemów, jeśli model ma właściwości, które mają być tylko do odczytu. Na przykład:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Nie chcesz, aby użytkownikom aktualizowanie `IsAdmin` właściwości i podnieść się administratorom! Najbezpieczniejszą strategii jest używanie klasy modelu, która dokładnie odpowiada, do których klient może wysłać:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Wpis w blogu Brada Wilsona "[vs sprawdzania poprawności danych wejściowych. Model weryfikacji w aplikacji ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"zawiera Obszerne omówienie niepełnego publikowanie i publikowanie nadmierne. Mimo że wpis o ASP.NET MVC 2 problemów są nadal istotne dla interfejsu API sieci Web.


## <a name="handling-validation-errors"></a>Obsługa błędów sprawdzania poprawności

Interfejs API sieci Web nie automatycznie zwraca błąd do klienta podczas sprawdzania poprawności zakończy się niepowodzeniem. Jest akcji kontrolera, aby sprawdzić stan modelu i reagowania.

Można również utworzyć filtr akcji, aby sprawdzić stan modelu przed wywołaniu akcji kontrolera. Poniższy kod przedstawia przykład:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

W przypadku niepowodzenia weryfikacji modelu ten filtr zwraca odpowiedź HTTP, który zawiera błędy weryfikacji. W takim przypadku nie jest wywoływany akcji kontrolera.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Aby zastosować filtr do wszystkich kontrolerów interfejsu API sieci Web, należy dodać wystąpienia filtr, aby **HttpConfiguration.Filters** kolekcji podczas konfigurowania:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Innym rozwiązaniem jest, aby ustawić filtr jako atrybut na poszczególnych kontrolerach lub akcji kontrolera:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
