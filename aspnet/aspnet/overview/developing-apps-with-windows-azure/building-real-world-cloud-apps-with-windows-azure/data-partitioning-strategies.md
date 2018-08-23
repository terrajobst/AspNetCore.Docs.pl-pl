---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: (Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) strategie partycjonowania danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 1a19b5b8ce47439c39359a805e16ab4e7631deec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754626"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>(Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) strategie partycjonowania danych
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacje na temat serii, zobacz [pierwszy rozdział](introduction.md).


Wcześniej widzieliśmy się, jak łatwo jest skalowanie warstwy internetowej, aplikacji w chmurze, dodając i usuwając serwerów sieci web. Ale jeśli one występują wszystkie tego samego magazynu danych, wąskich gardeł w aplikacji, zostanie przeniesiony z frontonu do zaplecza, a warstwa danych jest najtrudniejsze skalowania. W tym rozdziale przedstawiony sposób zapewniania warstwie danych skalowalne, dzieląc dane na wiele relacyjnych baz danych lub łącząc magazynu relacyjnej bazy danych w innych opcjach magazynu danych.

Konfigurowanie schematu partycjonowania jest najlepsze gotowe się frontonu z tego samego powodu, o których wspomniano wcześniej: bardzo trudno zmienić swoją strategię magazynu danych, po aplikacji znajduje się w środowisku produkcyjnym. Należy rozważać twardych na początku różne metody, można uniknąć, posiadające "Twitter momencie" aplikacja ulegnie awarii lub ulegnie przez długi czas, gdy reorganizowanie danych i kod dostępu do danych aplikacji.

## <a name="the-three-vs-of-data-storage"></a>Trzy Vs magazynu danych

W celu ustalenia, czy potrzebujesz strategii partycjonowania i co powinno być, należy wziąć pod uwagę trzy pytania dotyczące danych:

- Wolumin — jak dużo danych będziesz ostatecznie przechowywać? Kilka gigabajtów? Kilka gigabajtów kilkuset? Terabajty? Petabajtów?
- Szybkość pracy — co to szybkość, z jaką dane będzie się zwiększać? Jest aplikacją wewnętrzną, która nie jest generowanie dużej ilości danych? Zewnętrznych aplikacji, którą klienci będą przekazywania, obrazy i filmy wideo do?
- Różne — typ danych będzie można przechowywać? Relacyjne, obrazy, pary klucz wartość i grafów społecznościowych?

Jeśli uważasz, że użytkownik chce mieć wiele woluminów, szybkość pracy lub różnych, masz zastanów się, jakiego rodzaju schematu partycjonowania najlepiej umożliwi aplikacji do skalowania wydajnie i skutecznie je wraz ze wzrostem, i aby upewnić się, nie wystąpią jakieś.

Istnieją trzy sposoby partycjonowania:

- Partycjonowanie pionowe
- Partycjonowanie poziome
- Partycjonowanie hybrydowe

## <a name="vertical-partitioning"></a>Partycjonowanie pionowe

Pionowe momentu porcjowania jest podobne do podziału tabeli według kolumn: jeden zestaw kolumn przechodzi do jednego magazynu danych, a inny zestaw kolumn przechodzi w innym magazynem danych.

Na przykład załóżmy, że Moja aplikacja przechowuje dane o osobach, w tym obrazów:

![Tabela danych](data-partitioning-strategies/_static/image1.png)

Po reprezentują te dane jako tabelę i spójrz na różnych odmian danych, można zobaczyć, że trzy kolumny z lewej strony ma dane ciągów, które mogą być skutecznie przechowywane przez relacyjnej bazy danych, dwie kolumny po prawej stronie są zasadniczo tablice typu byte tego języka c niektó z plików obrazu. Istnieje możliwość danych pliku obrazu magazynu relacyjnej bazy danych, a wiele osób to zrobić, ponieważ nie chcesz zapisać dane do systemu plików. Mogą nie mieć system plików można przechowywać wymagane ilości danych lub może nie chcą zarządzać oddzielne tworzenie kopii zapasowych i przywracanie systemu. Ta metoda działa dobrze w przypadku lokalnych baz danych i małe ilości danych w bazach danych w chmurze. W środowisku lokalnym może być łatwiejsze, umożliwiające po prostu DBA zajmie się wszystkiego.

Ale w bazie danych w chmurze, Magazyn jest relatywnie kosztowne, a dużą liczbę obrazów może spowodować, że rozmiar bazy danych poza granicami, w których on mogą działać wydajnie. Rozwiązać te problemy przez partycjonowanie danych w pionie, co oznacza, że wybierz najbardziej odpowiedniego magazynu danych dla każdej kolumny w tabeli danych. Co może być najlepszej metody dla tego przykładu polega na umieszczeniu danych ciągu w relacyjnej bazie danych i obrazów w usłudze Blob storage.

![Tabela danych partycje pionowe](data-partitioning-strategies/_static/image2.png)

Przechowywanie obrazów w magazynie obiektów Blob zamiast bazy danych jest praktyczniejsze w chmurze niż w środowisku lokalnym, ponieważ nie trzeba martwić się o Konfigurowanie serwerów plików i zarządzaniu nim kopii zapasowych i przywracania danych przechowywanych poza relacyjnej bazy danych: wszystkie które jest obsługiwane dla Ciebie automatycznie przez usługi Blob storage.

Jest to metoda partycjonowania wprowadziliśmy w aplikacji naprawić, a Spróbujemy znaleźć kod w [rozdział magazynu obiektów Blob](unstructured-blob-storage.md). Bez ten schemat partycjonowania przy założeniu, że średni rozmiar 3 MB aplikacja naprawić będzie można tylko do przechowywania około 40 000 zadań przed osiągnięcia maksymalny rozmiar bazy danych programu 150 GB. Po usunięciu obrazów, bazy danych można przechowywać 10 razy jako wiele zadań; Możesz przejść znacznie dłużej przed musisz myśleć o implementowaniu poziomy schematu partycjonowania. I zgodnie ze skalą aplikacji, wydatków zwiększanie wolniej, ponieważ większość potrzeb dotyczących magazynu są przesyłane do bardzo niedrogiego magazynu obiektów Blob.

## <a name="horizontal-partitioning-sharding"></a>Partycjonowanie poziome (fragmentowanie)

Poziomy momentu porcjowania jest podobne do podziału tabeli według wierszy: jeden zestaw wierszy jest przenoszony do jednego magazynu danych, a innego zestawu wierszy przechodzi w innym magazynem danych.

Biorąc pod uwagę tego samego zestawu danych, innym rozwiązaniem jest przechowywać różne zakresy nazw klienta w różnych bazach danych.

![Tabela danych podzielone na partycje w poziomie](data-partitioning-strategies/_static/image3.png)

Należy zachować ostrożność bardzo schemat fragmentowania, aby upewnić się, że data jest równomiernie rozłożona w celu uniknięcia punktów aktywnych. Ten prosty przykład przy użyciu pierwszej litery nazwiska nie spełnia tego wymagania, ponieważ wiele osób ma ostatni o nazwach zaczynających niektórych typowych litery. Czy osiągasz wcześniej, niż można by oczekiwać, ponieważ niektóre bazy danych może stać się bardzo duże, podczas gdy większość pozostanie małych ograniczenia rozmiaru tabeli.

Dół boku partycjonowanie poziome to, że może być trudny do zapytań dla wszystkich danych. W tym przykładzie zapytanie musi ma być rysowany od 26 do różnych baz danych do wszystkich danych przechowywanych przez aplikację.

## <a name="hybrid-partitioning"></a>Partycjonowanie hybrydowe

Partycjonowanie poziome i pionowe można łączyć. Można na przykład w przykładowych danych obrazy są przechowywane w magazynie obiektów Blob i poziomie partycjonowanie danych ciągu.

![Hybrydowe tabeli dane podzielone na partycje](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partycjonowanie aplikacji produkcyjnej

Koncepcyjnie jest łatwo sprawdzić działanie schematu partycjonowania, ale dowolnego schematu partycjonowania spowodują wzrost złożoności kodu i wprowadzono wiele nowych komplikacji, które mają do czynienia z. Jeśli przenosisz obrazów do magazynu obiektów blob, co możesz zrobić, gdy Usługa magazynu nie działa? Jak obsługiwać zabezpieczeń obiektu blob? Co się stanie, jeśli brak synchronizacji bazy danych i blob storage? W przypadku fragmentowania, jak będzie można obsługiwać zapytań dla wszystkich baz danych?

Komplikacji są zarządzane tak długo, jak planowane jest ich przed przejściem do produkcji. Wielu użytkowników, których nie chcesz, aby mieli oni później. Średnio nasz zespół Customer Advisory Team (CAT) pobiera panicked połączeń telefonicznych około raz na miesiąc od klientów, których aplikacje są mocno rozwijać przebiega bardzo duże, a nie zrobił to planowanie. I podobny mówią: "Pomoc! Umieść wszystko w jednym magazynie danych, i w ciągu 45 dni zamierzam zabraknie miejsca na nim!" A jeśli masz wiele logikę biznesową, wbudowane, jak dostęp do usługi magazynu danych i masz klienci, którzy korzystają z aplikacji, to nie dobry moment, aby przejść w dół na dzień podczas migrowania. Firma Microsoft znajdą się pośrednictwa herculean działań mających na celu pomóc partycji klienta swoje dane na bieżąco z krótkim czasie w dół. To bardzo ekscytujący i bardzo scary i nie mielibyśmy mieć czegoś chcesz brać udział w przypadku nie trzeba będzie ją! Myśl o tym na początku i integrowanie aplikacji będzie ułatwiać o wiele prostsze, jeśli aplikacja powiększa się później.

## <a name="summary"></a>Podsumowanie

Skuteczne schematu partycjonowania można włączyć skalowanie do petabajtów danych w chmurze bez wąskie gardła aplikacji w chmurze. I nie ma na początku płacić za maszyny dużych lub rozległe infrastruktury, jak to się dzieje w przypadku uruchomienia aplikacji w centrum danych w środowisku lokalnym. W chmurze możesz stopniowo dodawać pojemność zgodnie z ich potrzebujesz, i płacić one tylko dla jak używasz Jeśli zostanie on użyty.

W [następny rozdział](unstructured-blob-storage.md) zobaczymy, jak aplikacja naprawić implementuje partycjonowanie pionowe, zapisując obrazy w usłudze Blob storage.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat strategii partycjonowania zobacz następujące zasoby.

Dokumentacja:

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument — Markiem Simmsem i Michael Thomassy.
- [Microsoft Patterns and Practices — wzorce projektowe oparte na chmurze](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące partycjonowania danych, wzorzec fragmentowania.

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Dziewięć serii Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Zobacz Omówienie partycjonowania w odcinku 7.
- [Tworzenie Big: Wnioski wyciągnięte z klientów usługi Windows Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). — Markiem Simmsem w tym artykule omówiono poszczególne schematy partycjonowania, strategie fragmentowania, jak zaimplementować dzielenia na fragmenty i Federacji bazy danych SQL, zaczynając od 19:49. Podobnie jak przed uszkodzeniami serii, ale zbliża się bardziej szczegółowe informacje z instrukcjami.

Przykładowy kod:

- [Podstawy usługi w systemie Windows Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja, która zawiera bazę danych podzielonych na fragmenty. Aby uzyskać opis fragmentowanie zaimplementowane, zobacz [DAL — fragmentowanie danych RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) na blogu Windows Azure.

> [!div class="step-by-step"]
> [Poprzednie](data-storage-options.md)
> [dalej](unstructured-blob-storage.md)
