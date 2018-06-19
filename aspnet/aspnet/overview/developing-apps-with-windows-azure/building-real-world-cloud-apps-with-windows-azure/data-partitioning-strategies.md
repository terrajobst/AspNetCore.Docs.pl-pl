---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Dane partycjonowania strategie (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 9ff7f37a03d8d3dfab50e8007a6645bb0d88f453
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871353"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Dane partycjonowania strategie (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy rozdział](introduction.md).


Widzieliśmy wcześniej, jak łatwo jest skalowanie warstwy sieci web aplikacji w chmurze, przez dodawanie i usuwanie serwerów sieci web. Jednak jeśli one są wszystkie naciśnięcie tego samego magazynu danych, aplikacji wąskie gardło będące przyczyną są przenoszone z frontonu do zaplecza i jest najtrudniejsze skalowania warstwy danych. W tym rozdziale opisano, jak można utworzyć warstwę danych skalowalne przez partycjonowanie danych do wielu relacyjnych baz danych, lub łącząc relacyjnej bazy danych magazynu z innymi opcjami magazynu danych.

Konfigurowanie schemat partycjonowania najlepiej gotowe zapasowej przodu z tego samego powodu wymienionych poniżej: jest bardzo trudne zmienić strategii magazynu danych po aplikacji w środowisku produkcyjnym. Jeśli myślisz twardych góry o różne podejścia, można uniknąć, o "Twitter momencie" aplikacja ulegnie awarii lub przestanie działać przez dłuższy czas podczas reorganizować danych i kod dostępu do danych aplikacji.

## <a name="the-three-vs-of-data-storage"></a>Trzy Vs magazynu danych

W celu ustalenia, czy potrzebujesz strategii partycjonowania i jakie powinny być, rozważ pytania dotyczące danych:

- Wolumin — jak dużo danych będzie można przechowywać dane ostatecznie? Kilka gigabajtów? Kilka gigabajtów kilkuset? Terabajtów? Petabajtów?
- Szybkość pracy — co to szybkość, z jaką wzrośnie danych? Jest to wewnętrzny aplikację, która nie jest generowanie dużą ilość danych? Aplikacja zewnętrznych klienci będą przekazywania, obrazy i klipy wideo do?
- Różne — typ danych będzie można zapisać? Relacyjna, obrazy, pary klucz wartość, wykresy społecznościowymi?

Jeśli uważasz, że użytkownik chce wiele woluminów, prędkość lub różnych, należy rozważyć, jakiego rodzaju schemat partycjonowania najlepiej spowoduje włączenie aplikacji do skalowania skutecznego jako ich przyrostu i zapewnienie nie wystąpiły wąskie gardła.

Istnieją trzy sposoby partycjonowania:

- Partycjonowanie pionowe
- Partycjonowanie pozioma
- Partycjonowanie hybrydowego

## <a name="vertical-partitioning"></a>Partycjonowanie pionowe

Pionowy momentu porcjowania jest podobne do podziału tabeli według kolumn: jeden zestaw kolumn przechodzi w jednym magazynie danych, i przejdzie do innego zestawu kolumn do magazynu danych.

Na przykład załóżmy, że Moja aplikacja przechowuje dane dotyczące osób, w tym obrazów:

![Tabela danych](data-partitioning-strategies/_static/image1.png)

Po przedstawienia tych danych jako tabelę i przyjrzyj się różnych odmian danych, można zobaczyć, że trzy kolumny po lewej stronie mają ciągu danych, które mogą być skutecznie przechowywane przez relacyjnej bazy danych, gdy dwie kolumny po prawej stronie są zasadniczo tablice typu byte tego c ome z plików obrazów. Istnieje możliwość przechowywania danych pliku obrazu relacyjnej bazy danych, a wiele osób to zrobić, ponieważ nie chcesz zapisać dane w systemie plików. Mogą nie mieć możliwość przechowywania wymagane ilości danych systemu plików lub może nie chcą zarządzać oddzielnych kopii zapasowych i przywracanie systemu. Ta metoda działa także dla lokalnych baz danych i niewielkich ilości danych w bazach danych w chmurze. W środowisku lokalnym może być łatwiej właśnie let DBA automatyzującą wszystkie elementy.

Ale w bazie danych w chmurze, magazynu jest stosunkowo drogie i dużą liczbę obrazów może spowodować rozmiar bazy danych poza granicami, w których aplikacja może działać wydajnie. Rozwiązania tych problemów podział danych w pionie, co oznacza, że możesz wybrać najodpowiedniejszy magazynu danych dla każdej kolumny w tabeli danych. Co może działa najlepiej w tym przykładzie jest umieszczenie danych ciągu w relacyjnej bazie danych i obrazów w magazynie obiektów Blob.

![Tabela danych w pionie na partycje](data-partitioning-strategies/_static/image2.png)

Przechowywania obrazów w magazynie obiektów Blob, a nie bazy danych jest bardziej praktyczne w chmurze niż w środowisku lokalnym, ponieważ nie trzeba martwić Konfigurowanie serwerów plików lub zarządzania kopii zapasowych i przywracania danych przechowywane poza relacyjna baza danych: wszystkie który jest już obsługiwane automatycznie przez usługę magazynu obiektów Blob.

Jest to metoda partycjonowania wprowadziliśmy w aplikacji napraw i przyjrzymy kod w tym w [rozdział magazynu obiektów Blob](unstructured-blob-storage.md). Bez ten schemat partycjonowania przy założeniu, że średni rozmiar 3 MB aplikacji naprawić tylko będzie można przechowywać około 40 000 zadań przed szukaniem maksymalny rozmiar bazy danych gigabajtów 150. Po usunięciu obrazów, bazy danych można przechowywać 10 razy jako wiele zadań; można przejść dłużej przed trzeba myśleć o implementacji poziomy schemat partycjonowania. I jako skalowanie aplikacji, wydatków wzrostu wolniej, ponieważ zbiorczego zapotrzebowanie na pamięć będzie do niedrogie magazynu obiektów Blob.

## <a name="horizontal-partitioning-sharding"></a>Poziomego podziału (dzielenia na fragmenty)

Poziomy momentu porcjowania jest podobne do podziału tabeli wiersze: jeden zestaw wierszy przechodzi w jednym magazynie danych, i przejdzie do innego zestawu wierszy do magazynu danych.

Podana ten sam zestaw danych, innym rozwiązaniem byłoby przechowywać różne zakresy nazw użytkowników w różnych baz danych.

![Poziomo podzielona na partycje tabeli danych](data-partitioning-strategies/_static/image3.png)

Należy ostrożnie schematem dzielenia na fragmenty, aby upewnić się, że data jest rozmieszczana równomiernie w celu uniknięcia punkty aktywne. Ten prosty przykład przy użyciu pierwszą literę nazwisko nie spełniają to wymaganie, ponieważ ostatnia o nazwach zaczynających niektórych typowych liter ma wiele osób. Czy wcześniej, niż może spodziewać się, ponieważ niektóre bazy danych będą bardzo duże podczas większość pozostanie małych osiągnęła ograniczenia rozmiaru tabeli.

Dół strony poziomych partycji jest, czy może być trudny do zapytania dla wszystkich danych. W tym przykładzie zapytanie musi ma być rysowany od 26 do różnych baz danych do wszystkich danych przechowywanych przez aplikację.

## <a name="hybrid-partitioning"></a>Partycjonowanie hybrydowego

Możesz łączyć poziome i pionowe partycjonowania. Można na przykład w przykładowe dane obrazy są przechowywane w magazynie obiektów Blob i poziomie partycjonowania danych ciągu.

![Dane tabeli hybrydowego podzielona na partycje](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partycjonowanie aplikacji produkcyjnej

Koncepcyjnie jest łatwo sprawdzić, jak schemat partycjonowania będzie działać, ale wszelkie schemat partycjonowania zwiększenia złożoności kodu i wprowadzono wiele nowych komplikacji, które masz na wypadek. Jeśli przenosisz obrazów do magazynu obiektów blob, co zrobić, gdy działa Usługa magazynu? Sposób obsługi zabezpieczeń obiektów blob? Co się stanie, jeśli magazyn bazy danych i obiektów blob zsynchronizowane? W przypadku dzielenia na fragmenty, jak możesz obsłuży zapytań dla wszystkich baz danych?

Komplikacji są łatwe w zarządzaniu tak długo, jak planowane jest dla nich przed przejściem do środowiska produkcyjnego. Wiele osób, które nie są, które chcesz, aby miały później. Średnia nasz zespół Advisory zespołu klienta (CAT) pobiera panicked połączeń telefonicznych o raz w miesiącu od klientów, których aplikacje są startu w taki sposób, duże i nie zrobił to planowanie. I podają wyglądać mniej więcej tak: "Help! Umieść wszystko w jednym magazynie danych, a w ciągu 45 dni użyjemy całe miejsce na nim!" I jeśli masz wiele logiki biznesowej, wbudowane dostęp do Twojego magazynu danych i masz klientów, którzy korzystają z aplikacji, nie ma żadnych odpowiedni moment, aby go w dół na dzień, podczas migracji. Firma Microsoft przechodzili pośrednictwa wysiłków herculean ułatwiające partycji klienta swoje dane na bieżąco, czas nie przestoju. Jest bardzo atrakcyjne i bardzo przerażenie i coś nie chcesz brać udział w Jeśli można go uniknąć! Pomyśleć o to góry i integrowanie aplikacji będzie ułatwiać znacznie łatwiejsze, jeśli aplikacja rozwoju później.

## <a name="summary"></a>Podsumowanie

Skuteczne schemat partycjonowania można włączyć aplikacji chmury można skalować do poziomu petabajtów danych w chmurze bez wąskich gardeł. I nie trzeba płacić góry ogromną maszyny lub szeroką infrastrukturę, jak mogą podczas uruchamiania aplikacji w lokalnym centrum danych. W chmurze można przyrostowo można dodać pojemności, jak potrzebne, i tylko w przypadku płatności dla jak używasz używając go.

W [następnego rozdziału](unstructured-blob-storage.md) zajmiemy się tym, jak aplikacji Usuń implementuje partycjonowanie pionowe przez zapisanie obrazów w magazynie obiektów Blob.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat strategii partycjonowania zobacz następujące zasoby.

Dokumentacja:

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w systemie Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument przez moduły SIMM znaku i Michael Thomassy.
- [Microsoft Patterns and Practices - chmury wzorce projektowe](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz partycjonowanie danych wskazówki wzorca dzielenia na fragmenty.

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Zawiera omówienie partycjonowania w epizodu 7.
- [Big budynku: Wnioski uzyskane od klientów systemu Windows Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). Oznacz moduły SIMM omówiono partycjonowania schematy strategii dzielenia na fragmenty implementowania dzielenia na fragmenty oraz federacje bazy danych SQL, zaczynając od 19:49. Podobnie jak seria przed uszkodzeniami, ale przechodzi w szczegółowe instrukcje.

Przykładowy kod:

- [Podstawy usługi w systemie Windows Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja obsługująca oferuje podzielonej bazy danych. Opis schematu dzielenia na fragmenty zaimplementowana, zobacz [DAL — dzielenia na fragmenty RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) na blogu systemu Windows Azure.

> [!div class="step-by-step"]
> [Poprzednie](data-storage-options.md)
> [dalej](unstructured-blob-storage.md)
