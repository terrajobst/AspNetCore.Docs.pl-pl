---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opcje magazynu danych (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 16388bf00c291ee21eec7bc72a9c01c33cec9371
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820337"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opcje magazynu danych (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Większość osób służą do relacyjnych baz danych i charakteryzują się przeoczyć innych opcjach magazynu danych, gdy ich projektowania aplikacji w chmurze. Może to spowodować nieoptymalne wydajność, wysoką wydatków lub co gorsza, ponieważ [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (nierelacyjnej) baz danych może obsługiwać kilka zadań umożliwi bardziej wydajne niż relacyjne bazy danych. Gdy klienci zapytaj NAS, aby uzyskać pomoc w rozwiązywaniu problemu z magazynem danych krytycznych, często jest ponieważ mają one relacyjnej bazy danych gdzie jedną z opcji NoSQL będą działały lepiej. W takiej sytuacji klient byłoby lepsze rozwiązanie, jeśli były one wdrożone rozwiązanie NoSQL przed wdrożeniem aplikacji do środowiska produkcyjnego.

Z drugiej strony byłoby błędu, aby założył, że baza danych typu NoSQL można wykonać wszystkie czynności dobrze lub wystarczająco dobrze. Wybór jest niedostępny, pojedynczy najlepsze danych zarządzania dla wszystkich zadań magazynu danych; rozwiązania do zarządzania różnymi danymi są zoptymalizowane pod kątem różnych zadań. Większość aplikacji w chmurze w rzeczywistych warunkach mają różne wymagania dotyczące magazynu danych i często są udostępniane najlepsze przy użyciu kombinacji wielu rozwiązań magazynowania danych.

Ten rozdział ma na celu im szerszy poznać dostępne opcje magazynowania danych do aplikacji w chmurze, a niektóre podstawowe wskazówki dotyczące sposobu wybierz te, które dopasowania ich do scenariusza. Najlepiej należy pamiętać o dostępnych opcji i zastanów się ich zalety i słabe strony, aby opracować aplikację. Zmiana opcji przechowywania danych w aplikacji produkcyjnej może być bardzo trudne, takich jak zmiana aparatu jet, natomiast płaszczyzna jest w locie.

## <a name="data-storage-options-on-azure"></a>Opcje przechowywania danych na platformie Azure

Chmura sprawia, że stosunkowo łatwa do używania różnych relacyjnych i magazynów danych NoSQL. Poniżej przedstawiono niektóre platformy magazynu danych, dla których można użyć na platformie Azure.

![](data-storage-options/_static/image1.png)

W tabeli przedstawiono cztery rodzaje bazy danych NoSQL:

- [Bazy danych kluczy/wartości](https://msdn.microsoft.com/library/dn313285.aspx#sec7) przechowywania pojedynczy obiekt Zserializowany dla każdej wartości klucza. Są one odpowiednie do przechowywania dużych ilości danych, której chcesz uzyskać jeden element dla danej wartości klucza i nie masz do wykonywania zapytań w oparciu o inne właściwości elementu.

    [Usługa Azure Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) jest klucz/wartość bazy danych, które funkcje takie jak magazyn plików w chmurze, za pomocą wartości kluczy, które odnoszą się do nazw plików i folderów. Możesz pobrać plik przez folder i nazwę pliku, a nie wyszukiwania wartości w zawartości pliku.

    [Usługa Azure Table storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) również jest bazą danych klucz wartość. Każda wartość jest nazywana *jednostki* (podobnie jak w wierszu, który jest identyfikowany przez klucz partycji i klucz wiersza) i zawiera wiele *właściwości* (podobne do kolumn, ale nie wszystkie jednostki w tabeli muszą współużytkować ten sam kolumny). Wykonywanie zapytań w kolumnach innych niż klucz jest bardzo mało wydajne i należy ich unikać. Na przykład można przechowywać dane profilu użytkownika, z jedną partycją przechowuje informacje dotyczące pojedynczego użytkownika. Dane, takie jak nazwa użytkownika, wartość skrótu hasła, datę urodzenia i tak dalej, można przechowywać w oddzielnych właściwości jedną jednostkę lub osobne jednostki w jednej partycji. Ale nie chcesz wykonywać zapytania dla wszystkich użytkowników z danego zakresu dat urodzenia i nie można wykonać zapytanie sprzężenia między tabeli profilu, a inną tabelą. Magazyn tabel jest bardziej skalowalna i mniej kosztowne niż relacyjnej bazy danych, ale nie Włączanie, złożonych kwerend lub sprzężenia.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) baz danych klucz wartość, w których wartości są *dokumenty*. "Dokument" w tym miejscu nie jest używany w tym sensie, dokumentu programu Word lub Excel, ale oznacza kolekcję nazwanych pól i wartości, które mogą być dokumentem podrzędnych. Na przykład w tabeli historii w kolejności dokumentu zamówienia może być liczbą porządkową, Data zamówienia i pola klienta; i pole Klient może mieć pola nazwy i adresu. Baza danych koduje dane pola w formacie XML, YAML, JSON lub BSON; lub można użyć zwykłego tekstu. Jedną funkcję, która ustawia bazy danych dokumentów, oprócz bazy danych kluczy/wartości jest możliwość wykonywania zapytań w polach-key i zdefiniować indeksy pomocnicze, aby wprowadzić bardziej wydajne wykonywanie zapytań. Ta możliwość sprawia, że bazy danych dokumentów jest bardziej odpowiedni dla aplikacji, które są potrzebne do pobierania danych na podstawie kryteriów bardziej skomplikowane niż wartość klucza dokumentu. Na przykład w bazie danych dokumentów historii zamówień sprzedaży można zbadać w różnych dziedzinach, takich jak identyfikator produktu, identyfikator klienta, nazwę klienta i tak dalej. [Bazy danych MongoDB](http://www.mongodb.org/) bazą danych popularnych dokumentów.
- [Bazy danych rodzin kolumn](https://msdn.microsoft.com/library/dn313285.aspx#sec9) są klucz wartość magazynów danych, które umożliwiają przechowywanie danych struktury w kolekcjach powiązanych kolumn o nazwie rodziny kolumn. Na przykład bazę danych spisu może mieć jedną grupę kolumn dla nazwisko osoby (najpierw środkowy, ostatni), jedna grupa dla osoby adresu, a jedna grupa dla informacji o profilu osoby (podana data urodzenia, płeć, itp.). Bazy danych można następnie zapisać każda rodzina kolumn w oddzielnej partycji, przy jednoczesnym zachowaniu wszystkie dane dla jednej osoby, które dotyczą tego samego klucza. Wszystkie informacje o profilu można odczytywać, bez konieczności zapoznaj się z artykułem wszystkie nazwy i adresu informacje, jak również. [Bazy danych Cassandra](http://cassandra.apache.org/) to popularne baza danych rodzin kolumn.
- [Bazy danych grafów](https://msdn.microsoft.com/library/dn313285.aspx#sec10) informacji przechowywanych w kolekcji obiektów i relacji. Celem grafowej bazy danych jest umożliwienie aplikacji efektywnego wykonywania zapytań, które przechodzą przez sieć obiekty i relacje między nimi. Na przykład obiekty mogą być pracownicy działu zasobów ludzkich bazy danych i może być ułatwiające zapytania takie jak "Znajdź wszystkich pracowników, którzy pracują bezpośrednio lub pośrednio dla Scott." [Neo4j](http://www.neo4j.org/) jest popularnych grafowej bazy danych.

W porównaniu do relacyjnych baz danych, opcje NoSQL oferuje znacznie większej skalowalności i ekonomiczności magazynu i analizy danych bez struktury. Kosztem jest, że nie udostępniają bogaty queryability i funkcje integralności danych niezawodne relacyjnych baz danych. NoSQL będzie działać dobrze w przypadku danych dziennika usług IIS, co obejmuje dużą bez potrzeby zapytań sprzężenia. NoSQL nie będzie działać tak dobrze dla banków transakcji, która wymaga integralność danych bezwzględne i obejmuje wiele relacji z innymi danymi związanych z kontem.

Istnieje również nowszej kategorii platformy bazy danych o nazwie [NewSQL](http://en.wikipedia.org/wiki/NewSQL) skalowalność bazę danych NoSQL, łączy z queryability i integralności transakcyjnej relacyjnej bazy danych. NewSQL baz danych są przeznaczone do rozproszonej przechowywania i przetwarzania zapytań, co jest często trudno zaimplementować w bazach danych "OldSQL". [NuoDB](http://www.nuodb.com/) jest przykładem NewSQL bazy danych, które mogą być używane na platformie Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Usługi Hadoop i funkcji MapReduce

Duże ilości danych, które mogą być przechowywane w bazach danych NoSQL może być trudne do analizowania wydajnie w odpowiednim czasie. Należy użyć środowiska takiego jak [Hadoop](http://hadoop.apache.org/) który implementuje [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funkcji. Zasadniczo proces MapReduce nie jest następująca:

- Limit rozmiaru danych, który ma zostać przetworzony przez wybranie poza dane przechowywane tylko dane, które jest potrzebna do analizy. Na przykład chcesz wiedzieć korzeń podstawowej przez rok urodzenia użytkownika, musisz więc wybrać tylko urodzenia lat poza swoim magazynem danych profilu użytkownika.
- Podzielić dane na części i wyślij je do innych komputerów w celu przetworzenia. Komputer A oblicza liczbę osób, które znajdują się daty w 1950 1959, komputer B nie 1969 roku 1960, itp. Ta grupa komputerów jest nazywany *klastra Hadoop*.
- Po zakończeniu przetwarzania na części, umieść wyniki każdej części ponownie razem. Masz teraz względnie krótkich listę, jak wiele osób dla każdego rok urodzenia i zadanie obliczania wartości procentowych na tej liście ogólnego jest łatwe w zarządzaniu.

Na platformie Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) pozwala na przetwarzanie, analizowanie i wyciągaj nowe wnioski na podstawie dużych ilości danych przy użyciu możliwości usługi Hadoop. Na przykład można go użyć do analizowania dzienników serwera sieci web:

- Włącz rejestrowanie serwera sieci web do swojego konta magazynu. Spowoduje to utworzenie Azure zapisywanie dzienników usług obiektów Blob dla każdego żądania HTTP do aplikacji. Blob Service to w zasadzie magazyn plików w chmurze i dobrze integruje się z HDInsight. 

    ![Dzienniki do magazynu obiektów Blob](data-storage-options/_static/image2.png)
- Ponieważ aplikacja pobiera ruchu, dzienniki usług IIS na serwerze sieci web są zapisywane do magazynu obiektów Blob. 

    ![Dzienniki serwera sieci Web](data-storage-options/_static/image3.png)
- W portalu, kliknij przycisk **New** - **usług danych** - **HDInsight** - **szybkie tworzenie**, i określ nazwę klastra HDInsight, rozmiar klastra (liczba węzłów danych klastrów HDInsight) i nazwę użytkownika i hasło dla klastra HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Można teraz skonfigurować zadania MapReduce do analizowania dzienników i Uzyskaj odpowiedzi na pytania, takie jak:

- Jakich porach dnia Moja aplikacja pobieranie większości lub co najmniej ruch?
- Jakich krajach jest Moje ruchu przychodzącego z?
- Co to jest otoczenie średni dochód obszary, które mojego ruchu sieciowego pochodzi z. (Brak publicznego zestawu danych, który zapewnia otoczenie przychodów przy użyciu adresu IP, a który można dopasować względem adresu IP w dzienniki serwera sieci web).
- Jak jest dochodu otoczenie skorelowany określonych stron lub produktów w witrynie?

Można następnie użyć odpowiedzi na pytania, takie jak te na docelowy reklam w zależności od stopnia prawdopodobieństwa, które klient może zainteresować lub zakupu roweru konkretnego produktu.

Jak wyjaśniono w [Automatyzowanie wszystkiego rozdział](automate-everything.md), większość funkcji, które można wykonać w portalu można zautomatyzować, które zawiera, konfigurowania i wykonywania zadań analizy HDInsight. W typowym skrypcie HDInsight może zawierać następujące czynności:

- Zainicjuj obsługę klastra usługi HDInsight i połączyć go do swojego konta magazynu dla magazynu obiektów Blob w danych wejściowych.
- Przekaż pliki wykonywalne zadania MapReduce (Pliki JAR lub .exe) do klastra HDInsight.
- Prześlij MapReduce, która przechowuje dane wyjściowe do usługi Blob storage.
- Poczekaj na ukończenie zadania.
- Usuwanie klastra HDInsight.
- Dostęp do danych wyjściowych z magazynu obiektów Blob.

Przez uruchomienie skryptu, który wykonuje to wszystko, można zminimalizować ilość czasu, który zainicjowaniu obsługi klastra HDInsight, co minimalizuje Twoje koszty.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platforma jako usługa (PaaS) i infrastruktura jako usługa (IaaS)

Opcje magazynu danych, które są wymienione wcześniej obejmują Platform-as-a-Service (PaaS) i rozwiązania Infrastructure-as-a-Service (IaaS). PaaS oznacza, że możemy zarządzać infrastrukturą sprzętu i oprogramowania, a po prostu korzystać z usługi. SQL Database to funkcja PaaS platformy Azure. Możesz zadawać pytania w przypadku baz danych, a za kulisami usługi Azure konfiguruje konfiguruje maszyn wirtualnych i konfiguruje baz danych na nich. Nie mają bezpośredniego dostępu do maszyn wirtualnych i nie trzeba zarządzać nimi. IaaS oznacza, że można skonfigurować, skonfigurować i zarządzać maszyn wirtualnych, które działają w naszej infrastruktury centrum danych i umieścić dowolne na nich. Firma Microsoft zapewnia galerii wstępnie skonfigurowanych obrazów maszyn wirtualnych dla typowych konfiguracji maszyny Wirtualnej. Na przykład można zainstalować wstępnie skonfigurowanych obrazów maszyn wirtualnych dla systemu Windows Server 2008, Windows Server 2012, programu BizTalk Server, serwera Oracle WebLogic Server, Oracle Database itp.

Rozwiązania danych PaaS, które są oferowane na platformie Azure obejmują:

- Usługa Azure SQL Database (znana wcześniej jako SQL Azure). Chmura relacyjna baza danych oparta na programie SQL Server.
- Usługa Azure Table storage. Klucz/wartość bazy danych NoSQL.
- Usługa Azure Blob storage. Magazyn plików w chmurze.

W przypadku IaaS możesz uruchomić wszystko, co można załadować do maszyny Wirtualnej, na przykład:

- Relacyjne bazy danych, takich jak SQL Server, Oracle, MySQL, SQL Compact, bazy danych SQLite lub Postgres.
- Klucz wartość magazynów danych, takich jak Memcached, Redis, Cassandra i Riak.
- Kolumna magazynów danych, takich jak bazy danych HBase.
- Takich jak bazy danych MongoDB, RavenDB i CouchDB bazy danych dokumentów.
- Przykład Neo4j baz danych programu Graph.

![Opcje przechowywania danych na platformie Azure](data-storage-options/_static/image5.png)

Opcja IaaS zapewnia opcje magazynowania danych niemal nieograniczoną. Ponadto wiele z nich jest szczególnie łatwy w użyciu, ponieważ można utworzyć przy użyciu wstępnie skonfigurowanych obrazów maszyn wirtualnych. Na przykład, w portalu zarządzania wybierz **maszyn wirtualnych**, kliknij przycisk **obrazów** kartę, a następnie kliknij przycisk **Przeglądaj VM Depot**.

![Przeglądaj VM Depot](data-storage-options/_static/image6.png)

Następnie wyświetlić listę [setki wstępnie skonfigurowanych obrazów maszyn wirtualnych](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), i utworzyć Maszynę wirtualną z obrazu, który ma wstępnie system zarządzania bazami danych, takich jak bazy danych MongoDB, Neo4J, Redis, Cassandra i CouchDB:

![Bazy danych MongoDB w repozytorium VM Depot](data-storage-options/_static/image7.png)

Dzięki systemowi Azure opcje magazynowania danych IaaS tak łatwe w użyciu, ale oferty rozwiązań PaaS mają wiele zalet, które stały się bardziej ekonomiczne i praktyczne w wielu scenariuszach:

- Nie ma konieczności tworzenia maszyn wirtualnych, wystarczy użyć portalu lub skrypt do skonfigurowania magazynu danych. Jeśli chcesz, aby Magazyn danych 200 terabajtów, można po prostu kliknij przycisk lub uruchamiają polecenia, a w ciągu kilku sekund będzie gotowa do użycia.
- Nie trzeba zarządzać ani ich poprawiać maszyny wirtualne używane przez usługę; Microsoft robi to dla Ciebie automatycznie. — nie trzeba martwić się o Konfigurowanie infrastruktury, skalowania lub o wysokiej dostępności; Firma Microsoft podchodzi wszystko to za Ciebie.
- Nie musisz kupować licencji; opłat za licencje są objęte opłat za usługę.
- Płacisz tylko za rzeczywiste użycie.

Opcje przechowywania danych PaaS na platformie Azure obejmują ofert przez dostawców innych firm. Na przykład, można wybrać [dodatku MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) z Store platformy Azure, aby aprowizować bazę danych MongoDB jako usługa.

## <a name="choosing-a-data-storage-option"></a>Wybieranie opcji magazynu danych

Nie jedno z podejść jest odpowiednia w przypadku wszystkich scenariuszy. Jeśli każda osoba mówi, że ta technologia jest odpowiedź na pytanie, najpierw zapytaj jest "Co to jest pytanie?", ponieważ różne rozwiązania są zoptymalizowane do różnych rzeczy. Istnieją pewne zalety łączenia model relacyjny; Dlatego właśnie zostało wokół tak długo. Istnieją także stron w dół, do SQL Server, który może zostać zlikwidowane za pomocą rozwiązania NoSQL.

Często co widzimy pracy najlepsze jest składu podejścia, w którym używasz SQL i NoSQL za pomocą jednego rozwiązania. Nawet gdy Ludzie mówią one one założeń NoSQL, jeśli możesz teraz przejść do szczegółów co robią można często znaleźć, przy użyciu kilku różnych platform NoSQL: przy użyciu [CouchDB](http://wiki.apache.org/couchdb/Introduction), i [Redis](http://redis.io/)i [ Riak](http://basho.com/riak/) różne. Nawet Facebook, która używa NoSQL często, używa różnych platform NoSQL dla różnych części usługi. Elastyczna mieszanie i dopasowywanie metod przechowywania danych jest jedną z rzeczy, czyli nieuprzywilejowany o chmurze, ponieważ jest łatwy w obsłudze wielu rozwiązań danych i ich integracji w jednej aplikacji.

Oto kilka pytań, które zastanów się, gdy masz Wybieranie podejścia:

| Dane semantyczne | — Co to jest rdzeń magazynem i danymi dostępu do danych semantycznych (możesz przechowywanych danych relacyjnych lub bez struktury)? Dane bez określonej struktury, takich jak pliki multimedialne najlepiej odpowiada wymaganiom w magazynie obiektów blob; zbiór powiązanych danych, takich jak produkty spisów, dostawców, zamówienia, itp., najlepiej pasuje do relacyjnej bazy danych. |
| --- | --- |
| Obsługa zapytań | — Jak łatwo jest do wykonywania zapytań o dane? -Co typów pytań można efektywnie pytanie? Magazyny danych klucz/wartość są bardzo dobrze radzą wprowadzenie pojedynczy wiersz z danej wartości klucza, ale nie są więc odpowiednie dla złożonych zapytań. Dla magazynu danych profilu użytkownika gdzie są zawsze pobierania danych dla jednego określonego użytkownika magazynu danych klucz wartość można świetnie; katalog produktów, w której chcesz uzyskać różne sposoby, na podstawie różnych atrybutów produktu relacyjnej bazy danych może działać lepiej. Bazy danych NoSQL można przechowywać bardzo dużych ilości danych, ale trzeba struktury bazy danych w całym jak aplikacja wykonuje zapytania o dane, a to sprawia, że zapytania ad hoc trudniej wykonać. Dzięki relacyjnej bazie danych mogą tworzyć prawie każdy rodzaj zapytania. |
| Projekcja funkcjonalności | — Może pytania, agregacje, itp., być wykonany po stronie serwera? Jeśli liczba wybierz (\*) z tabeli w języku SQL, będą bardzo wydajny sposób wykonają tę pracę na serwerze i zwrotu, liczba szukam. Aby takie samo obliczenie z magazynu danych NoSQL, która nie obsługuje agregacji, jest nieefektywne "niepowiązane zapytania", prawdopodobnie przekroczy limit czasu. Nawet jeśli zapytanie zakończy się pomyślnie muszę pobrać wszystkie dane z serwera do klienta, a liczba wierszy na komputerze klienckim. — Można użyć jakie języki lub typy wyrażeń? Można używać z relacyjnej bazy danych SQL. Za pomocą niektórych baz danych NoSQL takich jak Azure Table storage, czy mogę używać [OData](http://www.odata.org/), i może zrobić, wystarczy filtrować według klucza podstawowego i Uzyskaj projekcji (Wybierz podzbiór dostępne pola). |
| Łatwość skalowania | -Jak często i będzie dużej ilości danych chcesz skalować? -Zawiera skalowalnego w poziomie jest natywnie implementacji platformy? — Jak łatwo jest usunąć pojemność (rozmiar i przepływność)? Relacyjnych baz danych i tabel nie są automatycznie podzielone na partycje aby były one skalowalne, aby były trudne do skalowania poza pewne ograniczenia. Wszystko, czego partycji natury magazynów danych NoSQL, takie jak usługa Azure Table storage, a nie ma limitu prawie zacząć dodawać partycji. Table Storage można łatwo skalować do 200 terabajtów, ale maksymalny rozmiar bazy danych usługi Azure SQL Database to 500 GB. Możesz skalować dane relacyjne, dzieląc go na wiele baz danych, ale Konfigurowanie aplikacji do obsługi modelu obejmuje wiele pracy programowania. |
| Instrumentacja i możliwości zarządzania | — Jak łatwo jest platformą Instrumentacja i monitorowanie oraz zarządzać nimi? Konieczne będzie aktualne informacje o kondycji i wydajności magazynu danych, więc musisz wiedzieć na początku jakie metryki platforma zapewnia bezpłatne, a posiadane przez Ciebie do opracowywania samodzielnie. |
| Operacje | — Jak łatwo jest platformy w celu wdrażania i uruchamiania na platformie Azure? PaaS? IaaS? Linux? Table Storage i bazy danych SQL są łatwe do skonfigurowania na platformie Azure. Platformy, które nie są wbudowane rozwiązania PaaS platformy Azure wymagają więcej nakładu pracy. |
| Obsługa interfejsu API | -Jest interfejs API ułatwiający pracę z platformą? Usługę Azure Table Service jest zestaw SDK przy użyciu interfejsu API platformy .NET, który obsługuje asynchronicznego modelu programowania .NET 4.5. Jeśli piszesz aplikację platformy .NET będzie znacznie ułatwia pisanie i testowanie kodu dla usługi Azure Table Service, w porównaniu do innej klucz/wartość kolumny danych magazynu platformy, która ma żadnych interfejsów API lub mniej kompleksowe jeden. |
| Poziomu spójności transakcyjnej, integralności i danych | -Jest krytyczne, czy platforma obsługuje transakcji w celu zagwarantowania spójności danych? Do śledzenia zbiorcze wiadomości e-mail, wydajność i koszt magazynowania danych niski może być ważniejsza niż automatyczna obsługa dla transakcji lub integralności referencyjnej w platformy danych, dzięki czemu usługę Azure Table Service to dobry wybór. Do śledzenia konta bankowego równoważy lub zamówień zakupu platformy relacyjnej bazy danych, która zapewnia silne gwarancje transakcyjnej może być lepszym rozwiązaniem. |
| Ciągłość prowadzenia działalności biznesowej | — Jak łatwo jest kopii zapasowej, przywracanie i odzyskiwanie po awarii? Wcześniej czy później ulegną uszkodzeniu danych produkcyjnych i musisz mieć funkcję cofania. Relacyjne bazy danych często mają więcej możliwości przywracania szczegółowych, takie jak możliwość przywracania do punktu w czasie. Zrozumienie, jakie funkcje przywracania są dostępne w poszczególnych platform, które rozważasz jest ważnym czynnikiem do przeanalizowania. |
| Koszt | — Jeśli więcej niż jedną platformę może obsługiwać obciążenie danych, ich porównanie kosztów? Na przykład jeśli używasz produktu ASP.NET Identity, można przechowywać dane profilu użytkownika w usłudze Azure Table Service lub Azure SQL Database. Jeśli nie potrzebujesz zaawansowanych zapytań funkcji SQL Database, może być częściowo wybrać tabele platformy Azure, ponieważ to kosztuje znacznie szybciej w przypadku danego ilość miejsca. |

Co to jest ogólnie zaleca się jest znany odpowiedzi na pytania w każdej z tych kategorii, przed wybraniem rozwiązania magazynu danych.

Ponadto obciążenie może mieć specyficzne wymagania, które niektórych platform może obsługiwać lepiej niż inne. Na przykład:

- Twojej aplikacji wymagają inspekcji funkcji?
- Jakie są wymagania dotyczące trwałość danych — jest wymagane automatyczne możliwości archiwizacji lub przeczyszczania?
- Czy masz potrzeby w zakresie wyspecjalizowanych zabezpieczeń? Na przykład dane obejmują dane osobowe (identyfikowalne dane osobowe), ale muszą być w stanie upewnić się, czy też danych osobowych jest wykluczony z wyników zapytania.
- Jeśli masz dane, które nie mogą być przechowywane w chmurze z powodów prawnych lub technologii, może być konieczne to platforma magazynu danych chmury, która ułatwia integrowanie z magazynu lokalnego.

## <a name="demo--using-sql-database-in-azure"></a>Pokaz — przy użyciu bazy danych SQL na platformie Azure

Aplikacja naprawić używa relacyjnej bazy danych do przechowywania zadań. Skrypt programu Windows PowerShell tworzenia środowiska objętego [Automatyzowanie wszystkiego rozdział](automate-everything.md) tworzy dwa wystąpienia bazy danych SQL. Możesz to zobaczyć w portalu, klikając **baz danych SQL** kartę.

![Bazy danych SQL w portalu](data-storage-options/_static/image8.png)

Jest również łatwo jest tworzyć baz danych przy użyciu portalu.

Kliknij przycisk **nowe — Data Services —** -- **bazy danych SQL** -- **szybkie tworzenie**, wprowadź nazwę bazy danych, wybierz serwer, masz już na Twoim koncie lub Utwórz nową i kliknij przycisk **tworzenie bazy danych SQL**.

![Nowa baza danych SQL](data-storage-options/_static/image9.png)

Odczekaj kilka sekund, a masz bazę danych na platformie Azure gotowe do użycia.

![Utworzona nowa baza danych SQL](data-storage-options/_static/image10.png)

Dlatego platformy Azure w kilka sekund, co może potrwać możesz dzień lub w tygodniu lub dłużej mogą to zrobić w środowisku lokalnym. A ponieważ użytkownik może równie łatwo można utworzyć bazy danych automatycznie w skrypcie lub za pomocą interfejsu API zarządzania, można dynamicznie skalować w poziomie Dzięki rozdzieleniu danych w wielu < o:p > bazach danych, tak długo, jak aplikacja ma zaprogramowane w tym. < /o : p >

Jest to przykład naszego modelu platforma jako usługa. Nie trzeba zarządzać serwerami, robimy to. Nie trzeba martwić się o kopiach zapasowych, robimy to. Działa w wysokiej dostępności — w bazie danych są automatycznie replikowane między trzema serwerami. Jeśli maszyna jest niszczony, firma Microsoft automatycznie awaryjnie i utraty żadnych danych. Serwer jest regularnie poprawek, nie trzeba martwić.

Kliknij przycisk i Pobierz parametry połączenia dokładnie, wymagane i od razu zacząć korzystać z nowej bazy danych.

![Parametry połączenia](data-storage-options/_static/image11.png)

Pulpit nawigacyjny zawiera Historia połączeń i wielkość wykorzystanego magazynu.

![Pulpit nawigacyjny bazy danych SQL](data-storage-options/_static/image12.png)

Można zarządzać bazami danych w portalu lub przy użyciu narzędzi programu SQL Server możesz już znasz, w tym SQL Server Management Studio (SSMS) i narzędzi programu Visual Studio, SQL Server Object Explorer (SSOX) i Eksploratora serwera.

![SSOX](data-storage-options/_static/image13.png)

Inny korzyścią jest model rozliczania usług. Rozwój można zacząć od bezpłatna baza danych o rozmiarze 20 MB i produkcyjnej bazy danych, który rozpoczyna się od około 5 USD miesięcznie. Możesz płacić tylko za używane dane, które faktycznie są przechowywane w bazie danych nie maksymalnej pojemności. Nie musisz kupować licencji.

Baza danych SQL jest łatwe skalowanie. Poprawka aplikacji bazy danych, tworzonej przez nas w naszym skrypt automatyzacji wynosi o rozmiarze 1 GB. Jeśli przeskalujesz ją w górę do 150 GB po prostu przejdź do portalu i zmienić to ustawienie albo wykonaj polecenie interfejsu API REST, a w ciągu kilku sekund masz, którą można wdrożyć dane do bazy danych 150 GB.

![Wersje bazy danych SQL i rozmiary](data-storage-options/_static/image14.png)

To z możliwości chmury, aby wdrożyć infrastrukturę szybko i łatwo i rozpocząć korzystanie z niej natychmiast.

Aplikacja naprawić korzysta z dwóch baz danych SQL, jeden dla członkostwa (uwierzytelnianie i autoryzacja) i jeden dla danych, a to wszystko, co trzeba zrobić, aby aprowizować go, a przeskalujesz ją. Wcześniej jak aprowizować bazy danych za pomocą skryptów środowiska Windows PowerShell, a teraz wiesz także, jak łatwo można to zrobić w portalu.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework, a dostęp bezpośrednie bazy danych za pomocą platformy ADO.NET

Aplikacja naprawić uzyskuje dostęp do tych baz danych przy użyciu platformy Entity Framework, zalecaną przez firmę Microsoft ORM (mapowania obiektowo relacyjny) dla aplikacji .NET. Jest to jest doskonałym narzędziem, które ułatwia wydajność pracy deweloperskiej, ale wydajność pochodzi kosztem pogorszenie wydajności w niektórych scenariuszach. W aplikacji w chmurze w rzeczywistych warunkach nie wprowadzania wybór między korzystaniem z programów EF bezpośrednio za pomocą platformy ADO.NET — zarówno użyjesz. W większości przypadków, kiedy piszesz kod, który współdziała z bazy danych, pobieranie maksymalnej wydajności nie jest krytyczne i można korzystać z uproszczonego kodowanie i testowania możesz uzyskać za pomocą programu Entity Framework. W sytuacjach, w której obciążenie EF spowodowałoby niedopuszczalne wydajności może zapisu i wykonywania własnych zapytań za pomocą programu ADO.NET, najlepiej przez wywołanie procedury składowane.

Niezależnie od wybranej metody dostępu do bazy danych, należy zminimalizować "liczbę operacji" jak najszerzej. Innymi słowy Jeśli w zestawie zamiast dziesiątek, jak i setek mniejszymi jeden większych wyników kwerendy, można uzyskać potrzebne dane, jest zazwyczaj bardziej pożądane. Na przykład, jeśli potrzebujesz do listy studentów i kursy, które są one rejestrowane w zazwyczaj lepiej jest pobrać wszystkie dane w jedno połączenie zapytania, a nie wprowadzenie uczniów w jednym zapytaniu i wykonywania oddzielnych zapytań dla każdego ucznia kursów.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Baz danych SQL i programu Entity Framework w aplikacji naprawić

W aplikacji naprawić `FixItContext` klasy, która pochodzi z programu Entity Framework `DbContext` klasy, identyfikuje bazę danych i określa tabele w bazie danych. Kontekst określa zestaw jednostek (tabela) dla zadań, a kod przekazuje kontekst nazwa parametrów połączenia. Tę nazwę odnosi się do parametrów połączenia, który jest zdefiniowany w pliku Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Parametry połączenia w *Web.config* plik nosi appdb (w tym miejscu skierowana do rozwoju lokalnego bazy danych):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Tworzy Entity Framework *FixItTasks* tabeli na podstawie właściwości zawarte w `FixItTask` Klasa jednostki. Jest to prosty klasy POCO (zwykłe stare obiektu CLR), która oznacza, że nie dziedziczy, lub mieć żadnych zależności na platformie Entity Framework. Ale Entity Framework wie, jak utworzyć tabelę na jego podstawie i wykonywanie operacji CRUD (Tworzenie — odczyt aktualizowanie usuwanie) operacji z nim.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

Aplikacja rozwiązać go zawiera interfejs repozytorium, który używa dla operacji CRUD, pracy z magazynem danych.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Zauważ, że metody repozytorium są wszystkie asynchroniczne, więc dostępu do danych może odbywać się w sposób całkowicie asynchroniczna.

Wywołań metod asynchronicznych Entity Framework implementacji repozytorium do pracy z danymi, w tym LINQ zapytań oraz jak w przypadku wstawiania, operacje aktualizowania i usuwania. Poniżej przedstawiono przykładowy kod do wyszukiwania zadania rozwiązać go.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Można zauważyć ma również niektóre terminy i kod rejestrowanie błędu, omówimy, później w [rozdział monitorowanie i dane telemetryczne](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Wybieranie bazy danych SQL Database (PaaS) i programu SQL Server na maszynie Wirtualnej (IaaS), na platformie Azure

Korzyścią dotyczące programu SQL Server i usługi Azure SQL Database jest taka sama podstawowego modelu programowania dla obu z nich. Korzystać z większości tego samego umiejętności, w obu środowiskach. Nawet służy bazy danych programu SQL Server w rozwoju i wystąpienie bazy danych SQL w chmurze, co jest, jak skonfigurować aplikację naprawić.

Jako alternatywę można uruchomić tego samego programu SQL Server w chmurze działają lokalnie przez zainstalowanie go na maszynach wirtualnych IaaS. W przypadku niektórych starszych aplikacji programem SQL Server na maszynie wirtualnej może być lepszym rozwiązaniem. Ponieważ bazy danych programu SQL Server działa na dedykowanych maszyn wirtualnych, ma więcej dostępnych zasobów niż baza danych SQL Database, które jest uruchamiane na serwerze współużytkowanym. Oznacza to bazy danych programu SQL Server może być większy i nadal działać dobrze. Ogólnie rzecz biorąc im mniejszy rozmiar bazy danych i rozmiar tabeli, lepiej przypadek użycia działa w przypadku bazy danych SQL Database (PaaS).

Oto niektóre wytyczne dotyczące wyboru między dwoma modelami.

| Usługa Azure SQL Database (PaaS) | SQL Server na maszynie wirtualnej (IaaS) |
| --- | --- |
| **Specjaliści** — nie trzeba utworzyć Zarządzanie maszynami wirtualnymi, aktualizacji lub poprawek systemu operacyjnego lub SQL; Platforma Azure zrobi, które dla Ciebie. -Wbudowaną wysoką dostępność, z umową SLA na poziomie bazy danych. -Niski całkowity koszt posiadania (TCO), ponieważ płacisz tylko za rzeczywiste użycie (nie wymagana jest licencja). — Dobra do obsługi dużej liczby mniejszych baz danych (&lt;= 500 GB). -Łatwy do dynamicznego tworzenia nowych baz danych, aby umożliwić skalowalnego w poziomie. | ***Specjaliści*** — funkcja zgodne z lokalnym serwerem SQL. — Można wdrożyć program SQL Server [wysokiej dostępności (AlwaysOn)](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) w 2 + maszyn wirtualnych, za pomocą umowa SLA na poziomie maszyny Wirtualnej. — Masz pełną kontrolę nad jak zarządzane SQL. -Mogą ponownie używać licencji SQL już właścicielem, lub zapłacić za godzinę w przypadku jednego. -Dobrą obsługę mniej, ale większa (1 TB +) baz danych. |
| **Cons** -niektórych funkcji luki w porównaniu z lokalnym serwerem SQL (braku [integrację środowiska CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [obsługi kompresji](https://technet.microsoft.com/library/cc280449.aspx), [programu SQL Server Usługi Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), itp.) — limit rozmiaru bazy danych wynoszący 500 GB. | ***Cons*** — aktualizacji/poprawek (systemu operacyjnego i programu SQL) jest odpowiedzialny za — tworzenie i zarządzanie bazami danych, spoczywa wyłącznie usługi - dysku operacje We/Wy (operacje wejścia/wyjścia na sekundę) maksymalnie 8000 (za pośrednictwem dysków z danymi 16). |

Jeśli chcesz użyć programu SQL Server na maszynie wirtualnej, można użyć własnej licencji programu SQL Server, lub możesz zapłacić za godzinę. Na przykład w portalu lub za pośrednictwem interfejsu API REST można utworzyć nową maszynę Wirtualną przy użyciu obrazu programu SQL Server.

![Tworzenie maszyny Wirtualnej z programem SQL Server](data-storage-options/_static/image16.png)

![Lista obrazów maszyn wirtualnych programu SQL Server](data-storage-options/_static/image17.png)

Podczas tworzenia maszyny Wirtualnej przy użyciu obrazu programu SQL Server, firma Microsoft proaktywne współczynnik koszty licencji programu SQL Server za godzinę na podstawie użycia maszyny wirtualnej. Jeśli masz projekt, który jest tylko do uruchamiania z kilku miesięcy, jest tańsze zapłacić za godzinę. Jeśli uważasz, że projekt będzie ostatnich lat, jest tańsze kupić licencję sposobu robisz to zazwyczaj.

## <a name="summary"></a>Podsumowanie

Chmura obliczeniowa pozwala łatwo można mieszać i dopasowania danych magazynu podejścia do optymalnego dopasowania do potrzeb aplikacji. Jeśli tworzysz nową aplikację, należy dobrze przemyśleć pytania wymienione tutaj, aby można było wybrać metod, które będą nadal działać dobrze w przypadku, gdy wzrośnie aplikacji. [Następny rozdział](data-partitioning-strategies.md) wyjaśnią kilka strategii partycjonowania, używane do łączenia wielu metod przechowywania danych.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji zobacz następujące zasoby.

Wybieranie platformy bazy danych:

- [Dostęp do danych dla rozwiązania o wysokiej skalowalności: przy użyciu programu SQL, NoSQL i Polyglot Persistence](http://aka.ms/dag-doc). Książka elektroniczna przez Microsoft Patterns and Practices, przechodzi szczegółowo w różne rodzaje danych przechowywane są dostępne dla aplikacji w chmurze.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/ff898430.aspx). Zobacz podstawy spójności danych, replikacja danych oraz wskazówki dotyczące synchronizacji, wzorzec indeksowania tabeli, wzorzec zmaterializowanego widoku.
- [PODSTAWOWA: Acid alternatywę](http://queue.acm.org/detail.cfm?id=1394128). Artykuł o kompromis między spójnością danych i skalowalności.
- [Siedem baz danych w ciągu siedmiu tygodni: Przewodnik dotyczący nowoczesnych baz danych i przepływu NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Książka autorstwa Eric Redmond i Jim R. Wilson. Zdecydowanie zaleca się wprowadzenie samodzielnie należała do zakresu platform magazyn danych jest obecnie dostępne.

Wybieranie między programu SQL Server i bazy danych SQL:

- [Wytyczne dotyczące bazy danych SQL w wersji zapoznawczej Premium](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Wprowadzenie do bazy danych SQL w warstwie Premium i wskazówek dotyczących kiedy należy wybrać go za pośrednictwem wersji bazy danych SQL w sieci Web i Business.
- [Wytyczne i ograniczenia (usługa Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Strony portalu, który stanowi łącze do dokumentacji dotyczącej ograniczenia bazy danych SQL, łącznie z jedną, która koncentruje się na funkcje programu SQL Server tej bazy danych SQL nie obsługuje.
- [SQL Server na maszynach wirtualnych platformy Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Strona portalu, który stanowi łącze do dokumentacji dotyczącej uruchomiony program SQL Server na platformie Azure.
- [Scott Guthrie wyjaśnia baz danych SQL na platformie Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6-minutowe wideo z wprowadzeniem do bazy danych SQL, Scott Guthrie.
- [Wzorce aplikacji i strategie programowania dla programu SQL Server na maszynach wirtualnych platformy Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Za pomocą programu Entity Framework i bazy danych SQL w aplikacji sieci Web platformy ASP.NET

- [Wprowadzenie do programów EF 6 z wykorzystaniem MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Dziewięć części serii samouczków, który przeprowadzi Cię przez tworzenie aplikacji MVC, która korzysta z programu EF i wdraża bazy danych na platformę Azure i bazy danych SQL.
- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Dwanaście części serii samouczków jest przenoszony do bardziej szczegółowe informacje o tym, jak wdrożyć bazę danych przy użyciu programu EF Code First.
- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web systemu Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Samouczek krok po kroku, który przeprowadzi Cię przez tworzenie aplikacji sieci web, który korzysta z uwierzytelniania, tabel aplikacji są przechowywane w bazie danych członkostwa, modyfikuje schemat bazy danych, a aplikacja jest wdrażana na platformie Azure.
- [Mapa zawartości dostępu do danych w programie ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Zawiera łącza do zasobów do pracy z programem EF i bazy danych SQL.

Używanie bazy danych MongoDB na platformie Azure:

- [MongoLab — bazy danych MongoDB na platformie Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Strona portalu dokumentacji dotyczącej działa baza danych MongoDB na platformie Azure.
- [Utwórz witrynę sieci web platformy Azure, który nawiązuje połączenie z bazą danych MongoDB uruchomioną na maszynie wirtualnej na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Samouczek krok po kroku, który pokazuje, jak używać bazy danych MongoDB w aplikacji sieci web ASP.NET.

HDInsight (Hadoop na platformie Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portal w dokumentacji HDInsight [Azure](https://azure.microsoft.com/) witryny sieci Web.
- [Usługi Hadoop i HDInsight: dane Big Data na platformie Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artykuł w MSDN Magazine Bruno Terkaly i Villalobos Ricardo, wprowadzenie do usługi Hadoop na platformie Azure.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz MapReduce wzorca.

> [!div class="step-by-step"]
> [Poprzednie](single-sign-on.md)
> [dalej](data-partitioning-strategies.md)
