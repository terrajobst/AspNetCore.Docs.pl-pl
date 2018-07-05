---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Wskazówki dotyczące zabezpieczeń dla wzorca ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 72aee37e715fa11ccfe5d02eef4e450dd747095a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378034"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Wskazówki dotyczące zabezpieczeń dla wzorca ASP.NET Web API 2 OData
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pośrednictwem interfejsu OData.

## <a name="edm-security"></a>Zabezpieczenia EDM

Semantyki zapytań są oparte na modelu entity data model (EDM) struktury, nie podstawowych typów modelu. Właściwości można wykluczyć z EDM i nie będą widoczne dla zapytania. Na przykład załóżmy, że model zawiera typ pracownika z właściwością wynagrodzenia. Możesz chcieć wyłączyć tę właściwość z EDM, aby go ukryć od klientów.

Istnieją dwa sposoby wyłączają właściwość EDM. Możesz ustawić **[IgnoreDataMember]** atrybutu dla właściwości w klasie modelu:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Możesz również usunąć właściwość z EDM programowe:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Zabezpieczenie zapytania

Złośliwego lub prostego klienta można utworzyć zapytanie, które bardzo długi czas do wykonania. W najgorszym przypadku może to zakłócać dostęp do usługi.

**[Queryable]** atrybut jest filtr akcji, która analizuje, weryfikuje i stosuje zapytanie. Filtr konwertuje opcje zapytania w wyrażeniu LINQ. Gdy zwraca kontrolera OData **IQueryable** typu **IQueryable** dostawcy LINQ konwertuje wyrażenie LINQ do kwerendy. W związku z tym wydajność zależy od dostawcy LINQ, która jest używana, a także na konkretnej właściwości schemat zestawu danych lub bazy danych.

Aby uzyskać więcej informacji o używaniu opcji zapytania protokołu OData w interfejsie API sieci Web platformy ASP.NET, zobacz [obsługi opcji zapytania OData](supporting-odata-query-options.md).

Jeśli wiesz, że wszyscy klienci są zaufane (na przykład w środowisku przedsiębiorstwa) lub zestaw danych jest małe, wydajność zapytań nie może być problem. W przeciwnym razie należy rozważyć poniższe zalecenia.

- Testowanie usługi z różnych zapytań i profilowanie bazy danych.
- Włączanie stronicowania opartych na serwerze uniknąć zwrócenie dużych zestawów danych w jednym zapytaniu. Aby uzyskać więcej informacji, zobacz [stronicowania Server-Driven](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Potrzebujesz $filter i $orderby? Niektóre aplikacje mogą zezwolić na klientów, stronicowania, przy użyciu $top i $skip, ale wyłącz innymi opcjami zapytania. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Należy rozważyć ograniczenie $orderby do właściwości w indeksie klastrowanym. Sortowanie dużej ilości danych bez indeksu klastrowanego trwa długo. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Maksymalna dopuszczalna liczba węzłów: **MaxNodeCount** właściwość **[Queryable]** ustawia maksymalnej liczby węzłów w drzewie składni $filter dozwolone. Wartość domyślna to 100, ale warto ustawić niższą wartość, ponieważ dużą liczbę węzłów może być powolne skompilować. Jest to szczególnie istotne w przypadku korzystania z LINQ to Objects (czyli zapytań LINQ w kolekcji w pamięci, bez użycia pośrednie dostawcy LINQ). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Rozważ wyłączenie funkcji funkcja any() i all(), ponieważ mogą one być powolne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Jeśli wszystkie właściwości parametrów zawierają dużych ciągów & #8212for przykład, opis produktu lub wpis w blogu & #8212consider wyłączenie funkcji ciągów. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Należy wziąć pod uwagę, nie można przydzielać filtrowania według właściwości nawigacji. Filtrowanie według właściwości nawigacji może spowodować sprzężenie, która może być powolne, w zależności od schematu bazy danych. Poniższy kod przedstawia moduł weryfikacji zapytania, który uniemożliwia filtrowania według właściwości nawigacji. Aby uzyskać więcej informacji na temat modułów weryfikacji zapytań zobacz [sprawdzanie poprawności zapytań](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Należy rozważyć ograniczenie zapytania $filter, pisząc modułu sprawdzania poprawności, który jest dostosowany do bazy danych. Na przykład należy wziąć pod uwagę te dwa zapytania: 

  - Wszystkie filmy z aktorów, których nazwisko zaczyna się od "A".
  - Wydane w 1994 r. wszystkie filmy.

    Chyba że filmy są indeksowane przez podmioty, pierwsze zapytanie może wymagać aparat bazy danych w celu skanowania całą listę filmów. Drugie zapytanie może być akceptowalne, przyjmuje filmy są indeksowane przez rok wydania.

    Poniższy kod przedstawia modułu sprawdzania poprawności, który umożliwia filtrowanie na właściwości "ReleaseYear" i "Title", ale żadne inne właściwości.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Ogólnie rzecz biorąc należy wziąć pod uwagę jakie funkcje $filter, potrzebujesz. Jeśli klienci pełną wyrazistość $filter nie jest konieczne, można ograniczyć dozwolonych funkcji.
