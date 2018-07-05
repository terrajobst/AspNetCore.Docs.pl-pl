---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Pamięć podręczna | Dokumentacja firmy Microsoft
author: microsoft
description: Zrozumienie buforowania jest ważne dla właściwie wykonanego aplikacji ASP.NET. ASP.NET 1.x oferowane trzy różne opcje do buforowania; dane wyjściowe, buforowanie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f52a88680db54de6271b17bd52cbdace66425e9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387702"
---
<a name="caching"></a>Pamięć podręczna
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Zrozumienie buforowania jest ważne dla właściwie wykonanego aplikacji ASP.NET. ASP.NET 1.x oferowane trzy różne opcje do buforowania; buforowanie danych wyjściowych, buforowanie fragmentu i interfejsu API w pamięci podręcznej.


Zrozumienie buforowania jest ważne dla właściwie wykonanego aplikacji ASP.NET. ASP.NET 1.x oferowane trzy różne opcje do buforowania; buforowanie danych wyjściowych, buforowanie fragmentu i interfejsu API w pamięci podręcznej. Platformy ASP.NET 2.0 udostępnia wszystkie te trzy metody, ale dodaje niektóre istotne dodatkowe funkcje. Istnieje kilka nowych zależności pamięci podręcznej i deweloperzy mają teraz opcję, aby utworzyć również zależności niestandardowej pamięci podręcznej. Konfiguracja buforowania została również ulepszona znacznie programu ASP.NET 2.0.

## <a name="new-features"></a>Nowe funkcje

## <a name="cache-profiles"></a>Profile pamięci podręcznej

Profile pamięci podręcznej umożliwia deweloperom Definiowanie ustawień określonych pamięci podręcznej, które następnie mogą być stosowane do poszczególnych stron. Na przykład w przypadku niektórych stron, które powinny wygasnąć z pamięci podręcznej po 12 godzinach łatwo utworzyć profil pamięci podręcznej, który można zastosować do tych stron. Aby dodać nowy profil pamięci podręcznej, użyj &lt;outputCacheSettings&gt; sekcji w pliku konfiguracji. Na przykład poniżej przedstawiono konfigurację z profilu pamięci podręcznej o nazwie *twoday* , konfiguruje czas trwania pamięci podręcznej 12 godzin.

[!code-xml[Main](caching/samples/sample1.xml)]

Aby zastosować ten profil pamięci podręcznej na danej stronie, użyj atrybutu CacheProfile dyrektywy @ OutputCache, jak pokazano poniżej:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Zależności niestandardowej pamięci podręcznej

Deweloperzy 1.x ASP.NET Zawołał: dla zależności niestandardowej pamięci podręcznej. W programie ASP.NET: 1.x, klasa CacheDependency została zapieczętowana który uniemożliwił deweloperów od elementu pochodnego dla własnych klas go. W programie ASP.NET 2.0 tego ograniczenia jest usuwany, a deweloperzy są bezpłatne do opracowywania własnych zależności niestandardowej pamięci podręcznej. Klasa CacheDependency pozwala na tworzenie zależności niestandardowej pamięci podręcznej oparte na plikach, katalogach i kluczy pamięci podręcznej.

Na przykład poniższy kod tworzy zależność niestandardowej pamięci podręcznej na podstawie pliku o nazwie stuff.xml znajduje się w katalogu głównym aplikacji sieci Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

W tym scenariuszu po zmianie pliku stuff.xml unieważniony element pamięci podręcznej.

Istnieje również możliwość tworzenia zależności niestandardowej pamięci podręcznej przy użyciu kluczy pamięci podręcznej. Za pomocą tej metody, usunięcie klucza pamięci podręcznej unieważni dane buforowane. Ilustruje to poniższy przykład:

[!code-csharp[Main](caching/samples/sample4.cs)]

Unieważnianie elementu, który został wstawiony powyżej, po prostu usunąć element, który został wstawiony w pamięci podręcznej jako klucz w pamięci podręcznej.

[!code-csharp[Main](caching/samples/sample5.cs)]

Należy zauważyć, że klucz elementu, który działa jako klucz w pamięci podręcznej musi być taka sama jak wartość dodawane do tablicy kluczy pamięci podręcznej.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Na podstawie sondowania zależności pamięci podręcznej SQL<em>(nazywane również opartą na tabeli zależności)</em>

SQL Server 7 i 2000 na użytek model oparty na sondowanie zależności pamięci podręcznej SQL. Model oparty na sondowanie używa wyzwalacza w tabeli bazy danych, który jest wyzwalany, gdy zmienią się dane w tabeli. Który wyzwolić aktualizacje **changeId** w tabeli powiadomienie, że aplikacja ASP.NET sprawdza okresowo. Jeśli **changeId** pola zostały zaktualizowane, ASP.NET wie, że dane zostały zmienione, a jego unieważnia buforowane dane.

> [!NOTE]
> SQL Server 2005, można również użyć modelu opartego na sondowania, ale ponieważ model oparty na sondowanie nie jest najbardziej efektywny sposób modelu, zaleca się model oparty na zapytaniu (omówione w dalszej części) za pomocą programu SQL Server 2005.


Zależności pamięci podręcznej SQL przy użyciu modelu opartego na sondowanie działała prawidłowo, tabele musi mieć włączone powiadomienia. Można to zrobić programowo przy użyciu klasy SqlCacheDependencyAdmin lub za pomocą aspnet\_regsql.exe narzędzia.

Rejestruje następujące polecenie w wierszu tabeli Produkty bazy danych Northwind, znajduje się w wystąpieniu programu SQL Server o nazwie *dbase* SQL w pamięci podręcznej zależności.

[!code-console[Main](caching/samples/sample6.cmd)]

Poniżej przedstawiono omówienie przełączników wiersza polecenia używane w poleceniu powyżej:

| **Przełącznik wiersza polecenia** | **Cel** |
| --- | --- |
| -S *serwera* | Określa nazwę serwera. |
| -ed | Określa, że baza danych powinna być włączona dla zależności pamięci podręcznej SQL. |
| -d *bazy danych\_nazwy* | Określa nazwę bazy danych, która powinna być włączona dla zależności pamięci podręcznej SQL. |
| -E | Określa, że aspnet\_regsql należy używać uwierzytelniania Windows, podczas nawiązywania połączenia z bazą danych. |
| -et | Określa, że umożliwiamy tabeli bazy danych dla zależności pamięci podręcznej SQL. |
| -t *tabeli\_nazwy* | Określa nazwę tabeli bazy danych, aby włączyć dla zależności pamięci podręcznej SQL. |

> [!NOTE]
> Dostępne są inne przełączniki dla aspnet\_regsql.exe. Aby uzyskać pełną listę, należy uruchomić aspnet\_regsql.exe-? z poziomu wiersza polecenia.


Po uruchomieniu tego polecenia następujące zmiany zostały wprowadzone do bazy danych programu SQL Server:

- **AspNet\_SqlCacheTablesForChangeNotification** tabela zostanie dodana. Ta tabela zawiera jeden wiersz dla każdej tabeli w bazie danych, dla którego włączono zależności pamięci podręcznej SQL.
- Następujące procedury przechowywane są tworzone wewnątrz bazy danych:


| AspNet\_SqlCachePollingStoredProcedure | Wysyła kwerendę AspNet\_SqlCacheTablesForChangeNotification tabeli i zwraca wszystkie tabele, które są włączone dla zależności pamięci podręcznej SQL i wartość changeId dla każdej tabeli. Ta przechowywanej służy do sondowania w celu określenia, czy dane uległy zmianie. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Zwraca wszystkie tabele włączonych dla zależności pamięci podręcznej SQL, badając AspNet\_SqlCacheTablesForChangeNotification tabeli i zwraca wszystkie tabele są włączone dla programu SQL w pamięci podręcznej zależności. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Rejestruje tabeli dla zależności pamięci podręcznej SQL przez dodanie wymaganego wpisu w tabeli powiadomienia i dodaje wyzwalacza. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Wyrejestrowuje tabeli dla zależności pamięci podręcznej SQL, usuwając wpis w tabeli powiadomień i usuwa wyzwalacz. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualizuje tabelę powiadomień przez zwiększenie changeId zmienione tabeli. ASP.NET używa tej wartości, aby ustalić, czy dane uległy zmianie. Wyszczególnionych poniżej tej wartości przechowywanej jest wykonywana przez wyzwalacz utworzony po włączeniu tabeli. |


- Wyzwalacz programu SQL Server o nazwie ***tabeli\_nazwa *\_AspNet\_SqlCacheNotification\_wyzwalacza** jest tworzony dla tabeli. Ten wyzwalacz jest wykonywany AspNet\_SqlCacheUpdateChangeIdStoredProcedure podczas wykonywania w tabeli INSERT, UPDATE lub DELETE.
- Rola programu SQL Server o nazwie **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** zostanie dodany do bazy danych.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** roli programu SQL Server ma uprawnienia EXEC AspNet\_SqlCachePollingStoredProcedure. Aby modelu sondowania działała prawidłowo, należy dodać swoje konto procesu do aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess roli. Aspnet\_regsql.exe narzędzie nie będzie to dla Ciebie.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurowanie zależności pamięci podręcznej SQL na podstawie sondowania

Istnieje kilka kroków, które są wymagane do skonfigurowania na podstawie sondowania zależności pamięci podręcznej SQL. Pierwszym krokiem jest umożliwienie bazy danych i tabeli, zgodnie z powyższym opisem. Po wykonaniu tego kroku pozostałą część konfiguracji jest następująca:

- Konfigurowanie pliku konfiguracji platformy ASP.NET.
- Konfigurowanie SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurowanie pliku konfiguracji platformy ASP.NET

Oprócz dodawania parametrów połączenia, zgodnie z opisem w poprzednim module, należy również skonfigurować &lt;pamięci podręcznej&gt; element z &lt;sqlCacheDependency&gt; elementu, jak pokazano poniżej:

[!code-xml[Main](caching/samples/sample7.xml)]

Ta konfiguracja umożliwia zależności pamięci podręcznej SQL na *pubs* bazy danych. Należy zauważyć, że pollTime atrybutu w &lt;sqlCacheDependency&gt; element domyślnie 60000 milisekund lub 1 minutę. (Ta wartość nie może być mniejszy niż 500 milisekund). W tym przykładzie &lt;Dodaj&gt; elementu dodaje nową bazę danych i zastępuje pollTime, ustawieniem dla niego 9000000 milisekund.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurowanie SqlCacheDependency

Następnym krokiem jest skonfigurowanie SqlCacheDependency. Najprostszym sposobem wykonania, która jest Określ wartość dla atrybutu SqlDependency w dyrektywie @ Outcache w następujący sposób:

[!code-aspx[Main](caching/samples/sample8.aspx)]

W powyższej dyrektywy @ OutputCache zależności pamięci podręcznej SQL jest skonfigurowany dla *autorzy* tabelę *pubs* bazy danych. Można skonfigurować wiele zależności, oddzielając je średnikami w następujący sposób:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Inną metodą konfiguracji SqlCacheDependency jest programistycznych. Poniższy kod tworzy nowe zależności pamięci podręcznej SQL na *autorzy* tabelę *pubs* bazy danych.

[!code-csharp[Main](caching/samples/sample10.cs)]

Jedną z zalet programowane Definiowanie zależności pamięci podręcznej SQL jest, że może obsługiwać wszystkie wyjątki, które mogą wystąpić. Na przykład, Jeśli spróbujesz do definiowania zależności pamięci podręcznej SQL dla bazy danych, które nie zostały włączone powiadomienia **databasenotenabledfornotificationexception —** zostanie zgłoszony wyjątek. W takim przypadku można spróbować włączyć bazę danych dla powiadomień przez wywołanie **SqlCacheDependencyAdmin.EnableNotifications** metody i przekazanie do niej nazwy bazy danych.

Podobnie, Jeśli spróbujesz do definiowania zależności pamięci podręcznej SQL dla tabeli, która nie została włączona dla powiadomienia **tablenotenabledfornotificationexception —** zostanie zgłoszony. Następnie możesz wywołać **SqlCacheDependencyAdmin.EnableTableForNotifications** metoda przekazanie jej nazwa bazy danych i nazwę tabeli.

Poniższy przykładowy kod ilustruje sposób poprawnie skonfigurować obsługę wyjątków, podczas konfigurowania zależności pamięci podręcznej SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Zależności pamięci podręcznej na podstawie kwerendy SQL (dotyczy tylko programu SQL Server 2005)

Model oparty na sondowania dla zależności pamięci podręcznej SQL, korzystając z programu SQL Server 2005, nie jest konieczne. W przypadku użycia za pomocą programu SQL Server 2005, zależności pamięci podręcznej SQL komunikują się bezpośrednio za pośrednictwem połączeń SQL w wystąpieniu programu SQL Server (bez dalszej konfiguracji jest to konieczne) przy użyciu powiadomienia o zapytaniach programu SQL Server 2005.

Najprostszym sposobem, aby włączyć powiadomienia oparte na zapytaniach jest więc deklaratywne, ustawiając **SqlCacheDependency** atrybut obiektu źródła danych, aby **CommandNotification** i ustawiania **EnableCaching** atrybutu **true**. Metoda ta jest wymagany żaden kod. Jeśli wynik polecenia wykonywane względem danych zmian źródła, unieważni dane pamięci podręcznej.

Poniższy przykład umożliwia skonfigurowanie kontroli źródła danych dla zależności pamięci podręcznej SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

W tym przypadku, gdy zapytanie określone w **SelectCommand** zwraca różne wyniki niż została pierwotnie, wyniki, które są buforowane są nieważne.

Można również określić wszystkie źródła danych włączenia dla zależności pamięci podręcznej SQL, ustawiając **SqlDependency** atrybutu **@ OutputCache** dyrektywę **CommandNotification** . W poniższym przykładzie przedstawiono to.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat powiadomienia zapytań w programie SQL Server 2005 zobacz programu SQL Server — książki Online.


Inną metodą konfigurowania zależności pamięci podręcznej na podstawie kwerendy SQL jest zrobić programowo przy użyciu klasy SqlCacheDependency. Poniższy przykładowy kod przedstawia, jak to zrobić.

[!code-csharp[Main](caching/samples/sample14.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Post-Cache Substitution

Buforowanie strony może znacznie zwiększyć wydajność aplikacji sieci Web. W niektórych przypadkach należy jednak większość strony przechowywanie w pamięci podręcznej i niektóre fragmenty w obrębie strony dynamiczne. Na przykład jeśli utworzysz stronę najnowsze, która jest całkowicie statyczne przez ustawić czas, można ustawić całej strony w pamięci podręcznej. Chcąc Dołącz rotacji transparent ad, która się zmieniła się na każde żądanie strony części strony zawierającej anons musi być dynamiczne. Umożliwia strony w pamięci podręcznej, ale zastąp część zawartości dynamicznej, można użyć zastępczej po pamięci podręcznej ASP.NET. Za pomocą podstawienia pamięci podręcznej po całej strony jest pamięci podręcznej z określonymi częściami oznaczona jako wykluczona z buforowania danych wyjściowych. W przykładzie banery ad kontroli AdRotator umożliwia zalet podstawienia po pamięci podręcznej tak, aby reklam tworzone dynamicznie dla każdego użytkownika i każdym odświeżeniu strony.

Istnieją trzy sposoby wykonania podstawienia po pamięci podręcznej:

- Deklaratywne przy użyciu kontrolki zastępczej.
- Programowo przy użyciu kontrolki zastępczej interfejsu API.
- Niejawnie przy użyciu kontrolki AdRotator.

### <a name="substitution-control"></a>Kontrolki zastępczej

Kontrolki zastępczej ASP.NET określa części strony pamięci podręcznej, który jest tworzony dynamicznie, a nie pamięci podręcznej. W lokalizacji na stronie, w którym ma się pojawić zawartość dynamiczna jest umieszczenie kontrolki zastępczej. W czasie wykonywania kontrolki zastępczej wywołuje metodę, która określić za pomocą właściwości MethodName. Metoda musi zwracać ciąg, który zastąpi zawartość kontrolki zastępczej. Metoda musi być statyczna metoda sterowanie zawierającego kontrolkę użytkownika lub strony. Za pomocą kontrolki zastępczej powoduje, że buforowania po stronie klienta zmienione w celu buforowania server tak, aby strona nie będzie zapisywane na komputerze klienckim. Daje to gwarancję, że przyszłe żądania do strony, wywołaj metodę ponownie do generowania zawartości dynamicznej.

### <a name="substitution-api"></a>Podstawianie interfejsu API

Aby programowo utworzyć zawartości dynamicznej do buforowanej strony, można wywołać [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metody w kodzie strony przekazanie do niej nazwy metody jako parametr. Metoda, która obsługuje tworzenie dynamicznej zawartości przyjmuje jeden [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametr i zwraca ciąg. Ciąg zwracany jest zawartość, która zostanie zastąpiona w podanej lokalizacji. Zaletą wywołanie metody WriteSubstitution zamiast deklaratywne kontrolki zastępczej jest, czy można wywołać metodę dowolnych obiektów zamiast wywołania metody statycznej strony lub obiektu UserControl.

Wywołanie metody WriteSubstitution powoduje, że buforowania po stronie klienta zmienione w celu buforowania server tak, aby strona nie będzie zapisywane na komputerze klienckim. Daje to gwarancję, że przyszłe żądania do strony, wywołaj metodę ponownie do generowania zawartości dynamicznej.

### <a name="adrotator-control"></a>Funkce AdRotator kontroli

Funkce AdRotator, który implementuje formant serwera obsługę pamięci podręcznej po podstawienia wewnętrznie. Jeśli umieścisz kontrolkę AdRotator na Twojej stronie, renderowanie zostanie przeprowadzone w anonse unikatowy dla każdego żądania, niezależnie od tego, czy jest buforowana Strona nadrzędna. W rezultacie strony, który zawiera kontrolkę AdRotator jest tylko pamięci podręcznej po stronie serwera.

## <a name="controlcachepolicy-class"></a>Klasa ControlCachePolicy

Klasa ControlCachePolicy umożliwia programistyczną kontrolę fragmentu pamięci podręcznej za pomocą kontrolki użytkownika. ASP.NET osadza kontrolki użytkownika w ramach [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) wystąpienia. Klasa BasePartialCachingControl reprezentuje kontrolkę użytkownika, który ma produkt wyjściowy włączone buforowanie.

Jeśli uzyskujesz dostęp do [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) właściwość [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) kontrolki, zawsze będzie otrzymywać prawidłowy obiekt ControlCachePolicy. Jednak jeśli dostęp [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) właściwość [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) formant, pojawi się prawidłowy obiekt ControlCachePolicy tylko wtedy, gdy opakowana przez kontrolkę użytkownika Kontrolka BasePartialCachingControl. Jeśli nie opakowaniu obiektu ControlCachePolicy zwrócona przez właściwość będzie zgłaszają wyjątki, gdy użytkownik podejmie próbę manipulowania on, ponieważ nie ma skojarzonego BasePartialCachingControl. Aby ustalić, czy wystąpienie elementu UserControl obsługuje buforowanie bez generowania wyjątków, należy sprawdzić [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) właściwości.

Za pomocą klasy ControlCachePolicy jest jednym z kilku sposobów, aby umożliwić buforowanie danych wyjściowych. Na poniższej liście opisano metody, których można użyć, aby włączyć buforowanie danych wyjściowych:

- Użyj [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) dyrektywy, aby włączyć danych wyjściowych w pamięci podręcznej w scenariuszach deklaratywne.
- Użyj [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atrybutu, aby włączyć buforowanie kontrolki użytkownika w pliku związanym z kodem.
- Użyj klasy ControlCachePolicy, aby określić ustawienia pamięci podręcznej w scenariuszach programowy, w których pracujesz z wystąpień BasePartialCachingControl, które zostały pamięci podręcznej obsługą przy użyciu jednej z powyższych metod i ładowane dynamicznie przy użyciu [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) metody.

Wystąpienie ControlCachePolicy mogą być pomyślnie zmieniane tylko między Init i PreRender etapach cyklu życia kontroli. Jeśli zmodyfikujesz obiektu ControlCachePolicy po fazie PreRender ASP.NET zgłasza wyjątek, ponieważ wszelkie zmiany wprowadzone po renderowania formantu faktycznie nie może mieć wpływ na ustawienia pamięci podręcznej (formant jest buforowana na etapie renderowania). Na koniec wystąpienie kontrolki użytkownika (i w związku z tym obiektem ControlCachePolicy) jest dostępna tylko dla programowe operowanie po faktycznie jest renderowany.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Zmiany konfiguracji pamięci podręcznej — &lt;buforowania&gt; — Element

Wprowadzono kilka zmian w konfiguracji buforowania programu ASP.NET 2.0. &lt;Buforowania&gt; element jest nowa w programie ASP.NET 2.0 i umożliwia zapewnienie buforowania zmiany konfiguracji w pliku konfiguracji. Następujące atrybuty są dostępne.

| **Element** | **Opis** |
| --- | --- |
| **cache** | Element opcjonalny. Definiuje ustawienia powiązane z aplikacji globalnej pamięci podręcznej. |
| **outputCache** | Element opcjonalny. Określa ustawienia pamięci podręcznej danych wyjściowych w całej aplikacji. |
| **outputCacheSettings** | Element opcjonalny. Określa ustawienia pamięci podręcznej danych wyjściowych, które mogą być stosowane do stron w aplikacji. |
| **sqlCacheDependency** | Element opcjonalny. Konfiguruje zależności pamięci podręcznej SQL dla aplikacji ASP.NET. |

### <a name="the-ltcachegt-element"></a>&lt;Pamięci podręcznej&gt; — Element

Następujące atrybuty są dostępne w &lt;pamięci podręcznej&gt; elementu:

| **Atrybut** | **Opis** |
| --- | --- |
| **disableMemoryCollection** | Opcjonalnie **logiczna** atrybutu. Pobiera lub ustawia wartość wskazującą, czy kolekcja pamięci podręcznej, która występuje, gdy komputer jest w dużym wykorzystaniu pamięci jest wyłączona. |
| **disableExpiration** | Opcjonalnie **logiczna** atrybutu. Pobiera lub ustawia wartość wskazującą, czy wygaśnięcia pamięci podręcznej jest wyłączona. Po wyłączeniu elementów pamięci podręcznej nie wygasają, a tło oczyszczania elementów wygasłych pamięci podręcznej nie jest wykonywane. |
| **privateBytesLimit** | Opcjonalnie **Int64** atrybutu. Pobiera lub ustawia wartość wskazująca, że elementy wygasłe maksymalny rozmiar Bajty prywatne aplikacji przed uruchomieniem pamięci podręcznej, opróżnianie i próby odzyskania pamięci. Ten limit obejmuje zarówno pamięci używanej przez pamięć podręczną, a także normalne pamięci narzut uruchomionej aplikacji. Ustawienie wartości zerowej oznacza, że ASP.NET będzie używać własnej Algorytm heurystyczny do określania, kiedy rozpoczyna odzyskiwanie pamięci. |
| **percentagePhysicalMemoryUsedLimit** | Opcjonalnie **Int32** atrybutu. Pobiera lub ustawia wartość wskazująca, że elementy wygasłe maksymalnej wartości procentowej pamięci fizycznej na komputerze, którą mogą być używane przez aplikację, przed rozpoczęciem pamięci podręcznej, opróżnianie i próby odzyskania pamięci to wykorzystania pamięci zawiera zarówno pamięci, które posługują się również w pamięci podręcznej jak użycie pamięci normalne uruchomionej aplikacji. Ustawienie wartości zerowej oznacza, że ASP.NET będzie używać własnej Algorytm heurystyczny do określania, kiedy rozpoczyna odzyskiwanie pamięci. |
| **privateBytesPollTime** | Opcjonalnie **TimeSpan** atrybutu. Pobiera lub ustawia wartość określającą interwał sondowania w celu użycia pamięci Bajty prywatne aplikacji. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; — Element

Następujące atrybuty są dostępne dla &lt;outputCache&gt; elementu.


|       <strong>Atrybut</strong>        |                                                                                                                                                                                                                                                       <strong>Opis</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Opcjonalnie <strong>logiczna</strong> atrybutu. Włącza/wyłącza pamięci podręcznej danych wyjściowych strony. Wyłączenie strony nie są buforowane niezależnie od ustawień programistycznych albo zezwala na deklaratywnego. Wartość domyślna to <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Opcjonalnie <strong>logiczna</strong> atrybutu. Włącza/wyłącza fragmentu pamięci podręcznej aplikacji. Wyłączenie strony nie są buforowane, niezależnie od wartości [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) dyrektywy lub buforowanie profil używany. Zawiera nagłówek z kontroli pamięci podręcznej wskazującą, czy serwery proxy nadrzędnego, jak również do klientów w przeglądarkach należy zrezygnować z danych wyjściowych strony w pamięci podręcznej. Wartość domyślna to <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Opcjonalnie <strong>logiczna</strong> atrybutu. Pobiera lub ustawia wartość wskazującą czy <strong>pamięci podręcznej — kontrolki: prywatne</strong> nagłówka są domyślnie wysyłane przez moduł wyjściowej pamięci podręcznej. Wartość domyślna to <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Opcjonalnie <strong>logiczna</strong> atrybutu. Włącza/wyłącza wysyłania Http "<strong>zależne: \</ strong ><em>" Nagłówek w odpowiedzi. Ustawieniem domyślnym false, "</em>* zależne: \* <strong>" Nagłówek są wysyłane do stron pamięci podręcznej danych wyjściowych. Po wysłaniu nagłówka zależne, umożliwia ona różne wersje przechowywanie w pamięci podręcznej na podstawie został określony w nagłówku zależne. Na przykład <em>zależne: użytkownik-agentów</em> będą przechowywane w różnych wersjach strony, w zależności od agenta użytkownika żądania. Wartość domyślna to ** false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; — Element

&lt;OutputCacheSettings&gt; elementu pozwala na tworzenie profilów pamięci podręcznej, jak opisano wcześniej. Element podrzędny tylko dla &lt;outputCacheSettings&gt; element jest &lt;outputCacheProfiles&gt; element do konfigurowania profilów pamięci podręcznej.

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt; — Element

Następujące atrybuty są dostępne dla &lt;sqlCacheDependency&gt; elementu.

| **Atrybut** | **Opis** |
| --- | --- |
| **Włączone** | Wymagane **logiczna** atrybutu. Wskazuje, czy zmiany są sondowane dla. |
| **pollTime** | Opcjonalnie **Int32** atrybutu. Określa częstotliwość, z którym SqlCacheDependency sonduje zmiany można znaleźć w tabeli bazy danych. Ta wartość odpowiada liczbę milisekund między kolejnymi pollings. Nie można ustawić mniej niż 500 milisekund. Wartość domyślna to 1 minuta. |

### <a name="more-information"></a>Więcej informacji

Istnieje pewne dodatkowe informacje, które należy pamiętać o dotyczące konfiguracji pamięci podręcznej.

- Jeśli nie ustawiono limit bajtów prywatnych procesu roboczego, pamięci podręcznej użyje jednego z następujących ograniczeń: 

    - x86 2 GB: 800 MB lub 60% pamięci fizycznej RAM, która kwota jest mniejsza
    - x86 3 GB: 1800 MB lub 60% pamięci fizycznej RAM, która kwota jest mniejsza
    - x 64: 1 terabajta lub 60% pamięci fizycznej RAM która kwota jest mniejsza
- Jeśli oba proces roboczy procesu prywatnych bajtów ograniczenia i &lt;privateBytesLimit w pamięci podręcznej /&gt; skonfigurowanie pamięci podręcznej będą używane co najmniej dwa.
- Podobnie jak w 1.x, możemy usunąć wpisy w pamięci podręcznej i wywołać GC. Zbierz dwóch powodów: 

    - Firma Microsoft bardzo zbliża się limit Bajty prywatne
    - Dostępna pamięć jest prawie lub mniejsze niż 10%
- Można skutecznie wyłączeniu przycinania i pamięci podręcznej dla warunków dostępnej pamięci, ustawiając &lt;percentagePhysicalMemoryUseLimit w pamięci podręcznej /&gt; do 100.
- W odróżnieniu od 1.x, w wersji 2.0 zostanie zawieszone przycinania i zbieranie wywołań, jeśli ostatni GC. Zbieranie nie zmniejszenia bajtów prywatnych ani rozmiaru sterty zarządzanej przez więcej niż 1% limitu pamięci (pamięć podręczna).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Zależności niestandardowej pamięci podręcznej

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik XML o nazwie cache.xml i zapisz go w katalogu głównym aplikacji sieci Web.
3. Dodaj następujący kod do strony\_metoda w kodem default.aspx obciążenia: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Dodaj następujący element do góry default.aspx w widoku źródła: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Przeglądaj Default.aspx. Co to czas mówią
6. Odśwież przeglądarkę. Co to czas mówią
7. Otwórz cache.xml i Dodaj następujący kod: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Zapisz cache.xml.
9. Odśwież przeglądarkę. Co to czas mówią
10. Wyjaśnić, dlaczego podczas aktualizacji zamiast wyświetlania wartości wcześniej w pamięci podręcznej:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorium 2: Używanie zależności pamięci podręcznej na podstawie sondowania

Projekt, który został utworzony w poprzednim module, który umożliwia edytowanie danych w bazie danych Northwind za pomocą kontrolki GridView i DetailsView korzysta z tego laboratorium.

1. Otwórz projekt w programie Visual Studio 2005.
2. Uruchom aspnet\_narzędzie regsql względem bazy danych Northwind, aby umożliwić bazy danych i tabeli Produkty. Użyj następującego polecenia z wiersza polecenia Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Dodaj następujący element do pliku web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Dodaj nowy formularz sieci Web o nazwie showdata.aspx.
5. Na stronie showdata.aspx, Dodaj następujące @ outputcache dyrektywy: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Dodaj następujący kod do strony\_obciążenia showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Dodawanie nowej kontrolki SqlDataSource do showdata.aspx i skonfigurować go do korzystania z połączenia z bazą danych Northwind. Kliknij przycisk Dalej.
8. Zaznacz odpowiednie pola wyboru z właściwościami ProductName i ProductID, a następnie kliknij przycisk Dalej.
9. Kliknij przycisk Zakończ.
10. Na stronie showdata.aspx, Dodaj nowe kontrolki GridView.
11. Wybierz SqlDataSource1 z listy rozwijanej.
12. Zapisz i Przeglądaj showdata.aspx. Zanotuj wyświetlany czas.
