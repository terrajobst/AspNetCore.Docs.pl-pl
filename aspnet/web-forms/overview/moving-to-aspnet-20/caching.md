---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Buforowanie | Dokumentacja firmy Microsoft
author: microsoft
description: Opis buforowania jest ważne dla dobrze wydajności aplikacji ASP.NET. ASP.NET 1.x oferowane w trzech różnych opcji buforowania; dane wyjściowe buforowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 90faaae75cc85585efa05e6e50eabe8c990d076e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="caching"></a>Buforowanie
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Opis buforowania jest ważne dla dobrze wydajności aplikacji ASP.NET. ASP.NET 1.x oferowane w trzech różnych opcji buforowania; buforowanie danych wyjściowych, buforowanie fragmentu i pamięć podręczną interfejsu API.


Opis buforowania jest ważne dla dobrze wydajności aplikacji ASP.NET. ASP.NET 1.x oferowane w trzech różnych opcji buforowania; buforowanie danych wyjściowych, buforowanie fragmentu i pamięć podręczną interfejsu API. Wszystkie te trzy metody oferuje ASP.NET 2.0, ale dodaje niektóre istotne dodatkowe funkcje. Istnieje kilka nowych zależności buforu i deweloperzy mają teraz opcję, aby utworzyć również zależności buforu niestandardowych. Konfiguracja buforowania również ulepszono w programie ASP.NET 2.0.

## <a name="new-features"></a>Nowe funkcje

## <a name="cache-profiles"></a>Profile pamięci podręcznej

Profile pamięci podręcznej umożliwiają deweloperom do definiowania ustawień określonych w pamięci podręcznej, które można zastosować do pojedynczych stron. Na przykład jeśli masz kilka stron, które powinny być wygasł z pamięci podręcznej po 12 godzinach, łatwo utworzyć profil pamięci podręcznej, który można zastosować do tych stron. Aby dodać nowy profil pamięci podręcznej, użyj &lt;outputCacheSettings&gt; sekcji w pliku konfiguracji. Na przykład poniżej jest konfiguracja profilu pamięci podręcznej o nazwie *twoday* który konfiguruje czas trwania pamięci podręcznej 12 godzin.

[!code-xml[Main](caching/samples/sample1.xml)]

Aby zastosować ten profil pamięci podręcznej do określonej strony, użyj atrybutu CacheProfile dyrektywy @ OutputCache, jak pokazano poniżej:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Zależności buforu niestandardowych

Deweloperów platformy ASP.NET 1.x Zawołał: dla zależności buforu niestandardowych. W programie ASP.NET 1.x, klasa CacheDependency został zapieczętowany uniemożliwił deweloperom z pochodny własnych klas. W programie ASP.NET 2.0 że ograniczenie zostało usunięte i deweloperzy mogą się do opracowywania własnych zależności buforu niestandardowych. Klasa CacheDependency umożliwia tworzenie zależność niestandardowych pamięci podręcznej na podstawie plików, katalogów lub kluczy pamięci podręcznej.

Na przykład poniższy kod tworzy zależność niestandardowych pamięci podręcznej na podstawie pliku o nazwie stuff.xml znajdujący się w katalogu głównym aplikacji sieci Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

W tym scenariuszu po zmianie pliku stuff.xml, unieważniona element pamięci podręcznej.

Istnieje również możliwość utworzenia zależność niestandardowych pamięci podręcznej przy użyciu kluczy pamięci podręcznej. Za pomocą tej metody, usuwania klucza pamięci podręcznej unieważni dane buforowane. Poniższy przykład przedstawia to:

[!code-csharp[Main](caching/samples/sample4.cs)]

Unieważnianie elementu, który został wstawiony powyżej, po prostu usuń element została umieszczona w pamięci podręcznej do działania jako klucz pamięci podręcznej.

[!code-csharp[Main](caching/samples/sample5.cs)]

Należy pamiętać, że klucza elementu, który działa jako klucz pamięci podręcznej musi być taka sama jak wartość dodawane do tablicy kluczy pamięci podręcznej.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Zależności buforu SQL na podstawie sondowania<em>(nazywanych również na podstawie tabeli zależności)</em>

SQL Server 7 i 2000 przy użyciu modelu na podstawie sondowania zależności buforu SQL. Model na podstawie sondowania używa wyzwalacza w tabeli bazy danych, która jest wyzwalane, gdy zmiana danych w tabeli. Który wyzwolić aktualizacje **changeId** tabeli powiadomienia, który aplikacja ASP.NET sprawdza okresowo. Jeśli **changeId** pola zostały zaktualizowane, ASP.NET wie, że dane zostały zmienione, a jego unieważnia dane buforowane.

> [!NOTE]
> SQL Server 2005 można również użyć modelu na podstawie sondowania, ale na podstawie sondowania modelu nie jest najbardziej wydajnym modelu, zaleca się oparte na zapytaniach modelu (omówione później) za pomocą programu SQL Server 2005.


Aby dla zależności bufora SQL, za pomocą modelu na podstawie sondowania działał prawidłowo tabele muszą mieć włączone powiadomienia. Można to osiągnąć, programowo przy użyciu klasy SqlCacheDependencyAdmin lub za pomocą aspnet\_regsql.exe narzędzia.

Następujące polecenie w wierszu rejestruje tabeli Produkty bazy danych Northwind znajduje się w wystąpieniu programu SQL Server o nazwie *dbase* dla bazy danych SQL pamięci podręcznej zależności.

[!code-console[Main](caching/samples/sample6.cmd)]

Oto objaśnienia dotyczące przełączników wiersza polecenia używane w poleceniu powyżej:

| **Przełącznik wiersza polecenia** | **Cel** |
| --- | --- |
| -S *serwera* | Określa nazwę serwera. |
| -ed | Określa, że bazy danych powinno być włączone dla zależności bufora SQL. |
| -d *bazy danych\_nazwy* | Określa nazwę bazy danych, które powinny być włączone dla zależności bufora SQL. |
| -E | Określa, że aspnet\_regsql należy używać uwierzytelniania systemu Windows podczas łączenia z bazą danych. |
| -et | Określa włączenia tabeli bazy danych dla zależności bufora SQL. |
| -t *tabeli\_nazwy* | Określa nazwę tabeli bazy danych, aby włączyć dla zależności bufora SQL. |

> [!NOTE]
> Dostępne są inne przełączniki dla aspnet\_regsql.exe. Aby uzyskać pełną listę, uruchom aspnet\_regsql.exe-? z wiersza polecenia.


Po uruchomieniu tego polecenia bazy danych programu SQL Server zostały wprowadzone następujące zmiany:

- **AspNet\_SqlCacheTablesForChangeNotification** dodaje tabelę. Ta tabela zawiera jeden wiersz dla każdej tabeli w bazie danych, dla którego włączono zależności bufora SQL.
- Poniższe procedury składowane są tworzone wewnątrz bazy danych:


| AspNet\_SqlCachePollingStoredProcedure | Wysyła zapytanie AspNet\_SqlCacheTablesForChangeNotification tabeli i zwraca wszystkich tabel włączonych dla zależności bufora SQL i wartość changeId dla każdej tabeli. To przechowywanej służy do sondowania w celu określenia, czy dane zostały zmienione. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Zwraca wszystkich tabel włączonych dla zależności bufora SQL, badając AspNet\_SqlCacheTablesForChangeNotification tabeli i zwraca wszystkie tabele włączone dla bazy danych SQL pamięci podręcznej zależności. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Rejestruje tabelę dla zależności bufora SQL, dodając wymaganego wpisu w tabeli powiadomień i dodaje wyzwalacza. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Wyrejestrowuje tabelę dla zależności bufora SQL przez usunięcie wpisu w tabeli powiadomień i usuwa wyzwalacz. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualizuje tabeli powiadomienia zwiększając changeId zmienione tabeli. ASP.NET używa tej wartości, aby określić, czy dane zostały zmienione. Jak pokazano poniżej, to przechowywanej jest wykonywana przez wyzwalacz utworzony po włączeniu tabeli. |


- Wyzwalacz programu SQL Server o nazwie ***tabeli\_nazwa *\_AspNet\_SqlCacheNotification\_wyzwalacza** jest tworzony dla tabeli. Wyzwalacz wykonuje AspNet\_SqlCacheUpdateChangeIdStoredProcedure podczas INSERT, UPDATE lub DELETE jest wykonywana na tabeli.
- Rola serwera SQL o nazwie **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** zostanie dodany do bazy danych.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** roli programu SQL Server ma uprawnienia EXEC do AspNet\_SqlCachePollingStoredProcedure. Aby modelu sondowania działał prawidłowo, należy dodać konto procesu do aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess roli. Aspnet\_narzędzia regsql.exe nie będzie to za Ciebie.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurowanie zależności buforu SQL na podstawie sondowania

Istnieje kilka czynności, które są wymagane do konfigurowania na podstawie sondowania zależności buforu SQL. Pierwszym krokiem jest umożliwienie bazy danych i tabeli zgodnie z powyższym opisem. Po wykonaniu tego kroku pozostałą część konfiguracji jest następujący:

- Konfigurowanie pliku konfiguracyjnego programu ASP.NET.
- Konfigurowanie SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurowanie pliku konfiguracji platformy ASP.NET

Oprócz dodania ciąg połączenia, zgodnie z opisem w poprzednim module, musisz również skonfigurować &lt;pamięci podręcznej&gt; element z &lt;sqlCacheDependency&gt; element, jak pokazano poniżej:

[!code-xml[Main](caching/samples/sample7.xml)]

Ta konfiguracja umożliwia zależności bufora SQL na *pubs* bazy danych. Należy pamiętać, że atrybutu pollTime w &lt;sqlCacheDependency&gt; element domyślne 60000 milisekund lub 1 minutę. (Ta wartość nie może być mniej niż 500 ms). W tym przykładzie &lt;dodać&gt; elementu dodaje nową bazę danych i zastępuje pollTime, ustawieniem dla niego 9000000 milisekund.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurowanie SqlCacheDependency

Następnym krokiem jest skonfigurowanie SqlCacheDependency. Najprostszy sposób realizacji tego jest określenie wartości Atrybut SqlDependency w dyrektywie @ Outcache w następujący sposób:

[!code-aspx[Main](caching/samples/sample8.aspx)]

W powyższej dyrektywy @ OutputCache zależności bufora SQL jest skonfigurowany dla *autorów* tabeli w *pubs* bazy danych. Można skonfigurować wiele zależności, oddzielając je średnikami w następujący sposób:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Inną metodą konfiguracji SqlCacheDependency jest programistycznych. Poniższy kod tworzy nowe zależności bufora SQL na *autorów* tabeli w *pubs* bazy danych.

[!code-csharp[Main](caching/samples/sample10.cs)]

Jedną z zalet programowane Definiowanie zależności bufora SQL jest, że może obsługiwać wszystkie wyjątki, które mogą wystąpić. Na przykład, jeśli podjęto próbę zdefiniowania zależności bufora SQL dla bazy danych, który nie został włączony dla powiadomień **databasenotenabledfornotificationexception —** zostanie wygenerowany wyjątek. W takim przypadku można spróbować włączyć dla powiadomień bazy danych przez wywołanie metody **SqlCacheDependencyAdmin.EnableNotifications** — metoda i przekazanie jej nazwa bazy danych.

Podobnie, jeśli podjęto próbę zdefiniowania zależności bufora SQL w tabeli, która nie została włączona dla powiadomień, **tablenotenabledfornotificationexception —** zostanie wygenerowany. Następnie można wywołać **SqlCacheDependencyAdmin.EnableTableForNotifications** metody przekazanie jej nazwa bazy danych i nazwy tabeli.

Poniższy przykładowy kod przedstawia sposób poprawnie skonfigurować Obsługa wyjątków podczas konfigurowania zależności bufora SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Zależności buforu SQL na podstawie kwerendy (dotyczy tylko programu SQL Server 2005)

W przypadku używania programu SQL Server 2005 dla zależności bufora SQL, na podstawie sondowania modelu nie jest konieczne. W przypadku użycia z programem SQL Server 2005, zależności buforu SQL komunikować się bezpośrednio przy użyciu połączeń z serwerem SQL do wystąpienia programu SQL Server (nie trzeba konfigurować niezbędnym) przy użyciu powiadomienia kwerendy SQL Server 2005.

Najprostszym sposobem, aby włączyć powiadamianie bazujących na zapytaniu jest więc deklaratywnie przez ustawienie **SqlCacheDependency** atrybutu obiektu źródła danych do **CommandNotification** i ustawiania **EnableCaching** atrybutu **true**. Za pomocą tej metody, żaden kod nie jest wymagana. Wynik polecenia wykonywane względem danych zmiany źródła, unieważni dane pamięci podręcznej.

Poniższy przykład konfiguruje formantu źródła danych dla zależności bufora SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

W tym przypadku zapytania określony w **SelectCommand** zwraca różne wyniki niż została oryginalnie, wyniki, buforowane są nieważne.

Można również określić, czy wszystkie źródła danych być włączona dla zależności buforu SQL **SqlDependency** atrybutu **@ OutputCache** dyrektywy do **CommandNotification** . W poniższym przykładzie przedstawiono to.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących powiadomień o zapytaniach w programie SQL Server 2005 zobacz SQL Server — książki Online.


Inną metodą konfiguracji oparte na zapytaniach zależności bufora SQL jest w tym celu programowo przy użyciu klasy elementu SqlCacheDependency w wyniku. Poniższy przykładowy kod przedstawia, jak to zrobić.

[!code-csharp[Main](caching/samples/sample14.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Post-Cache Substitution

Buforowanie strony mogą znacznie zwiększyć wydajność aplikacji sieci Web. Jednak w niektórych przypadkach należy większość strony pamięci podręcznej i niektóre fragmenty wewnątrz strony dynamiczne. Na przykład jeśli tworzysz stronę wiadomości, która jest całkowicie statycznych dla ustawione okresów, można ustawić całej strony pamięci podręcznej. Chcąc Dołącz obracania transparent ad, które zmienione na każdym żądaniu strony, część strony zawierającej anons musi być dynamiczny. Aby umożliwić w pamięci podręcznej strony, ale Zastąp zawartość dynamicznie, można użyć podstawiania po pamięci podręcznej programu ASP.NET. Z podstawienia pamięci podręcznej po całej strony jest z określonych części oznaczona jako wykluczona z buforowania w pamięci podręcznej danych wyjściowych. W tym przykładzie transparentach ad kontroli AdRotator umożliwia korzystać z pamięci podręcznej po podstawienia tak, aby reklam tworzone dynamicznie dla każdego użytkownika i dla każdej odświeżania strony.

Istnieją trzy sposoby wykonania podstawienia po pamięci podręcznej:

- Deklaratywnie za pomocą kontrolki zastępczej.
- Programowo za pomocą kontrolki zastępczej interfejsu API.
- Niejawnie przy użyciu formantu AdRotator.

### <a name="substitution-control"></a>Kontrolki zastępczej

Kontrolki zastępczej ASP.NET określa sekcję buforowana strona, która jest tworzona dynamicznie, a nie w pamięci podręcznej. Kontrolki zastępczej należy umieścić w lokalizacji na stronie, w którym ma się pojawić zawartość dynamiczna. W czasie wykonywania kontrolki zastępczej wywołuje metodę określić za pomocą właściwości MethodName. Metoda musi zwracać ciąg, który zastąpi zawartość kontrolki zastępczej. Metoda musi być metodą statyczną w formancie Page lub UserControl zawierającego. Za pomocą kontrolki zastępczej powoduje, że buforowanie po stronie klienta, należy zmienić buforowanie serwera, dzięki czemu strony nie będą buforowane na kliencie. Dzięki temu, że przyszłych żądań do strony Wywołaj metodę ponownie do generowania zawartości dynamicznej.

### <a name="substitution-api"></a>Podstawienie interfejsu API

Aby utworzyć dynamicznej zawartości buforowana strona programowo, można wywołać [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metody w kodzie strony, przekazując go nazwą metody jako parametr. Metoda, która obsługuje tworzenie dynamicznej zawartości przyjmuje jeden [element HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametrów i zwraca wartość typu ciąg. Zwracany ciąg ma zawartość, która zostanie zastąpiona w podanej lokalizacji. Zaletą wywołanie metody WriteSubstitution zamiast kontrolki zastępczej deklaratywnie jest, czy należy wywołać metodę z dowolnych obiektów zamiast wywołanie metody statycznej strony lub obiektu UserControl.

Wywołanie metody WriteSubstitution powoduje, że buforowanie po stronie klienta, należy zmienić buforowanie serwera, dzięki czemu strony nie będą buforowane na kliencie. Dzięki temu, że przyszłych żądań do strony Wywołaj metodę ponownie do generowania zawartości dynamicznej.

### <a name="adrotator-control"></a>AdRotator formantu

AdRotator, który implementuje kontrolki serwera obsługę pamięci podręcznej po podstawienia wewnętrznie. Jeśli umieścisz AdRotator kontrolki na stronie wyświetli anonsów unikatowe na każdym żądaniu, niezależnie od tego, czy jest buforowana Strona nadrzędna. W związku z tym strona, która zawiera formant AdRotator jest tylko pamięci podręcznej po stronie serwera.

## <a name="controlcachepolicy-class"></a>Klasa ControlCachePolicy

Klasa ControlCachePolicy umożliwia sterowanie programowe fragmentu buforowanie za pomocą kontrolki użytkownika. ASP.NET osadza kontrolek użytkownika w ramach [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) wystąpienia. Klasa BasePartialCachingControl reprezentuje kontrolkę użytkownika, który ma produkt wyjściowy włączone buforowanie.

Podczas uzyskiwania dostępu [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) właściwość [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) kontroli, zawsze otrzymasz prawidłowy obiekt ControlCachePolicy. Jednak jeśli dostęp do [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) właściwość [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) kontroli, zostanie wyświetlony prawidłowy obiekt ControlCachePolicy tylko wtedy, gdy formant użytkownika jest już opakowana przez Formant BasePartialCachingControl. Jeśli nie jest on zawijany, ControlCachePolicy obiekcie zwracanym przez właściwość zostanie zgłaszają wyjątki, gdy użytkownik podejmie próbę manipulowania on, ponieważ nie ma skojarzonego BasePartialCachingControl. Aby ustalić, czy wystąpienie UserControl obsługuje buforowanie bez generowania wyjątków, sprawdź [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) właściwości.

Za pomocą klasy ControlCachePolicy jest kilka sposobów, aby umożliwić buforowanie danych wyjściowych. Poniższa lista zawiera opis metody, których można użyć, aby włączyć buforowanie danych wyjściowych:

- Użyj [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) output dyrektywy, aby włączyć buforowanie w scenariuszach deklaratywne.
- Użyj [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atrybutu, aby włączyć buforowanie dla formantu użytkownika w pliku CodeBehind.
- Użyj klasy ControlCachePolicy, aby określić ustawienia pamięci podręcznej w scenariuszach programowe, w których użytkownik pracuje z wystąpieniami BasePartialCachingControl, które zostały pamięci podręcznej obsługą przy użyciu jednej z powyższych metod i dynamicznie ładowane przy użyciu [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) metody.

Wystąpienie ControlCachePolicy może manipulować pomyślnie tylko między Init i PreRender etapy cyklu życia formantu. Jeśli zmodyfikujesz obiektu ControlCachePolicy po fazie PreRender ASP.NET zgłasza wyjątek, ponieważ wszelkie zmiany dokonane po renderowania formantu rzeczywiście nie ma wpływu na ustawienia pamięci podręcznej (formantu jest buforowana na etapie renderowania). Na koniec wystąpienie kontrolki użytkownika (i w związku z tym obiektem ControlCachePolicy) jest dostępny tylko dla programowe manipulowania faktycznie jest renderowany.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Zmiany w konfiguracji buforowania — &lt;buforowanie&gt; — Element

Istnieje kilka zmian w pamięci podręcznej konfiguracji w programie ASP.NET 2.0. &lt;Buforowanie&gt; element jest nowe w programie ASP.NET 2.0 i pozwala na zapewnienie buforowania zmiany konfiguracji w pliku konfiguracji. Następujące atrybuty są dostępne.

| **Element** | **Opis** |
| --- | --- |
| **cache** | Element opcjonalny. Określa ustawienia pamięci podręcznej globalnych aplikacji. |
| **outputCache** | Element opcjonalny. Określa ustawienia pamięci podręcznej danych wyjściowych całej aplikacji. |
| **outputCacheSettings** | Element opcjonalny. Określa ustawienia pamięci podręcznej danych wyjściowych, które można zastosować do stron w aplikacji. |
| **sqlCacheDependency** | Element opcjonalny. Konfiguruje zależności buforu SQL dla aplikacji ASP.NET. |

### <a name="the-ltcachegt-element"></a>&lt;Pamięci podręcznej&gt; — Element

Następujące atrybuty są dostępne w &lt;pamięci podręcznej&gt; elementu:

| **Atrybut** | **Opis** |
| --- | --- |
| **disableMemoryCollection** | Opcjonalne **logiczna** atrybutu. Pobiera lub ustawia wartość wskazującą, czy kolekcja pamięci podręcznej, która występuje, gdy komputer jest wykorzystanie pamięci jest wyłączone. |
| **disableExpiration** | Opcjonalne **logiczna** atrybutu. Pobiera lub ustawia wartość wskazującą, czy wygaśnięcia pamięci podręcznej jest wyłączone. Po wyłączeniu pamięci podręcznej elementów nie wygasają i tła oczyszczania elementów wygasłych pamięci podręcznej nie występuje. |
| **privateBytesLimit** | Opcjonalne **Int64** atrybutu. Pobiera lub ustawia wartość wskazująca, że maksymalna liczba bajtów prywatnych aplikacji przed rozpoczęciem pamięci podręcznej opróżnianie elementy wygasłe i próby odzyskania pamięci. Ten limit obejmuje zarówno pamięci używanej przez pamięć podręczną, a także normalne pamięci narzut na podstawie uruchomionej aplikacji. Wartość 0 wskazuje, że ASP.NET będzie korzystać własną heurystyki określania, kiedy należy uruchomić odzyskiwanie pamięci. |
| **percentagePhysicalMemoryUsedLimit** | Opcjonalne **Int32** atrybutu. Pobiera lub ustawia wartość wskazująca, że elementy wygasłe maksymalnej wartości procentowej pamięci fizycznej na komputerze, które mogą być używane przez aplikację, przed rozpoczęciem pamięci podręcznej opróżnianie i próby odzyskania pamięci to użycie pamięci obejmuje zarówno pamięci używanych przez pamięć podręczną również jako użycie pamięci normalne działającej aplikacji. Wartość 0 wskazuje, że ASP.NET będzie korzystać własną heurystyki określania, kiedy należy uruchomić odzyskiwanie pamięci. |
| **privateBytesPollTime** | Opcjonalne **TimeSpan** atrybutu. Pobiera lub ustawia wartość określającą interwał sondowania użycia pamięci bajtów prywatnych aplikacji. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; — Element

Następujące atrybuty są dostępne dla &lt;outputCache&gt; elementu.


|       <strong>Atrybut</strong>        |                                                                                                                                                                                                                                                       <strong>Opis</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Opcjonalne <strong>logiczna</strong> atrybutu. Włącza/wyłącza pamięci podręcznej danych wyjściowych strony. Jeśli wyłączone, niezależnie od ustawień programistycznych albo zezwala na deklaratywne nie są buforowane nie ma żadnych stron. Wartość domyślna to <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Opcjonalne <strong>logiczna</strong> atrybutu. Włącza/wyłącza pamięci podręcznej fragmentu aplikacji. Jeśli wyłączona, żadne strony nie są buforowane bez względu na to [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) dyrektywa lub profil używany do buforowania. Zawiera nagłówek z kontroli pamięci podręcznej wskazującą, czy serwery proxy nadrzędnego, jak również klientów w przeglądarkach nie powinny podejmować próby pamięci podręcznej danych wyjściowych strony. Wartość domyślna to <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Opcjonalne <strong>logiczna</strong> atrybutu. Pobiera lub ustawia wartość wskazującą czy <strong>pamięci podręcznej-kontrolki: prywatne</strong> nagłówka domyślnie wysyłane przez moduł wyjściowej pamięci podręcznej. Wartość domyślna to <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Opcjonalne <strong>logiczna</strong> atrybutu. Włącza/wyłącza wysyłanie Http "<strong>Vary: \</ strong ><em>" nagłówka w odpowiedzi. Z domyślnym ustawieniem false, "</em>* Vary: \* <strong>" nagłówka jest wysyłane do stron pamięci podręcznej danych wyjściowych. Po wysłaniu nagłówka Vary umożliwia dla innej wersji można buforować oparte na nazwie określonej w nagłówka Vary. Na przykład <em>Vary: użytkownik-agentów</em> będzie przechowywać różne wersje strony oparte na agenta użytkownika żądania. Wartość domyślna to ** false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; — Element

&lt;OutputCacheSettings&gt; element umożliwia tworzenie profilów pamięci podręcznej w sposób opisany wcześniej. Element podrzędny tylko dla &lt;outputCacheSettings&gt; jest element &lt;outputCacheProfiles&gt; elementu Konfigurowanie profilów pamięci podręcznej.

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt; — Element

Następujące atrybuty są dostępne dla &lt;sqlCacheDependency&gt; elementu.

| **Atrybut** | **Opis** |
| --- | --- |
| **włączone** | Wymagane **logiczna** atrybutu. Wskazuje, czy zmiany są trwa sondowania. |
| **pollTime** | Opcjonalne **Int32** atrybutu. Określa częstotliwość, z którym SqlCacheDependency sonduje w tabeli bazy danych zmiany. Ta wartość odpowiada liczba milisekund między kolejnymi pollings. Nie można ustawić milisekund mniej niż 500. Wartość domyślna to 1 minuta. |

### <a name="more-information"></a>Więcej informacji

Brak niektórych dodatkowe informacje, które należy pamiętać o dotyczące konfiguracji pamięci podręcznej.

- Jeśli nie ustawiono limit bajtów prywatnych procesu roboczego, pamięci podręcznej użyje jednego z następujących ograniczeń: 

    - x86 2 GB: 800 MB lub 60% fizycznej pamięci RAM, ta wartość jest mniejsza
    - x86 3 GB: 1800 MB lub 60% fizycznej pamięci RAM, ta wartość jest mniejsza
    - x 64: 1 terabajt lub 60% fizycznej pamięci RAM ta wartość jest mniejsza
- Jeśli prywatne procesu roboczego zarówno ograniczyć bajtów i &lt;pamięci podręcznej privateBytesLimit /&gt; skonfigurowano pamięci podręcznej będzie używać co najmniej dwa.
- Podobnie jak w 1.x, możemy usunąć wpisy w pamięci podręcznej i Wywołaj GC. Zbierz informacje o dwóch powodów: 

    - Bardzo jesteśmy zbliża się limit bajtów prywatnych
    - Ilość dostępnej pamięci jest blisko lub mniej niż 10%
- Można skutecznie wyłączyć przycinanie i pamięci podręcznej dla warunków braku dostępnej pamięci, ustawiając &lt;pamięci podręcznej percentagePhysicalMemoryUseLimit /&gt; do 100.
- W odróżnieniu od 1.x, 2.0 wstrzyma wywołania przycinanie i zbieranie, jeśli ostatniej operacji GC. Zbieranie nie Zmniejsz bajtów prywatnych lub zarządzanych stosów przez więcej niż 1% limitu pamięci (pamięć podręczna).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Zależności buforu niestandardowych

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik XML o nazwie cache.xml i zapisz go na katalog główny aplikacji sieci Web.
3. Dodaj następujący kod do strony\_obciążenia w kodem default.aspx metody: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Dodaj następujący element do góry default.aspx w widoku źródła: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Przeglądaj Default.aspx. Co to powiedzieć czas?
6. Odświeżyć przeglądarkę. Co to powiedzieć czas?
7. Otwórz cache.xml i Dodaj następujący kod: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Zapisz cache.xml.
9. Odśwież przeglądarkę. Co to powiedzieć czas?
10. Wyjaśnić, dlaczego Godzina aktualizacji zamiast buforowanych wartości:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorium 2: Zależności buforu na podstawie sondowania przy użyciu

To laboratorium korzysta z projektu, który został utworzony w poprzednim module, umożliwiający edytowanie danych w bazie danych Northwind za pomocą formantu widoku GridView i widoku DetailsView.

1. Otwórz projekt w programie Visual Studio 2005.
2. Uruchom aspnet\_regsql Narzędzia bazy danych Northwind, aby włączyć bazę danych i tabeli Produkty. Użyj następującego polecenia z programu Visual Studio wiersz polecenia: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Dodaj następującą wartość do pliku web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Dodaj nowy formularz sieci Web o nazwie showdata.aspx.
5. Dodaj następujące @ outputcache dyrektywy do strony showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Dodaj następujący kod do strony\_obciążenia showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Dodaj nową kontrolkę SqlDataSource do showdata.aspx i skonfigurować go do używania połączenia z bazą danych Northwind. Kliknij przycisk Dalej.
8. Zaznacz pola wyboru ProductName i identyfikator produktu, a następnie kliknij przycisk Dalej.
9. Kliknij przycisk Zakończ.
10. Na stronie showdata.aspx, Dodaj nowy element GridView.
11. Wybierz SqlDataSource1 z listy rozwijanej.
12. Zapisz i Przeglądaj showdata.aspx. Zanotuj wyświetlany czas.
