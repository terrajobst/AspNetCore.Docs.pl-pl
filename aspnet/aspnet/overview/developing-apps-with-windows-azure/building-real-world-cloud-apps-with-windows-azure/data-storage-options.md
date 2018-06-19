---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opcje magazynu danych (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: d638dca331cb24c340a4471e5964a00b75bb608a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879715"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opcje magazynu danych (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Większość użytkowników służą do relacyjnych baz danych i często do przeoczenia inne opcje magazynu danych podczas ich projektowania aplikacji w chmurze. Może to spowodować powstanie nieoptymalne wydajność, wysoką wydatków lub co gorsza, ponieważ [NoSQL](http://en.wikipedia.org/wiki/NoSQL) baz danych (nierelacyjnych) może obsługiwać niektórych zadań wydajniej relacyjnych baz danych. W przypadku klientów nam możesz poprosić o pomoc rozwiązywanie problemu z magazynem danych o kluczowym znaczeniu, jest często ponieważ mają one relacyjnej bazy danych gdzie jedną z opcji NoSQL będą działały lepiej. W tych sytuacjach klienta byłby lepiej jeśli były one wdrożone rozwiązanie NoSQL przed wdrożeniem aplikacji do środowiska produkcyjnego.

Z drugiej strony byłoby błędu założenie, że bazy danych NoSQL może robić wszystko to również lub wystarczająco dobrze. Brak nie pojedynczego danych zarządzania najlepiej dla wszystkich zadań magazynu danych; dane różnych rozwiązań do zarządzania są zoptymalizowane pod kątem różnych zadań. Większość aplikacji w chmurze rzeczywistych mają różne wymagania dotyczące magazynu danych i często są udostępniane najlepsze przy użyciu kombinacji wielu rozwiązań magazynu danych.

W tym rozdziale ma na celu umożliwiają szerszym znaczeniu dostępnych opcji magazynu danych do aplikacji w chmurze, a niektóre podstawowe wskazówki dotyczące sposobu wybierz te, które pasują do danego scenariusza. Warto wiedzieć dostępne opcje i pomyśleć o ich mocne i słabe strony, aby utworzyć aplikację. Zmiana opcji przechowywania danych w aplikacji produkcyjnej może być bardzo trudne, takie jak zmiana aparatu jet, gdy płaszczyzny są przesyłane.

## <a name="data-storage-options-on-azure"></a>Opcje magazynu danych na platformie Azure

Chmura sprawia, że stosunkowo łatwa do używania różnych relacyjnych i magazyny danych NoSQL. Poniżej przedstawiono niektóre platformy magazynu danych, które można użyć w systemie Azure.

![](data-storage-options/_static/image1.png)

W tabeli przedstawiono cztery rodzaje bazy danych NoSQL:

- [Klucz/wartość baz danych](https://msdn.microsoft.com/library/dn313285.aspx#sec7) przechowywania pojedynczego obiektu serializacji dla każdej wartości klucza. Są one dobry do przechowywania dużych ilości danych, których chcesz pobrać jeden element dla danej wartości klucza i nie masz kwerendy na podstawie innych właściwości elementu.

    [Magazyn obiektów Blob Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) baza danych klucz wartość funkcje takie jak magazyn plików w chmurze, za pomocą wartości kluczy, które odpowiadają nazw folderów i plików. Można pobrać pliku przez jego folder i nazwę pliku, a nie przez wyszukiwanie wartości w zawartości pliku.

    [Magazyn tabel Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) jest również bazą danych klucz wartość. Każda wartość jest nazywana *jednostki* (podobnie jak wiersz, określonej przez klucz partycji i klucz wiersza) i zawiera wiele *właściwości* (podobny do kolumn, ale nie wszystkie obiekty w tabeli mają takie same udostępniania kolumny). Zapytanie dla kolumn niż klucz jest bardzo mało wydajne i należy unikać. Na przykład można przechowywać dane profilu użytkownika, z jedną partycją przechowuje informacje dotyczące pojedynczego użytkownika. W oddzielnych właściwości jedną jednostkę lub osobne jednostki w tej samej partycji można przechowywać dane, takie jak nazwa użytkownika, skrót hasła, datę urodzenia i tak dalej. Jednak nie chcesz wysyłać zapytania dotyczące wszystkich użytkowników z danego zakresu dat urodzenia i nie można wykonać kwerendy między tabeli profilu a inną tabelą. Magazyn tabel jest bardziej skalowalny i tańsze niż relacyjnej bazy danych, ale go nie włączyć złożonych zapytań lub sprzężenia.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) baz danych klucz wartość, w których wartości są *dokumenty*. "Dokument" w tym miejscu nie jest używana w tym sensie, dokumentu programu Word i Excel, ale oznacza zbiór pola o nazwie i wartości, które mogą być dokumentu podrzędnego. Na przykład w tabeli historii zamówień dokument kolejność może być numer zamówienia, Data zamówienia i pól odbiorcy; i pole klienta może mieć pola nazwy i adresu. Bazy danych koduje pola danych w formacie XML, yaml programu, JSON lub BSON; lub może używać zwykłego tekstu. Co funkcja, która ustawia bazy dokumentów oprócz klucza i wartości baz danych jest możliwość wykonywania zapytań na pola klucza i definiować indeksów pomocniczych, aby podczas badania efektywniejsze. Tę możliwość sprawia, że dokument bazy danych jest bardziej odpowiedni dla aplikacji, które są potrzebne do pobierania danych na podstawie kryteriów bardziej złożone niż wartość klucz dokumentu. Na przykład w bazie danych dokumentu historii zamówień sprzedaży można zbadać w różnych pól, takich jak identyfikator produktu, identyfikator klienta, nazwę klienta i tak dalej. [Bazy danych MongoDB](http://www.mongodb.org/) jest bazą danych popularnych dokumentu.
- [Rodziny kolumn baz danych](https://msdn.microsoft.com/library/dn313285.aspx#sec9) są magazynów, umożliwiające struktury magazynu danych w kolekcjach powiązane kolumny o nazwie rodzinami kolumn klucza i wartości. Na przykład baza danych spisu może mieć jednej grupy kolumn dla nazwisko osoby (najpierw środkowy, ostatni), jedna grupa dla adresu osoby i jedna grupa dla informacji o profilu osoby (podana data urodzenia, płeć, itp.). Bazy danych można następnie przechowywać każdej rodziny kolumn w oddzielnej partycji przy zachowaniu wszystkich danych do jednej osoby powiązane z tym samym kluczem. Następnie możesz przeczytać wszystkie informacje o profilu bez konieczności zapoznaj się z artykułem również informacje nazwy i adresu. [Cassandra](http://cassandra.apache.org/) jest bazą danych popularnych rodziny kolumn.
- [Bazy danych programu Graph](https://msdn.microsoft.com/library/dn313285.aspx#sec10) przechowują informacje jako kolekcję obiektów i relacji. Celem bazy danych wykresu jest umożliwienie aplikacji efektywnie zapytań, które Eksplorowanie sieci obiektów i relacji między nimi. Na przykład obiekty mogą być pracownicy działu zasobów ludzkich bazy danych, i może być ułatwiające zapytania takie jak "Znajdź wszystkich pracowników, którzy pracują bezpośrednio lub pośrednio Scott." [Neo4j](http://www.neo4j.org/) jest bazą danych popularnych wykresu.

W porównaniu do relacyjnych baz danych, opcje NoSQL oferuje znacznie większą skalowalność i opłacalności przechowywania i analizowania danych bez struktury. Zależności to, że nie udostępniają zaawansowane queryability i funkcje integralności danych niezawodne relacyjnych baz danych. NoSQL będzie działać dla danych dziennika usług IIS, który obejmuje dużą bez potrzeby zapytań sprzężenia. NoSQL nie będzie działać tak dobrze dla banków transakcji, co wymaga bezwzględnego integralność i obejmuje wiele relacji z innymi danymi związanych z kontem.

Jest również nowszej kategorii platformy bazy danych o nazwie [NewSQL](http://en.wikipedia.org/wiki/NewSQL) łączącą skalowalności bazy danych NoSQL queryability i transakcyjnych integralność relacyjnej bazy danych. NewSQL baz danych zostały zaprojektowane dla rozproszonych przechowywania i przetwarzania zapytania, które często jest trudne do zaimplementowania w bazach danych "OldSQL". [NuoDB](http://www.nuodb.com/) przykładem NewSQL bazy danych, który może służyć na platformie Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop i MapReduce

Dużych ilości danych, które można przechowywać w bazach danych NoSQL może być trudne do analizowania wydajnie w odpowiednim czasie. Należy użyć framework, takich jak [Hadoop](http://hadoop.apache.org/) z zaimplementowanym [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funkcji. Zasadniczo proces MapReduce nie jest następujący:

- Limit rozmiaru danych, który ma zostać przetworzony przez wybranie poza dane przechowywane tylko faktycznie potrzebnych do analizowania danych. Na przykład zapoznać się organizację podstawowej przez rok urodzenia użytkownika, wybierz tylko lat urodzenia Twojego magazynu danych profilu użytkownika.
- Podział danych na części i wysyłać je do różnych komputerów do przetwarzania. Komputer A oblicza liczbę osobom 1950 1959 daty, komputer B jest 1960 1969, itp. Ta grupa komputerów jest nazywany *klastra usługi Hadoop*.
- Umieść wyniki każdej części razem ponownie po ukończeniu przetwarzania na części. Masz teraz stosunkowo krótkich listę jak wiele osób dla każdej rok urodzenia i zadania obliczania wartości procentowe na tej liście ogólny może być zarządzana.

Na platformie Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) umożliwia przetwarzania, analizy i uzyskania nowych danych z danych big data przy użyciu platformy Hadoop. Na przykład można go użyć do analizowania dzienniki serwera sieci web:

- Włącz rejestrowanie pracy serwera sieci web do swojego konta magazynu. Azure to skonfigurowanie zapisywanie dzienników usługi obiektów Blob dla każdego żądania HTTP do aplikacji. Usługa Blob jest zasadniczo magazynu plików w chmurze i dobrze integruje się z usługą HDInsight. 

    ![Dzienniki do magazynu obiektów Blob](data-storage-options/_static/image2.png)
- Jak aplikacja pobiera ruchu, dzienniki programu IIS serwera sieci web są zapisywane do magazynu obiektów Blob. 

    ![Dzienniki serwera sieci Web](data-storage-options/_static/image3.png)
- W portalu kliknij **nowy** - **usług danych** - **HDInsight** - **szybkie tworzenie**, i określ nazwę klastra usługi HDInsight, rozmiar klastra (liczba węzłów danych klastra usługi HDInsight) oraz nazwę użytkownika i hasło dla klastra usługi HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Można teraz skonfigurować zadań MapReduce do analizowania dzienników i uzyskać odpowiedzi na pytania, takich jak:

- Jakich porach dnia Moja aplikacja uzyskać ruchu większości lub co najmniej?
- Jakie krajach jest Mój ruchu przychodzącego z?
- Co to jest dochodu średni otoczenie obszarów mojego ruchu sieciowego pochodzi z. (Brak publicznego zestawu danych, który umożliwia otoczenie rachunku za pomocą adresu IP, a można zgodną z adresu IP w dziennikach serwera sieci web).
- Jak użytymi dochodu otoczenie dla określonych stron lub produktów w lokacji

Następnie można użyć odpowiedzi na pytania takich do docelowego reklamy oparte na prawdopodobieństwo, który klient będzie zainteresowana lub prawdopodobieństwo zakupu określonego produktu.

Zgodnie z objaśnieniem w [zautomatyzować wszystko rozdział](automate-everything.md), większość funkcji, które można wykonać w portalu można zautomatyzować, i który obejmuje skonfigurowanie i wykonywania zadań analizy HDInsight. W typowym skrypcie HDInsight może zawierać następujące czynności:

- Udostępnianie klastra usługi HDInsight i połączyć ją z kontem magazynu dla danych wejściowych z magazynu obiektów Blob.
- Przekazywanie plików wykonywalnych zadania MapReduce (Pliki JAR lub .exe) do klastra usługi HDInsight.
- Przedstawia MapReduce, która przechowuje dane do magazynu obiektów Blob.
- Poczekaj na ukończenie zadania.
- Usuń klaster usługi HDInsight.
- Dostęp do danych wyjściowych z magazynu obiektów Blob.

Przez uruchomienie skryptu w tym wszystkich, możesz zminimalizować ilość czasu, który zainicjowaniu obsługi klastra usługi HDInsight, co minimalizuje koszty.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platforma jako usługa (PaaS) a infrastruktura jako usługa (IaaS)

Opcje magazynu danych, które są wymienione wcześniej obejmują zarówno platforma jako — usługa (PaaS), jak i rozwiązania infrastruktury jako — usługa (IaaS). PaaS oznacza, że firma Microsoft zarządzania infrastrukturą sprzętu i oprogramowania, a tylko korzystania z usługi. Baza danych SQL jest funkcją PaaS systemu Azure. Pytaj dla baz danych, a w tle Azure konfiguruje i konfiguruje maszyn wirtualnych i konfiguruje je baz danych. Nie ma bezpośredniego dostępu do maszyn wirtualnych i nie trzeba zarządzać nimi. IaaS oznacza, że ustawienie, konfigurowania i zarządzania maszyn wirtualnych w naszym infrastruktury centrum danych i możesz umieścić dowolne na nich. Udostępniamy wstępnie skonfigurowane obrazów maszyn wirtualnych z galerii dla typowych konfiguracji maszyny Wirtualnej. Na przykład można zainstalować wstępnie skonfigurowane obrazów maszyn wirtualnych dla systemu Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, bazy danych Oracle,... itd.

PaaS danych rozwiązania, które platforma Azure oferuje obejmują:

- Azure SQL Database (wcześniej znane jako SQL Azure). Chmura relacyjnej bazy danych oparta na programie SQL Server.
- Magazyn tabel Azure. Klucz/wartość bazy danych NoSQL.
- Magazyn obiektów Blob Azure. Magazyn plików w chmurze.

Dla IaaS możesz uruchomić coś, co można załadować do maszyny Wirtualnej, na przykład:

- Relacyjnych baz danych, takich jak SQL Server, Oracle, MySQL, SQL Compact, SQLite lub Postgres.
- Magazyny danych klucza i wartości, takich jak Memcached, Redis Cassandra i Riak.
- Magazyny danych kolumny, takie jak bazy danych HBase.
- Następnie należy udokumentować baz danych, takich jak bazy danych MongoDB, RavenDB i CouchDB.
- Wykres baz danych, takich jak Neo4j.

![Opcje magazynu danych na platformie Azure](data-storage-options/_static/image5.png)

Opcja IaaS zapewnia opcje magazynowania danych praktycznie nieograniczona i wielu z nich są szczególnie łatwe w użyciu, ponieważ można utworzyć za pomocą wstępnie skonfigurowanych obrazów maszyn wirtualnych. Na przykład w portalu zarządzania, przejdź do **maszyn wirtualnych**, kliknij przycisk **obrazów** , a następnie kliknij pozycję **Przeglądaj magazynu maszyny Wirtualnej**.

![Przeglądaj magazynu maszyny Wirtualnej](data-storage-options/_static/image6.png)

Następnie wyświetlona lista [setki wstępnie skonfigurowane obrazów maszyn wirtualnych](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), można również utworzyć Maszynę wirtualną z obrazu, który ma wstępnie zainstalowane system zarządzania bazy danych, takie jak bazy danych MongoDB, Neo4J, Redis, Cassandra lub CouchDB:

![Bazy danych MongoDB w magazynie maszyny Wirtualnej](data-storage-options/_static/image7.png)

Azure udostępnia opcje magazynowania danych IaaS jako łatwych w użyciu, ale PaaS oferty mają wiele zalet, które stały się bardziej ekonomiczne i praktyczne w różnych scenariuszach:

- Nie trzeba tworzyć maszyny wirtualne, wystarczy użyć portalu lub skrypt do konfigurowania magazynu danych. Ma 200 terabajt magazyn danych można po prostu kliknij przycisk lub uruchamianie poleceń, a w sekundach jest gotowy do użycia.
- Nie trzeba zarządzać lub poprawka maszyn wirtualnych używanych przez usługę; Microsoft nie automatycznie.-nie trzeba martwić się o konfigurowaniu infrastruktury skalowanie i wysoka dostępność; Microsoft obsługuje wszystkie te dla Ciebie.
- Nie trzeba kupować licencje; opłaty licencji są uwzględniane w opłaty za usługę.
- Płacisz tylko za można użyć.

Opcje magazynu danych PaaS w Azure obejmują ofert przez dostawców innych firm. Na przykład można wybrać [dodatek MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) z magazynu Azure do udostępniania bazy danych MongoDB jako usługa.

## <a name="choosing-a-data-storage-option"></a>Wybranie opcji magazynu danych

Jeden z nich nie jest odpowiednia w przypadku wszystkich scenariuszy. Jeśli każda osoba, która informuje, czy ta technologia jest odpowiedź, przede wszystkim należy poprosić to "pytanie?", ponieważ różnych rozwiązań są zoptymalizowane pod kątem różnych rzeczy. Istnieją pewne zalety modelu relacyjne; dlatego został wokół tak długo. Istnieją także dół strony do bazy danych SQL, który może zostać zlikwidowane z rozwiązaniem NoSQL.

Często co widzimy pracy najlepiej jest składu podejście, gdy używasz programu SQL i NoSQL w jedno rozwiązanie. Nawet jeśli osób powiedzieć one jest obejmującego NoSQL, jeśli wejść co robią można często Znajdź, że ich korzysta z kilku różnych platform NoSQL: używanych [CouchDB](http://wiki.apache.org/couchdb/Introduction), i [Redis](http://redis.io/)i [ Riak](http://basho.com/riak/) różne. Nawet Facebook, który często używa NoSQL, używa różnych platform NoSQL dla różnych części usługi. Możliwość mieszać i dopasowywać podejścia magazynu danych jest jednym z elementów, które nieuprzywilejowany chmury, ponieważ jest łatwy w obsłudze wielu rozwiązań danych i ich integracji w jednej aplikacji.

Oto pytania zastanowić się, gdy jest wybranie podejścia:

| Semantycznego danych | — Co to jest core magazynu i dane dostępu do danych semantyczne (możesz przechowywanych danych relacyjnych lub bez struktury)? Danych niestrukturalnych, takich jak pliki multimedialne najlepiej dopasowane do magazynu obiektów blob; Kolekcja powiązanych danych, takich jak produkty, zapasów, dostawców, zamówienia, itp., najlepiej pasuje do relacyjnej bazy danych. |
| --- | --- |
| Obsługa zapytań | -Jak łatwo jest do pobrania danych? -Jakie typy pytania mogą być skutecznie zadawane? Magazyny danych klucz wartość są bardzo dobrze sobie radzi pobierania pojedynczy wiersz danej wartości klucza, ale nie są tak dobre złożonych zapytań. Magazynu danych profilu użytkownika gdzie są zawsze pobieranie danych dla jednego określonego użytkownika magazyn danych klucza i wartości można pracować katalog produktów, w którym chcesz uzyskać różne sposoby, na podstawie różnych atrybutów produktu relacyjnej bazy danych może działać lepiej. Bazy danych NoSQL może przechowywać dużych ilości danych, ale trzeba struktury bazy danych wokół jak aplikacja odpytuje dane, a dzięki temu zapytań ad hoc przeszkodę w celu. Z relacyjnej bazy danych można tworzyć niemal każdego rodzaju zapytania. |
| Projekcja funkcjonalności | — Można pytania, agregacje itp., można wykonać po stronie serwera? Czy mogę uruchomić wybierz liczbę (\*) z tabeli SQL zostanie bardzo wydajnie wykonywać wszystkie zadania na serwerze i zwracać numer poszukiwanego przeze. Aby te same obliczenia z magazynem danych NoSQL, który nie obsługuje agregacji, jest nieefektywne "unbounded"zapytania i będzie prawdopodobnie limit czasu. Nawet wtedy, gdy zapytanie zakończy się powodzeniem konieczne pobranie wszystkich danych z serwera do klienta, a liczba wierszy na kliencie. — Można użyć jakie języki lub typy wyrażeń? Z relacyjnej bazy danych I korzystać z SQL. Za pomocą niektórych NoSQL baz danych takie jak magazyn tabel Azure I będzie można przy użyciu [OData](http://www.odata.org/), i może zrobić jest filtrowanie według klucza podstawowego i uzyskać projekcje (Wybierz podzbioru dostępnych pól). |
| Łatwość skalowania | -Jak często i spowoduje dużą ilość danych musi przebiegać proces skalowania? -Nie platformy natywnie implementuje skalowalnych w poziomie? -Jak łatwo jest usunąć pojemności (rozmiar i przepływność)? Relacyjnych baz danych i tabele nie są automatycznie partycjonowany w celu zapewnienia ich skalowalne, tak, aby były trudne do skalowania poza pewne ograniczenia. Wszystkie partycje z założenia magazynów danych NoSQL, takie jak magazyn tabel Azure, a nie ma limitu prawie nie Dodawanie partycji. Magazyn tabel można łatwo skalować do 200 terabajtów, ale maksymalny rozmiar bazy danych dla bazy danych SQL Azure wynosi 500 GB. Podział go na wiele baz danych można skalować w relacyjnej bazie danych, ale Konfigurowanie aplikacji do obsługi modelu obejmuje dużo pracy programowania. |
| Instrumentacja i możliwości zarządzania | -Jak łatwo jest platformy Instrumentacja, monitorowania i zarządzania nimi? Konieczne będzie Śledź na bieżąco kondycję i wydajność magazynu danych, dlatego trzeba znać góry metryki, jakie platformy zapewnia bezpłatnie i co trzeba utworzyć samodzielnie. |
| Operacje | -Jak łatwo jest platforma do wdrażania i uruchamiania na platformie Azure? PaaS? IaaS? Linux? Magazyn tabel i bazy danych SQL są łatwe konfigurowanie na platformie Azure. Platformy, które nie są wbudowane rozwiązania Azure PaaS wymagają więcej wysiłku. |
| Obsługa interfejsu API | -Jest interfejs API ułatwiający pracę z platformą? Usługa tabel Azure jest SDK z interfejs API programu .NET obsługuje model programowania asynchronicznego .NET 4.5. Jeśli piszesz aplikację .NET będzie znacznie łatwiejsze do pisania i testowania kodu dla usługi tabel Azure, w porównaniu do innego klucza i wartości kolumny danych magazynu platforma, która ma żadnego interfejsu API lub mniej kompleksowe jeden. |
| Spójności transakcyjnej integralności i danych | -Jest krytyczny, że platforma obsługuje transakcje w celu zagwarantowania spójności danych? Dla zbiorczego wiadomości e-mail wysyłane, może być większe znaczenie niż automatyczną obsługę transakcji lub integralności referencyjnej w platformę danych wydajności i koszt magazynowania danych niski rejestrowanie informacji o wprowadzenie usługi tabel Azure dobrym rozwiązaniem. Śledzenia konta bankowego równoważy lub zamówienia zakupu platformy relacyjnej bazy danych, która zapewnia silne gwarancje transakcyjne będzie lepszym rozwiązaniem. |
| Ciągłość prowadzenia działalności biznesowej | -Jak łatwo jest kopii zapasowych, przywracania i odzyskiwania po awarii? Wcześniej lub później ulegną uszkodzeniu danych produkcyjnych i będzie trzeba funkcja cofania. Relacyjnych baz danych często mają więcej możliwości przywracania szczegółowych, takie jak możliwość przywracania do punktu w czasie. Zrozumienie, jakie funkcje przywracania są dostępne w każdej z platform, które rozważasz jest ważnym czynnikiem do przeanalizowania. |
| Koszt | — Jeśli więcej niż jednej platformie może obsługiwać obciążenie danych, ich porównanie kosztów? Na przykład jeśli używasz ASP.NET Identity można przechowywać dane profilu użytkownika w usłudze tabel Azure lub baza danych SQL Azure. Jeśli nie ma potrzeby zaawansowanej zapytań obiektów bazy danych SQL, można wybrać tabel Azure w części ponieważ kosztuje znacznie mniej dla danego ilość miejsca w magazynie. |

Co to zazwyczaj zalecane jest znany odpowiedzi na pytania w każdej z tych kategorii, przed wybraniem rozwiązań magazynu danych.

Ponadto obciążenie może mieć określone wymagania, obsługiwane w niektórych platform lepiej od innych. Na przykład:

- Twoje wymagana inspekcji możliwości?
- Jakie są wymagania trwałość danych — są wymagane automatyczne możliwości archiwizacji lub przeczyszczania?
- Czy istnieją wymagania zabezpieczeń specjalne? Na przykład dane obejmują dane osobowe (dane osobowe), ale musi być w stanie upewnić się, że dane osobowe jest wykluczony z wyników zapytania.
- Jeśli masz części danych, które nie mogą być przechowywane w chmurze powodów przepisami lub technologii, może być konieczne platformy magazynu danych chmury, który ułatwia integrowanie z magazynu lokalnego.

## <a name="demo--using-sql-database-in-azure"></a>Pokaz — przy użyciu bazy danych SQL na platformie Azure

Aplikacja napraw używa relacyjnej bazy danych do przechowywania zadań. Środowisko tworzenia skrypt programu Windows PowerShell pokazano [zautomatyzować wszystko rozdział](automate-everything.md) tworzy dwa wystąpienia bazy danych SQL. Można wyświetlić w portalu, klikając **baz danych SQL** kartę.

![Bazy danych SQL w portalu](data-storage-options/_static/image8.png)

Jest również łatwe do tworzenia baz danych za pomocą portalu.

Kliknij przycisk **nowych--usług danych** -- **bazy danych SQL** -- **szybkie tworzenie**, wprowadź nazwę bazy danych, wybierz serwer, masz już na Twoim koncie lub Utwórz nową i kliknij przycisk **tworzenie bazy danych SQL**.

![Nowej bazy danych SQL](data-storage-options/_static/image9.png)

Poczekaj kilka sekund, a masz bazę danych na platformie Azure jest gotowy do użycia.

![Utworzyć nową bazę danych SQL](data-storage-options/_static/image10.png)

Dlatego Azure w kilka sekund co może potrwać możesz dzień lub w tygodniu lub dłużej w środowisku lokalnym. I ponieważ można równie łatwo utworzyć baz danych automatycznie za pomocą skryptów lub przy użyciu interfejsu API zarządzania, można dynamicznie skalować w poziomie przez rozłożenie danych wielu < o: p > baz danych, tak długo, jak aplikacja ma zaprogramowane w tym. < /o : p >

To jest przykład naszego modelu platformy jako usługi. Nie trzeba zarządzać serwerami, jak ją. Nie trzeba martwić kopii zapasowych, jak ją. Działa ona w wysokiej dostępności — w bazie danych są automatycznie replikowane między trzema serwerami. W przypadku śmierci maszynę, firma Microsoft automatycznie awaryjnie i zostaną utracone żadne dane. Serwer jest regularnie poprawkami, nie musisz martwić się o który.

Kliknij przycisk i pobrać parametry połączenia dokładne, należy go i od razu rozpocząć korzystanie z nowej bazy danych.

![Parametry połączenia](data-storage-options/_static/image11.png)

Pulpit nawigacyjny zawiera Historia połączeń i pojemność magazynu używane.

![Pulpit nawigacyjny bazy danych SQL](data-storage-options/_static/image12.png)

Można zarządzać baz danych w portalu lub przy użyciu narzędzia SQL Server znasz już, w tym programu SQL Server Management Studio (SSMS) i Visual Studio tools, SQL Server obiektu Explorer (SSOX) i Eksploratora serwera.

![SSOX](data-storage-options/_static/image13.png)

Innym czynnikiem nieuprzywilejowany jest modelu cenowego. Programowanie można uruchomić z bazą danych 20 MB wolnego i produkcyjną bazę danych, który rozpoczyna się od około 5 USD miesięcznie. Płaci się tylko ilości danych, które faktycznie są przechowywane w bazie danych nie maksymalnej pojemności. Nie trzeba kupować licencji.

Baza danych SQL jest łatwo skalę. Poprawka aplikacji tworzonej przez nas w naszej skryptu automatyzacji bazy danych jest ograniczona do 1 GB. Jeśli użytkownik chce go skalować maksymalnie 150 GB, po prostu przejdź do portalu i zmienić to ustawienie albo wykonaj polecenie interfejsu API REST i w sekundach masz 150 GB bazy danych, którą można wdrożyć dane.

![Rozmiary i wersji bazy danych SQL](data-storage-options/_static/image14.png)

To mocy chmury wstanie infrastruktury szybkie i łatwe i rozpocząć korzystanie z niej natychmiast.

Aplikacja napraw korzysta z dwóch baz danych SQL, jeden dla członkostwa (uwierzytelnianie i autoryzacja) i jeden dla danych, a to wszystko, co należy zrobić, aby udostępnić go i skalować ją. Wcześniej przedstawiono udostępnianie baz danych za pomocą skryptów środowiska Windows PowerShell, a po zapoznaniu się również jak łatwo można to zrobić w portalu.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework lub dostępu bezpośredniego bazy danych za pomocą ADO.NET

Poprawka aplikacji uzyskuje dostęp do tych baz danych przy użyciu programu Entity Framework, firmy Microsoft zalecane ORM (mapowania obiektów relacyjnych) dla aplikacji .NET. ORM to doskonałe narzędzie, które ułatwia produktywność deweloperów, ale wydajność pochodzi kosztem pogorszenie wydajności w niektórych scenariuszach. W aplikacji rzeczywistych chmury nie dokonywania wyboru między używaniem EF bezpośrednio za pomocą ADO.NET — zarówno użyjesz. W większości przypadków podczas pisania kodu, który działa z bazy danych, pobieranie maksymalną wydajność nie ma znaczenia krytycznego i można korzystać z uproszczone kodowanie i testowania, możesz uzyskać z programu Entity Framework. W sytuacjach, w którym obciążenie EF spowodowałoby niedopuszczalne wydajności możesz zapisać i wykonywanie własnych zapytań za pomocą ADO.NET, najlepiej przez wywołanie procedury składowane.

Niezależnie od wybranej metody dostępu do bazy danych, należy zminimalizować "chattiness" jak to możliwe. Innymi słowy w przypadku wszystkich potrzebnych danych można uzyskać w zestawie zamiast dziesiątki lub setki mniejszych jeden większa wyników zapytania, który jest zwykle preferowane. Na przykład jeśli chcesz studentów listy i szkolenia, które są one rejestrowane w jest lepiej jest pobrać wszystkich danych na dołączenie do jednego zapytania zamiast pobierania studentów w jednym zapytaniu i wykonywania oddzielnych zapytań dotyczących szkoleń w poszczególnych studenta.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bazy danych SQL i programu Entity Framework w aplikacji napraw

W aplikacji Usuń `FixItContext` klasy, która pochodzi z programu Entity Framework `DbContext` , identyfikuje bazę danych, określa tabele w bazie danych. Kontekst określa zestaw jednostek (tabeli) dla zadania i kodu przekazuje w w kontekście nazwę ciągu połączenia. Ta nazwa odwołuje się do ciągu połączenia, który jest zdefiniowany w pliku Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Parametry połączenia w *Web.config* plik ma nazwę appdb (w tym miejscu skierowana do lokalnej bazy danych):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Tworzy programu Entity Framework *FixItTasks* tabeli na podstawie właściwości zawarte w `FixItTask` klasy jednostka. Jest to prosty klasy POCO (zwykły stary obiekt CLR), która oznacza, że nie dziedziczy, lub mieć żadnych zależności na platformie Entity Framework. Ale Entity Framework wie, jak utworzyć tabelę na ich podstawie i wykonywanie operacji CRUD (Tworzenie odczytu aktualizowania i usuwania) z nim.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

Poprawka aplikacji zawiera interfejs repozytorium używa operacji CRUD pracy z magazynem danych.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Zauważyć, że metody repozytorium są async wszystkich, więc dostęp do wszystkich danych może odbywać się w sposób całkowicie asynchronicznego.

Wywołań metod asynchronicznych Entity Framework implementacji repozytorium do pracy z danymi, w tym LINQ zapytań oraz jak w przypadku wstawiania, aktualizowania i usuwania działań. Oto przykładowy kod do wyszukiwania zadań usuń go.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Można zauważyć, dostępna jest również niektórych harmonogram i kod rejestrowania błędu, przyjrzymy który później w [rozdział monitorowanie i dane telemetryczne](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Wybieranie bazy danych SQL (PaaS) lub programu SQL Server na maszynie Wirtualnej na platformie Azure (IaaS)

Dobry element dotyczące programu SQL Server i bazy danych SQL Azure jest taki sam model programowania core dla obu z nich. Większość tego samego umiejętności można użyć w obu środowiskach. Nawet korzystania z bazy danych programu SQL Server w rozwoju i wystąpienie bazy danych SQL w chmurze, która jest konfiguracji aplikacji Usuń.

Alternatywnie można uruchomić tego samego serwera SQL w chmurze uruchomić lokalnie przez zainstalowanie go na maszyny wirtualne IaaS. W przypadku niektórych starszych aplikacji programem SQL Server na maszynie wirtualnej może okazać się lepszym rozwiązaniem. Ponieważ bazy danych programu SQL Server jest uruchomiony na dedykowany maszyna wirtualna, ma więcej dostępnych zasobów niż baza danych SQL bazy danych, która działa na serwerze udostępnionej. Oznacza to bazy danych programu SQL Server może być większa i nadal wykonywać również. Ogólnie rzecz biorąc mniejszego rozmiaru bazy danych i rozmiar tabeli, lepiej przypadek użycia działa w przypadku bazy danych SQL (PaaS).

Poniżej przedstawiono wskazówki dotyczące wyboru między dwa modele.

| Baza danych Azure SQL (PaaS) | Program SQL Server w maszynie wirtualnej (IaaS) |
| --- | --- |
| **Specjaliści** — nie trzeba utworzyć zarządzenie maszynami wirtualnymi, aktualizacja lub poprawka systemu operacyjnego lub języka SQL; Azure nie. -Wbudowaną wysoką dostępność, z SLA poziom bazy danych. -Małe całkowity koszt posiadania (TCO), ponieważ płacisz tylko za służą (nie licencji). -Dobra do obsługi dużej liczby mniejszych baz danych (&lt;= 500 GB). -Łatwy do dynamicznego tworzenia nowych baz danych, aby włączyć skalowalnego w poziomie. | ***Specjaliści*** — funkcja zgodnego z lokalnym programem SQL Server. — Można wdrożyć program SQL Server [wysokiej dostępności za pośrednictwem funkcji AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) w 2 + maszyn wirtualnych z poziomu maszyny Wirtualnej SLA. -Masz pełną kontrolę nad sposób zarządzania SQL. — Można użyć ponownie licencji SQL już posiadanych, lub płacić za godzinę dla jednego. -Dobra do obsługi mniej, ale większych (1 TB +) bazy danych. |
| **Cons** -niektórych funkcji luk w porównaniu do lokalnego programu SQL Server (braku [integrację środowiska CLR](https://technet.microsoft.com/library/ms131102.aspx), [funkcji TDE](https://technet.microsoft.com/library/bb934049.aspx), [obsługi kompresji](https://technet.microsoft.com/library/cc280449.aspx), [programu SQL Server Usługi Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), itd.)-ograniczenie rozmiaru bazy danych do 500 GB. | ***Cons*** — aktualizacje/poprawki (systemu operacyjnego i SQL) jest odpowiedzialny za — tworzenie i zarządzanie bazami danych jest odpowiedzialny za — IOPS dysku (operacje wejścia/wyjścia na sekundę), maksymalnie 8000 (za pomocą dysków z danymi 16). |

Jeśli chcesz użyć programu SQL Server na maszynie wirtualnej, można użyć własnej licencji programu SQL Server, lub możesz zapłacić za godzinę. Na przykład w portalu lub przy użyciu interfejsu API REST można utworzyć nowej maszyny Wirtualnej przy użyciu obrazu programu SQL Server.

![Tworzenie maszyny Wirtualnej z programem SQL Server](data-storage-options/_static/image16.png)

![Lista obrazów maszyn wirtualnych serwera SQL](data-storage-options/_static/image17.png)

Podczas tworzenia maszyny Wirtualnej z obrazu programu SQL Server, firma Microsoft wykraczającego szybkość kosztów licencji programu SQL Server o godzinę na podstawie użycia maszyny wirtualnej. Jeśli masz projektu, który jest tylko do uruchomienia z kilku miesięcy, jest tańsze zapłacić za godzinę. Jeśli uważasz, że projekt będzie ostatnich lat, jest tańsze zakupu licencję tak jak zwykle.

## <a name="summary"></a>Podsumowanie

Chmura obliczeniowa stwarza praktyczne mieszać i dopasowania danych magazynu podejścia, aby jak najlepiej do potrzeb aplikacji. Jeśli tworzysz nową aplikację, zastanów się uważnie pytania wymienione w tym miejscu w celu pobrania metod, które będą nadal działać w przypadku rozwoju aplikacji. [Następnego rozdziału](data-partitioning-strategies.md) objaśnia kilka strategii partycjonowania, które umożliwiają łączenie wielu metod magazynu danych.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji zobacz następujące zasoby.

Wybieranie platformy bazy danych:

- [Dostęp do danych dla rozwiązań wysokiej skalowalności: przy użyciu programu SQL, NoSQL i Polyglot trwałości](http://aka.ms/dag-doc). Książka elektroniczna przez Microsoft Patterns and Practices prowadzącym szczegółowo do różnych rodzajów danych przechowywane są dostępne dla aplikacji w chmurze.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/ff898430.aspx). Zobacz Elementarz spójności danych, replikacji danych i wytyczne dotyczące synchronizacji, wzorca indeksu tabeli, widoku Zmaterializowany wzorca.
- [PODSTAWOWA: Acid zamiast](http://queue.acm.org/detail.cfm?id=1394128). Artykuł o wady i zalety między spójność danych i skalowalność.
- [Siedem baz danych w siedmiu tygodni: Przewodnik dotyczący nowoczesne bazy danych i przepływu NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Książki marek Redmond i Jim R. Wilsona. Zdecydowanie zaleca się wprowadzenie samodzielnie różnych platform magazynu danych jest obecnie dostępne.

Wybór między programu SQL Server i bazy danych SQL:

- [Wskazówki dotyczące bazy danych SQL w wersji zapoznawczej Premium](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Wprowadzenie do bazy danych SQL — wersja Premium i wskazówki dotyczące sytuacji wybrać go za pośrednictwem bazy danych SQL w sieci Web i Business Edition.
- [Wskazówki i ograniczenia (baza danych Azure SQL)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Strony portalu, który stanowi łącze do dokumentacji o ograniczeniach bazy danych SQL, w tym, który skupia się na funkcjach programu SQL Server tej bazy danych SQL nie obsługuje.
- [SQL Server na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Strony portalu, który stanowi łącze do dokumentacji o uruchamianiu programu SQL Server na platformie Azure.
- [Bazy danych SQL na platformie Azure opisano Scott Guthrie](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6-minutowe wideo z wprowadzeniem do usługi SQL Database za Scott Guthrie.
- [Wzorce aplikacji i strategii rozwoju dla programu SQL Server na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Przy użyciu programu Entity Framework i bazy danych SQL w aplikacji sieci Web ASP.NET

- [Wprowadzenie do programów EF 6 przy użyciu MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Dziewięć części samouczka serii, która przeprowadzi Cię przez tworzenie aplikacji MVC, która używa EF i wdraża bazy danych na platformie Azure oraz bazy danych SQL.
- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 części samouczka serii, która przechodzi bardziej szczegółowo omówiono sposób wdrażania bazy danych przy użyciu EF Code First.
- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure witrynę sieci Web](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Samouczek krok po kroku, który przeprowadzi Cię przez proces tworzenia aplikacji sieci web, który korzysta z uwierzytelniania, przechowuje aplikacji tabele w bazie danych członkostwa modyfikuje schemat bazy danych i aplikacja jest wdrażana na platformie Azure.
- [Mapa zawartości dostępu do danych w programie ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Linki do zasobów do pracy z EF i bazy danych SQL.

Na platformie Azure przy użyciu bazy danych MongoDB:

- [MongoLab — bazy danych MongoDB na platformie Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Strona Portal dokumentacji o uruchamianiu bazy danych MongoDB na platformie Azure.
- [Utwórz witrynę sieci web platformy Azure, łączącego się bazy danych MongoDB uruchomione na maszynie wirtualnej na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Samouczek krok po kroku przedstawiono sposób użycia bazy danych MongoDB w aplikacji sieci web ASP.NET.

HDInsight (Hadoop na platformie Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Do dokumentacji usługi HDInsight w portalu [Azure](https://azure.microsoft.com/) witryny sieci Web.
- [Hadoop oraz usługi HDInsight: danych Big Data na platformie Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artykuł MSDN Magazine Bruno Terkaly i Villalobos Ricardo, wprowadzenie do platformy Hadoop w systemie Azure.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz MapReduce wzorca.

> [!div class="step-by-step"]
> [Poprzednie](single-sign-on.md)
> [dalej](data-partitioning-strategies.md)
