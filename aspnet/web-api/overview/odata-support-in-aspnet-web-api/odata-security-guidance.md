---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Wskazówki dotyczące zabezpieczeń dla składnika ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868711"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Wskazówki dotyczące zabezpieczeń dla składnika ASP.NET Web API 2 OData
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy rozważyć w przypadku ujawnienia zestawu danych za pośrednictwem OData.

## <a name="edm-security"></a>EDM zabezpieczeń

Semantyki zapytań są oparte na modelu danych jednostki (EDM), nie podstawowych typów modeli. Właściwość można wykluczyć z EDM i nie będą widoczne w zapytaniu. Na przykład załóżmy, że model zawiera typ pracownika z właściwością wynagrodzenia. Można wykluczyć tę właściwość z modelu EDM, aby je ukryć od klientów.

Istnieją dwa sposoby wyłączają właściwość z modelu EDM. Można ustawić **[IgnoreDataMember]** atrybutu we właściwości w klasie modelu:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Można również usunąć właściwość z EDM programowo:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Zapytanie zabezpieczeń

Klient złośliwego lub prostego można utworzyć zapytanie, które bardzo długi czas do wykonania. W najgorszym przypadku to przerwać dostęp do usługi.

**[Queryable]** atrybut jest filtr akcji, która analizuje, weryfikuje i stosuje zapytanie. Filtr konwertuje opcje zapytania w wyrażeniu LINQ. Gdy kontroler OData zwraca **IQueryable** typu **IQueryable** dostawcy LINQ Konwertuje wyrażenia LINQ na kwerendę. W związku z tym wydajności zależy od dostawcy LINQ, który jest używany, a także na specyfiki schemat zestawu danych lub bazy danych.

Aby uzyskać więcej informacji o używaniu opcji zapytania OData w interfejsie API sieci Web ASP.NET, zobacz [obsługi opcji zapytania OData](supporting-odata-query-options.md).

Jeśli wiesz, że wszyscy klienci są zaufane (na przykład w środowisku przedsiębiorstwa) lub zestawu danych jest mały, wydajność zapytań nie może być problem. W przeciwnym razie należy rozważyć poniższe zalecenia.

- Testowanie usługi z różne zapytania i profilu bazy danych.
- Włącz stronicowanie oparte na serwerze uniknąć zwrócenie dużych zestawów danych w jednym zapytaniu. Aby uzyskać więcej informacji, zobacz [stronicowania Server-Driven](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Potrzebujesz $filter i $orderby? Niektóre aplikacje mogą umożliwić klienckim stronicowania, przy użyciu $top i $skip, ale wyłącz inne opcje zapytania. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Należy rozważyć ograniczenie $orderby do właściwości w indeks klastrowany. Sortowanie dużej ilości danych bez indeksu klastrowanego jest powolne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Liczba węzłów maksymalna: **właściwość MaxNodeCount** właściwość **[Queryable]** Ustawia maksymalną liczba węzłów dozwolone w drzewie składni $filter. Wartość domyślna to 100, ale można ustawić niższą wartość, ponieważ dużej liczby węzłów może działać powoli skompilować. Jest to szczególnie istotne w przypadku korzystania z LINQ do obiektów (np. zapytań LINQ w kolekcji w pamięci, bez korzystania z pośredniego dostawcy LINQ). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Rozważ wyłączenie funkcji any() i all(), jak mogą być powolne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Jeśli wszystkie właściwości ciągu zawiera dużych ciągów & przykład #8212for, opis produktu lub wpis w blogu & #8212consider wyłączenie funkcji ciągów. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Należy wziąć pod uwagę, brak zezwolenia filtrowanie właściwości nawigacji. Filtrowanie właściwości nawigacji może spowodować sprzężenia, który może być wolne w zależności od schematu bazy danych. Poniższy kod przedstawia moduł weryfikacji zapytania, który uniemożliwia filtrowanie właściwości nawigacji. Aby uzyskać więcej informacji o moduły weryfikacji zapytań, zobacz [weryfikacji zapytań](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Należy rozważyć ograniczenie zapytania $filter pisząc modułu sprawdzania poprawności, który jest dostosowany do bazy danych. Na przykład wziąć pod uwagę następujące dwa zapytania: 

  - Wszystkie filmy z złośliwych użytkowników, których nazwisko zaczyna się od "A".
  - Wszystkie filmy wydane w 1994 r.

    Chyba, że filmy są indeksowane przez złośliwych użytkowników, pierwszego zapytania mogą wymagać aparat bazy danych, aby przeprowadzić skanowanie całą listę filmów. Drugiego zapytania mogą być akceptowane, filmy przyjmuje są indeksowane według roku wydania.

    Poniższy kod przedstawia modułu sprawdzania poprawności, który umożliwia filtrowanie właściwości "ReleaseYear" i "Title", ale ma inne właściwości.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Ogólnie rzecz biorąc należy wziąć pod uwagę jakie potrzebne funkcje $filter. Jeśli klienci nie potrzebują pełnego wyrazistość z $filter, można ograniczyć dozwolonych funkcji.
