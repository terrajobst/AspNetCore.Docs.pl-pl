---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfiguracja i Instrumentacja | Dokumentacja firmy Microsoft
author: microsoft
description: Istnieją istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET umożliwia na żądanie ściągnięcia wprowadzanie zmian w konfiguracji...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: ba116140faa0667d504e0ff101c274db9f46079e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751872"
---
<a name="configuration-and-instrumentation"></a>Konfiguracja i Instrumentacja
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Istnieją istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET umożliwia programowe wprowadzone zmiany w konfiguracji. Ponadto istnieje wiele nowych ustawień konfiguracji umożliwiają nowe konfiguracje i instrumentacji.


Istnieją istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET umożliwia programowe wprowadzone zmiany w konfiguracji. Ponadto istnieje wiele nowych ustawień konfiguracji umożliwiają nowe konfiguracje i instrumentacji.

W tym module omówimy interfejs API konfiguracji platformy ASP.NET on odnosi się do odczytywanie z oraz zapisywanie do plików konfiguracji programu ASP.NET, a firma Microsoft będzie również obejmowała Instrumentacji ASP.NET. Omówimy również nowe funkcje dostępne w śledzenie na platformie ASP.NET.

## <a name="aspnet-configuration-api"></a>Interfejs API konfiguracji platformy ASP.NET

Interfejs API konfiguracji ASP.NET umożliwia tworzenie, wdrażanie i zarządzanie danymi konfiguracyjnymi aplikacji przy użyciu pojedynczego interfejsu programowania. Interfejs API konfiguracji umożliwia rozwijać i modyfikować Dokończ konfigurację ASP.NET programowo bez konieczności bezpośredniego edytowania kodu XML w plikach konfiguracji. Ponadto można użyć konfiguracji interfejsu API w aplikacji konsoli i skrypty, które tworzysz, narzędzia do zarządzania opartego na sieci Web i przystawek programu Microsoft Management Console (MMC).

Następujące dwa narzędzia zarządzania konfiguracją korzystania z konfiguracji interfejsu API i są uwzględniane przy użyciu platformy .NET Framework w wersji 2.0:

- Przystawki MMC programu ASP.NET, która korzysta z konfiguracji interfejsu API, aby uprościć zadania administracyjne, zapewniając zintegrowany widok danych konfiguracji lokalnej na wszystkich poziomach hierarchii konfiguracji.
- Narzędzie Administratorskie witryny sieci Web, który umożliwia zarządzanie ustawieniami konfiguracji dla aplikacji lokalnych i zdalnych, w tym obsługiwanych witryn.

Interfejs API konfiguracji ASP.NET składa się z zestawu ASP.NET management Objects, które umożliwiają programowe Konfigurowanie witryny sieci Web i aplikacji. Zarządzanie obiektami są implementowane jako biblioteki klas .NET Framework. Model programowania interfejs API konfiguracji zapewnia spójność kodu i niezawodności, wymuszanie typów danych w czasie kompilacji. Aby ułatwić zarządzanie konfiguracjami aplikacji, interfejs API konfiguracji służy do wyświetlania danych, który będzie scalony ze wszystkich punktów w hierarchii konfiguracji jak pojedynczą kolekcją, zamiast wyświetlania danych jako oddzielne kolekcje za pośrednictwem różnych pliki konfiguracji. Ponadto interfejs API konfiguracji umożliwia manipulowanie konfiguracje dla całej aplikacji bez konieczności bezpośredniego edytowania kodu XML w plikach konfiguracji. Ponadto interfejs API upraszcza zadania związane z konfiguracji dzięki obsłudze narzędzi administracyjnych, takich jak narzędzia do administrowania witryną sieci Web. Interfejs API konfiguracji upraszcza wdrażanie obsługi tworzenia plików konfiguracyjnych na komputerze, a następnie uruchamiając skrypty konfiguracji na wielu komputerach.

> [!NOTE]
> Konfiguracja interfejsu API nie obsługuje tworzenia aplikacji usług IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Praca z ustawieniami konfiguracji lokalnych i zdalnych

Obiekt konfiguracji reprezentuje widok scalony ustawienia konfiguracji, które są stosowane do określonej jednostki fizyczne, taki jak komputer, lub logicznej jednostki, takie jak aplikacja lub witryna sieci Web. Określonej jednostki logicznej może istnieć na komputerze lokalnym lub na serwerze zdalnym. Gdy plik konfiguracji nie istnieje dla określonej jednostki, obiekt konfiguracji reprezentuje ustawienia konfiguracji domyślnej, zgodnie z definicją w pliku Machine.config.

Obiekt konfiguracji można uzyskać za pomocą jednej z metod otwórz konfigurację z następujące klasy:

1. Klasy ConfigurationManager, jeśli Twoja organizacja jest aplikacją kliencką.
2. Klasa WebConfigurationManager, jeśli Twoja organizacja jest aplikacją sieci Web.

Tych metod zwraca obiekt konfiguracji, który z kolei udostępnia wymaganych metod i właściwości w celu obsługi podstawowych plików konfiguracji. Możesz uzyskać dostęp do tych plików do odczytu lub zapisu.

### <a name="reading"></a>Odczytywanie

Metoda GetSection lub GetSectionGroup umożliwia odczytywanie informacji o konfiguracji. Użytkownik lub proces, który odczytuje musi mieć uprawnienia na wszystkich plików konfiguracji w hierarchii odczytu.

> [!NOTE]
> Jeśli używasz metody statycznej GetSection, który przyjmuje parametr ścieżki, parametr path musi odwoływać się do aplikacji, w którym wykonywany jest kod. W przeciwnym razie parametr jest ignorowany i informacje o obecnie uruchomionej aplikacji w konfiguracji jest zwracana.


### <a name="writing"></a>Zapisywanie

Użyj jednej z metod Zapisz można zapisać informacji o konfiguracji. Użytkownik lub proces, który musi mieć zapisów zapis w pliku konfiguracji i katalogu na bieżącym poziomie konfiguracji w hierarchii, a także uprawnienia do odczytu z wszystkich plików konfiguracji w hierarchii.

Aby wygenerować plik konfiguracji, który reprezentuje ustawienia konfiguracji dziedziczone dla określonej jednostki, użyj jednej z następujących metod konfiguracji zapisywania:

1. Metoda Zapisz do utworzenia nowego pliku konfiguracji.
2. Metoda zapisem do generowania nowego pliku konfiguracji w innej lokalizacji.

## <a name="configuration-classes-and-namespaces"></a>Konfiguracja klas i przestrzeni nazw

Wiele metod i klas konfiguracji są podobne do siebie nawzajem. W poniższej tabeli opisano konfigurację najczęściej używanych klas i przestrzenie nazw.

| **Konfiguracja klasą lub przestrzenią nazw** | **Opis** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) przestrzeni nazw | Zawiera klasy głównej konfiguracji dla wszystkich aplikacji .NET Framework. Klasy programu obsługi sekcji są używane do uzyskiwania danych konfiguracji dla sekcji z metod, takich jak GetSection i GetSectionGroup. Te dwie metody są niestatycznych. |
| Klasa System.Configuration.Configuration | Reprezentuje zestaw danych o konfiguracji dla komputera, aplikacji, katalogów sieci Web lub innego zasobu. Ta klasa zawiera przydatne metody, takie jak GetSection i GetSectionGroup, aktualizowania ustawień konfiguracji i uzyskaniu odwołania do sekcji i grupy sekcji. Ta klasa jest używana jako zwracany typ metody, które uzyskują dane konfiguracji w czasie projektowania, takie jak metody klasy WebConfigurationManager i ConfigurationManager. |
| System.Web.Configuration namespace | Zawiera klasy programu obsługi sekcji sekcjami konfiguracyjnymi ASP.NET zdefiniowany z numerem [ustawienia konfiguracji programu ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Klasy programu obsługi sekcji są używane do uzyskiwania danych konfiguracji dla sekcji z metod, takich jak GetSection i GetSectionGroup. |
| System.Web.Configuration.WebConfigurationManager class | Udostępnia przydatne metody uzyskiwania odwołania do ustawień konfiguracji w czasie wykonywania oraz w czasie projektowania. Metody te za pomocą klasy System.Configuration.Configuration jako typ zwracany. Zamiennie można użyć statycznej GetSection metody tej klasy lub niestatycznej GetSection klasy System.Configuration.ConfigurationManager. W przypadku konfiguracji aplikacji sieci Web zaleca się klasy System.Web.Configuration.WebConfigurationManager zamiast klasy System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | Umożliwia dostosowywanie i rozszerzanie dostawcę konfiguracji. To klasa podstawowa dla wszystkich klas dostawcy systemu konfiguracji. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Zawiera klasy i interfejsy do zarządzania i monitorowania kondycji aplikacji sieci Web. Ściśle rzecz ujmując ta przestrzeń nazw nie jest uważany za część konfiguracji interfejsu API. Na przykład śledzenie i zdarzenie odbywa się przez klasy w tej przestrzeni nazw. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) namespace | Udostępnia klasy, które są niezbędne do instrumentacji aplikacji, aby im eksponowanie informacji i zdarzeń za pomocą Instrumentacji zarządzania Windows (WMI) do potencjalnych klientów. Monitorowania kondycji ASP.NET używa usługi WMI w celu dostarczenia zdarzeń. Ściśle rzecz ujmując ta przestrzeń nazw nie jest uważany za część konfiguracji interfejsu API. |

## <a name="reading-from-aspnet-configuration-files"></a>Odczyt z plików konfiguracji programu ASP.NET

Klasa WebConfigurationManager jest podstawowa klasy odczyt z plików konfiguracji programu ASP.NET. Są zasadniczo trzy kroki umożliwiające odczytywanie plików konfiguracyjnych programu ASP.NET:

1. Pobierz obiekt konfiguracji, za pomocą metody OpenWebConfiguration.
2. Pobierz odwołanie do żądanego sekcji w pliku konfiguracji.
3. Przeczytaj odpowiednie informacje z pliku konfiguracji.

Konfiguracja, który reprezentuje obiekt nie reprezentuje pliku określonej konfiguracji. Zamiast tego reprezentuje widok scalony konfiguracji komputera, aplikacji lub witryny sieci Web. Poniższy przykład kodu tworzy obiekt konfiguracji reprezentujący konfigurację aplikacji sieci Web o nazwie *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Należy pamiętać, że jeśli ścieżka /ProductInfo nie istnieje, powyższy kod zwróci konfigurację domyślną, jak określono w pliku machine.config.


Po utworzeniu obiektu konfiguracji, można następnie użyć GetSection lub GetSectionGroup metody, aby przejść do ustawień konfiguracji. Poniższy przykład pobiera odwołanie do ustawienia personifikacji dla powyższych aplikacji ProductInfo:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Zapisywanie w plikach konfiguracji platformy ASP.NET

Jak Odczyt z plików konfiguracji, klasa WebConfigurationManager stanowi podstawę do zapisywania plików konfiguracyjnych programu Asp.NET. Istnieją trzy kroki, aby zapisywanie w plikach konfiguracji platformy ASP.NET.

1. Pobierz obiekt konfiguracji, za pomocą metody OpenWebConfiguration.
2. Pobierz odwołanie do żądanego sekcji w pliku konfiguracji.
3. Zapisywanie żądaną informacji z pliku konfiguracji, za pomocą zapisu lub Zapisz jako metody.

Poniższy kod zmiany **debugowania** atrybutu &lt;kompilacji&gt; element na wartość false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Gdy ten kod jest wykonywany, **debugowania** atrybutu &lt;kompilacji&gt; element zostanie ustawiony na wartość false dla *aplikacji sieci Web* pliku web.config aplikacji.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management przestrzeń nazw zawiera klasy i interfejsy do zarządzania i monitorowania kondycji aplikacji ASP.NET.

Rejestrowanie odbywa się przez definiowanie zasady, które kojarzy zdarzeń za pomocą dostawcy. Reguła określa typ zdarzenia, które są wysyłane do dostawcy. Następujące zdarzenia podstawowego są dostępne dla logowania:

| **WebBaseEvent** | Klasa podstawowa zdarzenia dla wszystkich zdarzeń. Zawiera wymagane właściwości dla wszystkich zdarzeń, takich jak kod zdarzenia, kod szczegółów zdarzenia, Data i godzina zdarzenia zostało podniesione, sekwencji numer, komunikatów o zdarzeniach i szczegóły zdarzenia. |
| --- | --- |
| **WebManagementEvent** | Klasa podstawowa zdarzeń dla zdarzeń zarządzania, takich jak okres istnienia aplikacji, żądań, błędów i zdarzeń inspekcji. |
| **WebHeartbeatEvent** | Zdarzenie wygenerowanymi przez aplikację w regularnych odstępach czasu do przechwytywania informacji o stanie przydatne w czasie wykonywania. |
| **WebAuditEvent** | Klasa bazowa dla zdarzeń inspekcji zabezpieczeń, które służą do oznaczania warunki, takie jak błąd autoryzacji, błąd odszyfrowywania *itp.* |
| **WebRequestEvent** | Klasa bazowa dla wszystkich zdarzeń informacyjny żądania. |
| **WebBaseErrorEvent** | Klasa bazowa dla wszystkich zdarzeń wskazujących warunków błędów. |

Typy dostawców dostępnych pozwalają wysyłać zdarzenie w danych wyjściowych do podglądu zdarzeń, programu SQL Server, Instrumentacji zarządzania Windows (WMI) i wiadomości e-mail. Wstępnie skonfigurowane dostawców i mapowania zdarzeń zmniejszyć ilość pracy, które są niezbędne, aby uzyskać dane wyjściowe zdarzenia rejestrowane.

ASP.NET 2.0 używa dziennika zdarzeń dostawca out-of--box mają być rejestrowane zdarzenia na podstawie domen aplikacji, uruchamianie i zatrzymywanie, a także rejestrowanie nieobsłużone wyjątki. Pomaga to obejmują niektóre z podstawowych scenariuszy. Załóżmy na przykład, że aplikacji zgłasza wyjątek, ale użytkownik nie zostanie zapisany błąd i nie można odtworzyć go. Za pomocą domyślnej reguły dziennika zdarzeń będzie mogła gromadzić informacje wyjątek i stos, aby lepiej zrozumieć, jakiego rodzaju błąd wystąpił. Inny przykład w przypadku aplikacji jest utraty stanu sesji. W takim przypadku możesz przejrzeć w dzienniku zdarzeń, aby określić, czy jest odtwarzane domeny aplikacji i dlaczego domeny aplikacji jest zatrzymana na pierwszym miejscu.

Ponadto kondycji systemu monitorowania jest rozszerzalny. Na przykład możesz zdefiniowania niestandardowych zdarzeń w sieci Web, uruchomić je w ramach aplikacji a następnie zdefiniować regułę do wysyłania informacji o zdarzeniach do dostawcy, takich jak wiadomości e-mail. Dzięki temu można łatwo powiązać Twoje Instrumentacji do monitorowania dostawców kondycji. Inny przykład można wyzwolić zdarzenie każdorazowo zamówienie jest przetwarzany i skonfigurować regułę, która wysyła każde zdarzenie w bazie danych programu SQL Server. Można również wyzwalać zdarzenie, gdy użytkownik zakończy się niepowodzeniem, zaloguj się na wiele razy z rzędu i skonfigurować zdarzenia do używania dostawców bazujące na poczcie e-mail.

Konfiguracja domyślnych dostawców i zdarzenia są przechowywane w pliku Web.config globalnego. Globalne pliku Web.config wszystkie ustawienia są przechowywane opartego na sieci Web, które były przechowywane w pliku Machine.config na platformie ASP.NET 1 x. Globalny plik Web.config znajduje się w następującym katalogu:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt; sekcji globalnej pliku Web.config zawiera domyślne ustawienia konfiguracji. Możesz przesłonić te ustawienia lub skonfigurować własne ustawienia, implementując &lt;healthMonitoring&gt; sekcji w pliku Web.config aplikacji.

&lt;HealthMonitoring&gt; sekcji globalnej pliku Web.config zawiera następujące elementy:

| **dostawcy** | Zawiera dostawców dla podglądu zdarzeń, usługi WMI i SQL Server. |
| --- | --- |
| **eventMappings** | Zawiera mapowania dla różnych klas WebBase. Można rozszerzyć tę listę, jeśli Generowanie klasy zdarzeń. Generowanie klasy zdarzeń zapewnia bardziej szczegółowy za pośrednictwem dostawców, którą możesz wysłać informacje, aby. Na przykład można skonfigurować nieobsłużone wyjątki, które zostanie wysłane do programu SQL Server podczas wysyłania zdarzeń niestandardowych do poczty e-mail. |
| **reguły** | Linki eventMappings dostawcy. |
| **buforowanie** | Używane z dostawcami zasobów programu SQL Server i wiadomości e-mail do określenia, jak często mają być opróżnienia zdarzeń dla dostawcy. |

Poniżej przedstawiono przykładowy kod z globalnego pliku Web.config.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Jak przechowywać zdarzenia w Podglądzie zdarzeń

Jak wspomniano wcześniej, dostawca dla rejestrowania zdarzeń w zdarzeniu podglądu jest skonfigurowane w globalnego pliku Web.config. Domyślnie, na podstawie wszystkich zdarzeń **WebBaseErrorEvent** i **WebFailureAuditEvent** są rejestrowane. Możesz dodać dodatkowe reguły, aby rejestrować dodatkowe informacje w dzienniku zdarzeń. Na przykład, jeśli chce się rejestrowanie wszystkich zdarzeń (*tj*, na podstawie każdego zdarzenia **WebBaseEvent**), można dodać następującą regułę do pliku Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Ta zasada będzie link **wszystkie zdarzenia** mapę zdarzeń, aby dostawca dziennika zdarzeń. Zarówno eventMapping, jak i dostawcy znajdują się w pliku Web.config globalnego.

## <a name="how-to-store-events-to-sql-server"></a>Jak przechowywać zdarzenia do programu SQL Server

Ta metoda używa **ASPNETDB** bazy danych, która jest generowana przez Aspnet\_regsql.exe narzędzia. Domyślny dostawca używa LocalSqlServer ciąg połączenia, który używa albo na podstawie pliku bazy danych w aplikacji\_folderu danych lub SQLExpress lokalne wystąpienie programu SQL Server. Parametry połączenia LocalSqlServer i SqlProvider są konfigurowane w pliku Web.config globalnego.

LocalSqlServer parametry połączenia w pliku Web.config globalnego wygląda następująco:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Jeśli chcesz użyć innego wystąpienia programu SQL Server, musisz użyć Aspnet\_regsql.exe narzędzia, które znajdują się w % windir%\Microsoft.Net\Framework\v2.0.\* folderu. Użyj Aspnet\_regsql.exe narzędzie do generowania niestandardowego **ASPNETDB** bazy danych w wystąpieniu programu SQL Server, a następnie dodaj parametry połączenia do pliku konfiguracyjnego aplikacji, a następnie dodać dostawcę przy użyciu nowego Parametry połączenia. Po utworzeniu **ASPNETDB** bazy danych utworzonej, należy ustawić zasadę, aby połączyć eventMapping sqlProvider.

Użyj domyślnej SqlProvider lub konfigurowanie własnego dostawcę, należy dodać regułę łączenia dostawcy z mapą zdarzeń. Następująca reguła łączy nowego dostawcę, który został utworzony powyżej do **wszystkie zdarzenia** Mapa zdarzeń. Ta zasada będzie rejestrować wszystkie zdarzenia na podstawie **WebBaseEvent** i wysyłać je do MySqlWebEventProvider, który będzie używany ciąg połączenia MYASPNETDB. Poniższy kod dodaje regułę do łączenia dostawcy z mapą zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Jeśli chcesz tylko błędy wysyłania do programu SQL Server, można dodać następującą regułę:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Jak przekazywać zdarzenia do Instrumentacji zarządzania Windows

Możesz również przekazywać zdarzenia do usługi WMI. Dostawca usługi WMI jest skonfigurowane w pliku Web.config globalnego domyślnie.

Poniższy przykład kodu Dodaje regułę do przesyłania dalej zdarzeń do usługi WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Musisz dodać zasadę, aby skojarzyć eventMapping dostawcę i aplikacji odbiornika usługi WMI, aby nasłuchiwać zdarzeń. Poniższy przykład kodu Dodaje regułę do łączenia dostawcy WMI do **wszystkie zdarzenia** Mapa zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Jak przekazywać zdarzenia do wiadomości e-mail

Możesz również przekazywać zdarzenia do poczty e-mail. Należy zachować ostrożność reguły zdarzenia, które należy mapować dostawcy poczty e-mail, ponieważ użytkownik może przypadkowo wysłać do siebie mnóstwo informacji, może być lepiej dopasowane do programu SQL Server i dzienniku zdarzeń. Istnieją dwa dostawców poczty e-mail; SimpleMailWebEventProvider i TemplatedMailWebEventProvider. Każda ma takie same atrybuty konfiguracji, z wyjątkiem "szablon" i "detailedTemplateErrors" atrybuty, które są dostępne tylko na TemplatedMailWebEventProvider.

> [!NOTE]
> Żadna z tych dostawców poczty e-mail nie jest skonfigurowane. Należy dodać je do pliku Web.config.


Główna różnica między tych dostawców poczty e-mail dwóch jest SimpleMailWebEventProvider i wysyła wiadomości e-mail w szablonie ogólny, który nie może być modyfikowany. Przykładowy plik Web.config dodaje tego dostawcy wiadomość e-mail do listy dostawców skonfigurowane za pomocą następujących reguł:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Następująca reguła jest także dodawane do wybranego dostawcy poczty e-mail, aby powiązać **wszystkie zdarzenia** Mapa zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Program ASP.NET 2.0 śledzenia

Istnieją trzy ważne udoskonalenia śledzenie w programie ASP.NET 2.0.

1. Śledzenie zintegrowane funkcje
2. Dostęp programowy do komunikatów śledzenia
3. Ulepszone śledzenie na poziomie aplikacji

## <a name="integrated-tracing-functionality"></a>Zintegrowane funkcje śledzenia

Możesz teraz kierowanie komunikatów w postaci wyemitowane przez system.Diagnostics.trace — klasa platformy ASP.NET, dane wyjściowe śledzenia i kierowanie komunikatów w postaci wyemitowane przez śledzenie na platformie ASP.NET interfejsu System.Diagnostics.trace. Możesz również przekazywać zdarzeń Instrumentacji ASP.NET interfejsu System.Diagnostics.trace. Ta funkcjonalność jest dostarczana przez nowy **writeToDiagnosticsTrace** atrybutu &lt;śledzenia&gt; elementu. To wartość logiczna ma wartość true, komunikaty śledzenia ASP.NET są przekazywane do infrastruktury śledzenia System.Diagnostics do użytku przez żadnych odbiorników, które są zarejestrowane do wyświetlania komunikatów śledzenia.

## <a name="programmatic-access-to-trace-messages"></a>Dostęp programowy do komunikaty śledzenia

Umożliwia dostęp programowy do wszystkich komunikatów śledzenia za pomocą platformy ASP.NET 2.0 **TraceContextRecord** klasy i **///TraceRecords** kolekcji. Najbardziej wydajny sposób uzyskiwania dostępu do komunikatów śledzenia jest zarejestrować **TraceContextEventHandler** delegata (inne nowości w programie ASP.NET 2.0) do obsługi nowej **TraceFinished** zdarzeń. Następnie można pętli komunikatów śledzenia, jak chcesz.

Poniższy przykład kodu ilustruje to:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

W powyższym przykładzie czy w pętli poprzez kolekcji ///TraceRecords, a następnie wpisz każdy komunikat w strumieniu odpowiedzi.

## <a name="improved-application-level-tracing"></a>Ulepszone śledzenie na poziomie aplikacji

Śledzenie na poziomie aplikacji została udoskonalona poprzez wprowadzenie nowego **mostRecent** atrybutu &lt;śledzenia&gt; elementu. Ten atrybut określa, czy najnowsze dane wyjściowe śledzenia na poziomie aplikacji są wyświetlane i starsze dane poza granicami, które są wskazane przez requestLimit zostaną odrzucone. W przypadku wartości FAŁSZ, aż do osiągnięcia atrybut requestLimit dane śledzenia są wyświetlane dla żądań.

## <a name="aspnet-command-line-tools"></a>Narzędzia wiersza polecenia platformy ASP.NET

Istnieje kilka narzędzi wiersza polecenia, aby ułatwić konfigurację programu ASP.NET. Deweloperów platformy ASP.NET, należy zapoznać się z aspnet\_regiis.exe narzędzia. ASP.NET 2.0 zawiera trzy inne narzędzia wiersza polecenia do pomocy w konfiguracji.

Dostępne są następujące narzędzia wiersza polecenia:

| **Narzędzie** | **Użyj** |
| --- | --- |
| **aspnet\_regiis.exe** | Umożliwia rejestracji programu ASP.NET w usługach IIS. Istnieją dwie wersje tego narzędzia dostarczonymi za pomocą programu ASP.NET 2.0, jeden dla systemów 32-bitowych (w folderze Framework) i jeden dla 64-bitowych systemach (w folderze Framework64.) Nie można zainstalowana wersja 64-bitowego na 32-bitowych systemach operacyjnych. |
| **aspnet\_regsql.exe** | Narzędzie rejestracji serwera SQL programu ASP.NET umożliwia utworzyć bazę danych programu Microsoft SQL Server do użytku przez dostawców programu SQL Server w programie ASP.NET lub dodawanie lub usuwanie opcji z istniejącej bazy danych. Aspnet\_regsql.exe plik znajduje się w [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber folder na serwerze sieci Web. |
| **aspnet\_regbrowsers.exe** | Narzędzie rejestracji programu ASP.NET w przeglądarce analizuje wszystkie definicje przeglądarki systemowe w zespół kompiluje i instaluje zestaw w globalnej pamięci podręcznej. To narzędzie używa plików definicji przeglądarki (. Pliki PRZEGLĄDARKI) z podkatalogu .NET Framework przeglądarki. Narzędzie można znaleźć w katalogu %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Narzędzia kompilacji platformy ASP.NET można skompilować aplikację sieci Web ASP.NET w miejscu lub we wdrożeniach w lokalizacji docelowej, takich jak na serwerze produkcyjnym. Kompilacja w miejscu pomaga wydajność aplikacji, ponieważ użytkownicy końcowi czy nie występują opóźnienia na pierwsze żądanie do aplikacji, podczas kompilowania aplikacji. |

Ponieważ aspnet\_narzędzie regiis.exe nie jesteś nowym użytkownikiem programu ASP.NET 2.0, nie omówimy je tutaj.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Narzędzie rejestracji serwera SQL platformy ASP.NET — aspnet\_regsql.exe

Można ustawić kilka typów opcji za pomocą narzędzia rejestracji serwera SQL programu ASP.NET. Można określić połączenia SQL, określ, które usług aplikacji ASP.NET używania programu SQL Server do zarządzania informacji, wskazać, które bazy danych lub tabela jest używana do zależności pamięci podręcznej SQL i dodać lub usunąć obsługę przechowywanie procedur i stanu sesji przy użyciu programu SQL Server.

Kilku usług aplikacji ASP.NET, zależy od dostawcy, zarządzanie, przechowywanie i pobieranie danych ze źródła danych. Każdy dostawca jest specyficzny dla źródła danych. Program ASP.NET zawiera dostawca programu SQL Server w taki sposób, aby uzyskać poniższe funkcje platformy ASP.NET:

- Członkostwa ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) klasy).
- Zarządzanie rolami ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) klasy).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) klasy).
- Web Part personalizacji ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) klasy).
- Zdarzenia w sieci Web ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) klasy).

Po zainstalowaniu programu ASP.NET, pliku Machine.config serwera zawiera elementy konfiguracji, które Określ dostawców programu SQL Server dla każdej funkcji programu ASP.NET, które zależą od dostawcy. Ci dostawcy są skonfigurowane domyślnie do połączenia z wystąpieniem użytkownika lokalnego programu SQL Server Express 2005. Jeśli zmienisz domyślny ciąg połączenia używany przez dostawców, przed użyciem dowolnej funkcji programu ASP.NET w konfiguracji maszyny należy zainstalować bazy danych programu SQL Server i elementy bazy danych z wybranej funkcji przy użyciu Aspnet\_regsql.exe. Jeśli bazy danych, określ za pomocą narzędzia rejestrowania SQL jeszcze nie istnieje (aspnetdb będzie domyślna baza danych, jeśli nie został określony w wierszu polecenia), a następnie bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w programie SQL Server oraz aby utworzyć schemat e lements w bazie danych.

### <a name="sql-cache-dependency"></a>Zależności pamięci podręcznej SQL

Zaawansowane funkcji buforowania danych wyjściowych ASP.NET jest zależności pamięci podręcznej SQL. Zależności pamięci podręcznej SQL obsługuje dwa różne tryby działania: jedną, która używa implementację ASP.NET sondowania tabeli i drugi tryb, który korzysta z funkcji powiadamiania zapytania programu SQL Server 2005. Narzędzie do rejestracji programu SQL, można skonfigurować tryb sondowania tabeli działania.

### <a name="session-state"></a>Stan sesji

Domyślnie wartości stanu sesji i informacje są przechowywane w pamięci w ramach procesu ASP.NET. Alternatywnie można przechowywać dane sesji w bazie danych programu SQL Server, gdzie mogą być współużytkowane przez wiele serwerów sieci Web. Jeśli bazy danych, należy określić dla stanu sesji za pomocą narzędzia rejestrowania SQL jeszcze nie istnieje, bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w programie SQL Server oraz do tworzenia elementów schematu w bazie danych. Jeśli baza danych istnieje, bieżący użytkownik musi mieć uprawnienia do tworzenia elementów schematu w istniejącej bazie danych.

Aby zainstalować bazę danych stanu sesji w programie SQL Server, należy uruchomić Aspnet\_narzędzie regsql.exe i podaj następujące informacje przy użyciu polecenia:

- Nazwa serwera SQL wystąpienia, za pomocą **-S:** opcji.
- Poświadczenia logowania dla konta które ma uprawnienia do tworzenia bazy danych na komputerze z uruchomionym programem SQL Server. Użyj **-E** opcję, aby użyć aktualnie zalogowanego użytkownika lub **- U** opcję, aby określić identyfikator użytkownika wraz z **-P** opcję, aby określić hasło.
- **- Ssadd** opcji wiersza polecenia, aby dodać bazę danych stanu sesji.

Domyślnie nie można użyć Aspnet\_regsql.exe narzędzie, aby zainstalować bazę danych stanu sesji na komputerze z systemem SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Narzędzie do rejestracji programu ASP.NET przeglądarki - aspnet\_regbrowsers.exe

W programie ASP.NET w wersji 1.1 plik Machine.config zawiera sekcję o nazwie &lt;browserCaps&gt;. W tej sekcji zawiera serię wpisów XML, zdefiniowane konfiguracje dla różnych przeglądarkach, w oparciu o wyrażenia regularnego. Dla platformy ASP.NET w wersji 2.0 nowy. Plik PRZEGLĄDARKI definiuje parametry konkretnej przeglądarce, za pomocą wpisów XML. Dodajesz, dodając nowe informacje w nowym oknie przeglądarki. Plik PRZEGLĄDARKI do folderu zlokalizowanego w %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers w systemie.

Ponieważ aplikacja nie odczytuje plik .config za każdym razem, gdy wymaga informacji przeglądarki, możesz utworzyć nową. Plik PRZEGLĄDARKI i wykonywania Aspnet\_regbrowsers.exe Aby dodać wymagane zmiany do zestawu. Dzięki temu serwerowi natychmiast uzyskać dostęp nowe informacje o przeglądarce, dzięki czemu nie trzeba zamknąć dowolnej aplikacji w taki sposób, aby wczytać dane. Aplikacji można uzyskać dostęp do możliwości przeglądarki za pośrednictwem przeglądarki właściwości bieżącego HttpRequest.

Dostępne są następujące opcje, podczas uruchamiania aspnet\_regbrowser.exe:

| **Option** | **Opis** |
| --- | --- |
| **-?** | Wyświetla Aspnet\_regbbrowsers.exe tekst pomocy w oknie wiersza polecenia. |
| **-i** | Tworzy zestaw funkcji środowiska uruchomieniowego przeglądarki i instaluje je w globalnej pamięci podręcznej. |
| **-u** | Odinstalowuje zestaw funkcji środowiska uruchomieniowego przeglądarki z globalnej pamięci podręcznej. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Narzędzia kompilacji platformy ASP.NET — aspnet\_compiler.exe

Narzędzie kompilacji platformy ASP.NET może być używane na dwa sposoby ogólne: kompilacja w miejscu i kompilacji dla wdrożenia, gdy jest określony katalog wyjściowy docelowego.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilowanie aplikacji w miejscu](https://msdn.microsoft.com/library/ms229863.aspx)

Narzędzia kompilacji platformy ASP.NET można kompilować aplikacje w miejscu, czyli naśladuje zachowanie wysyłania wielu żądań do aplikacji, powodując regularnych kompilacji. Użytkownicy wstępnie skompilowanej witryny nie doświadczy opóźnienia spowodowane przez kompilowanie strony na pierwsze żądanie.

W przypadku możesz prekompilowanie witryny w miejscu, zastosuj następujące elementy:

- Witryna zachowuje swoje pliki i strukturę katalogów.
- Konieczne jest posiadanie kompilatorów dla wszystkich języków programowania, używany przez witrynę na serwerze.
- W przypadku niepowodzenia kompilacji każdego pliku w całej lokacji nie powiedzie się kompilacji.

Można także ponownie skompilować aplikację w miejscu po dodaniu nowych plików źródłowych do niego. Narzędzie kompiluje tylko nowych lub zmienionych plików, chyba że dodasz **- c** opcji.

> [!NOTE]
> Kompilacją aplikacji, który zawiera zagnieżdżony aplikacji nie można skompilować aplikacji zagnieżdżonych. Zagnieżdżone aplikacji muszą być skompilowane oddzielnie.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilowanie aplikacji dla wdrożenia](https://msdn.microsoft.com/library/ms229863.aspx)

Określenie parametru targetDir kompilowania aplikacji dla wdrożenia (kompilacja do lokalizacji docelowej). TargetDir może być lokalizacji końcowej dla aplikacji sieci Web lub skompilowaną aplikację można wdrożyć więcej. Za pomocą **-u** opcji kompilacją aplikacji w taki sposób, że możesz wprowadzić zmiany do niektórych plików w skompilowanej aplikacji bez konieczności ponownego kompilowania. ASPNET\_compiler.exe rozróżnia między typami plików statycznych i dynamicznych i obsługuje je inaczej, tworząc wynikłej aplikacji.

- Typy plików statycznych są te, które nie mają skojarzonego kompilatora lub Dostawca, takie jak pliki źródłowe kompilacji o nazwie mają rozszerzenia, takie jak CSS, GIF, htm, HTML, jpg, js, i tak dalej. Te pliki po prostu są kopiowane do lokalizacji docelowej, ich względne miejsc w strukturze katalogów zachowane.
- Typy plików dynamicznych są te, które mają skojarzone kompilatora lub utworzyć dostawcę, w tym pliki z rozszerzeniami nazwy pliku specyficzne dla platformy ASP.NET, takich jak .asax, ascx, .ashx, .aspx, .browser, .master i tak dalej. Narzędzia kompilacji platformy ASP.NET generuje zestawów z tych plików. Jeśli **-u** opcja zostanie pominięty, narzędzie utworzy również pliki z rozszerzeniem nazwy pliku. COMPILED mapujące oryginalnych plików źródłowych do ich zestawu. Aby upewnić się, że struktura katalogów źródła aplikacji jest zachowywany, narzędzie wygeneruje plików zastępczych w odpowiednich miejscach w aplikacji docelowej.

Należy użyć **-u** opcję, aby wskazać, czy mogą modyfikować zawartości skompilowanej aplikacji. W przeciwnym razie kolejne modyfikacje są ignorowane lub powodują błędy czasu wykonywania.

W poniższej tabeli opisano, jak inny plik obsługuje narzędzia kompilacji platformy ASP.NET typów, kiedy **-u** opcji jest dołączony.

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .ascx, .aspx, .master | Te pliki są podzielone na znaczników i kod źródłowy, który zawiera pliki związane z kodem i wszelki kod, który jest ujęty w &lt;skryptu runat = "server"&gt; elementów. Kod źródłowy jest kompilowany do zestawów z nazwami, które są uzyskiwane z algorytmem wyznaczania wartości skrótu i zestawy są umieszczane w katalogu Bin. Każdy kod wbudowanego, ujęte w kodzie **&lt; %** i **% &gt;** nawiasy kwadratowe, jest dołączana do znaczników i nie są kompilowane. Nowe pliki z taką samą nazwę jak pliki źródłowe są tworzone zawierać znaczniki i umieszczane w odpowiednich katalogów danych wyjściowych. |
| .ashx, .asmx | Pliki te nie są kompilowane i zostaną przeniesione do katalogów danych wyjściowych i nie są kompilowane. Jeśli chcesz mieć kod procedury obsługi skompilowane, umieść kod w plikach kodu źródłowego w aplikacji\_katalog kodu. |
| .CS, .vb, .jsl .cpp (nie w tym plików z kodem dla typów plików wymienionych wcześniej) | Te pliki są kompilowane i dołączony jako zasób w zestawy odwołujące się do nich. Pliki źródłowe nie są kopiowane do katalogu wyjściowego. Jeśli nie odwołuje się do pliku z kodem, go nie jest kompilowana. |
| Niestandardowe typy plików | Te pliki nie zostały skompilowane. Te pliki są kopiowane do odpowiednich katalogów danych wyjściowych. |
| Źródło plików kodu w aplikacji\_podkatalogu kodu | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. |
| pliki resx i .resource w aplikacji\_GlobalResources podkatalogu | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. Brak aplikacji\_podkatalogu GlobalResources jest tworzony w katalogu głównym wyjściowego i żadne pliki resx lub Resources, znajduje się w katalogu źródłowym są kopiowane do katalogów danych wyjściowych. |
| pliki resx i .resource w aplikacji\_LocalResources podkatalogu | Pliki te nie są kompilowane i są kopiowane do odpowiednich katalogów danych wyjściowych. |
| pliki .skin w aplikacji\_podkatalogu motywów | Pliki .skin i pliki statyczne motywu nie są kompilowane i są kopiowane do odpowiednich katalogów danych wyjściowych. |
| .Browser pliku Web.config statyczne typy zestawów już istnieje w katalogu Bin | Te pliki są kopiowane jest katalogów danych wyjściowych. |

W poniższej tabeli opisano, jak inny plik obsługuje narzędzia kompilacji platformy ASP.NET typów, kiedy **-u** opcja zostanie pominięta.

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Te pliki są podzielone na znaczników i kod źródłowy, który zawiera pliki związane z kodem i wszelki kod, który jest ujęty w &lt;skryptu runat = "server"&gt; elementów. Kod źródłowy jest kompilowana do zestawów z nazwami, które są uzyskiwane z algorytmu wyznaczania wartości skrótu. Wynikowe zestawy są umieszczane w katalogu Bin. Każdy kod wbudowanego, ujęte w kodzie **&lt; %** i **% &gt;** nawiasy kwadratowe, jest dołączana do znaczników i nie są kompilowane. Kompilator tworzy nowe pliki, które ma zawierać znaczniki z taką samą nazwę jak pliki źródłowe. Te pliki wynikowe są umieszczane w katalogu Bin. Kompilator tworzy również pliki z taką samą nazwę jak pliki źródłowe, ale z rozszerzeniem. COMPILED, który zawiera informacje dotyczące mapowania. . SKOMPILOWANE pliki są umieszczane w katalogów wyjściowych, odpowiadające do oryginalnej lokalizacji plików źródłowych. |
| .ascx | Te pliki są dzielone na znaczników i kod źródłowy. Kod źródłowy jest skompilowane do zestawów i umieszczane w katalogu Bin z nazwami, które są uzyskiwane z algorytmu wyznaczania wartości skrótu. Żadne pliki znaczników są generowane. |
| .CS, .vb, .jsl .cpp (nie w tym plików z kodem dla typów plików wymienionych wcześniej) | Kod źródłowy, który odwołuje się do zestawów wygenerowany na podstawie ascx, ashx lub pliki aspx jest skompilowane do zestawów i umieszczane w katalogu Bin. Nie pliki źródłowe są kopiowane. |
| Niestandardowe typy plików | Te pliki są kompilowane, takich jak dynamiczne pliki. W zależności od typu pliku, w których są one oparte na kompilator można umieścić pliki mapowania w katalogów danych wyjściowych. |
| Pliki w aplikacji\_podkatalogu kodu | Pliki kodu źródłowego, w tym podkatalogu kompilowania do zestawów i umieszczane w katalogu Bin. |
| Pliki w aplikacji\_GlobalResources podkatalogu | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. Brak aplikacji\_podkatalogu GlobalResources jest tworzony w katalogu głównym wyjściowego. Jeśli plik konfiguracji nie określa appliesTo = "All", pliki resx i Resources są kopiowane do katalogów danych wyjściowych. Nie są kopiowane, jeśli są one określone przez [elementu BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| pliki resx i .resource w aplikacji\_LocalResources podkatalogu | Te pliki są kompilowane do zestawów, używając unikatowej nazwy i umieszczane w katalogu Bin. Żadne pliki resx lub .resource są kopiowane do katalogów danych wyjściowych. |
| pliki .skin w aplikacji\_podkatalogu motywów | Motywy kompilowania do zestawów i umieszczane w katalogu Bin. Pliki szczątkowe są tworzone dla plików .skin i umieszczane w odpowiedniej katalogu wyjściowego. Pliki statyczne (na przykład .css) są kopiowane do katalogów danych wyjściowych. |
| .Browser pliku Web.config statyczne typy zestawów już istnieje w katalogu Bin | Te pliki są kopiowane, ponieważ jest do katalogu wyjściowego. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nazwy zestawów stałej](https://msdn.microsoft.com/library/ms229863.aspx##)

Niektóre scenariusze, takie jak wdrażanie aplikacji sieci Web, za pomocą pliku MSI Instalatora Windows, korzystają z nazw plików spójne i zawartość, a także struktur katalogów spójne do identyfikowania zestawów lub ustawienia konfiguracji dla aktualizacji. W takich przypadkach można użyć **- fixednames** opcję, aby określić, czy zestaw powinien kompilować się narzędzia kompilacji platformy ASP.NET dla każdego pliku źródłowego zamiast where wielu stronach są kompilowane do zestawów. Może to prowadzić do dużej liczby zestawów, więc chcąc ze skalowalnością tej opcji należy używać ostrożnie.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Strong-Name Compilation](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysign**, **- keycontainer** i **- keyfile** opcje znajdują się tak, aby można było używać Aspnet\_ Compiler.exe do tworzenia silnie nazwanych zestawów bez użycia [narzędzie silnych nazw (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) oddzielnie. Te opcje odpowiadają odpowiednio do **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**i  **AssemblyKeyFileAttribute**.

Omówienie tych atrybutów jest poza zakresem tego kursu.

## <a name="labs"></a>Warsztaty

Każdego z następujących labs opiera się na poprzednim labs. Należy wykonywać je w kolejności.

## <a name="lab-1-using-the-configuration-api"></a>Laboratorium 1: Przy użyciu konfiguracji interfejsu API

1. Utwórz nową witrynę sieci Web o nazwie *mod9lab*.
2. Dodaj nowy plik konfiguracji sieci Web do witryny.
3. Dodaj następujący element do pliku web.config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Pozwoli to zagwarantować, że masz uprawnienia, aby zapisać zmiany w pliku web.config.

1. Dodaj nowego formantu etykiety do Default.aspx i zmień identyfikator, który ma **lblDebugStatus**.
2. Dodaj nową kontrolkę przycisku do Default.aspx.
3. Identyfikator kontrolki przycisku, aby zmienić **btnToggleDebug** i tekst, który ma **Przełącz stan debugowania**.
4. Otwórz widok kodu dla pliku związanym z kodem Default.aspx i Dodaj **przy użyciu** poufności informacji dotyczące **System.Web.Configuration** w następujący sposób:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Dodaj dwie zmienne prywatne do klasy i stronę\_metody Init, jak pokazano poniżej:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Dodaj następujący kod do strony\_obciążenia:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Zapisz i Przeglądaj default.aspx. Należy zauważyć, że formant etykiety wyświetla bieżący stan debugowania.
2. Kliknij dwukrotnie formant przycisku w Projektancie i Dodaj następujący kod do Zdarzenie kliknięcia dla przycisku kontroli:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Zapisz i Przeglądaj default.aspx, a następnie kliknij przycisk.
2. Otwórz plik web.config po każdy przycisk kliknij i obserwować **debugowania** atrybutu w &lt;kompilacji&gt; sekcji.

## <a name="lab-2-logging-application-restarts"></a>Laboratorium 2: Rejestrowanie aplikacja uruchamia się ponownie

W tym laboratorium użytkownik utworzy kod, który umożliwia przełączanie rejestrowanie zamykania aplikacji, startupów i ponowne kompilacje w Podglądzie zdarzeń.

1. Dodawanie kontrolki DropDownList na default.aspx i zmień identyfikator ddlLogAppEvents.
2. Ustaw **AutoPostBack** właściwość DropDownList do **true**.
3. Dodaj trzy elementy do kolekcji elementów dla metody DropDownList. Wprowadź **tekstu** pierwszego elementu *Select Value* i wartość -1. Wprowadzić **tekstu** i **wartość** drugiego elementu **True** i **tekstu** i **wartość** trzeciego elementu **False**.
4. Dodaj nową etykietę do default.aspx. Zmień identyfikator, który ma **lblLogAppEvents**.
5. Otwórz widok związanym z kodem default.aspx, a następnie dodaj nowe oświadczenie dla zmiennej typu HealthMonitoringSection, jak pokazano poniżej:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Dodaj następujący kod do istniejącego kodu na stronie\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Kliknij dwukrotnie metody DropDownList i Dodaj następujący kod do zdarzenie selectedindexchanged.:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Przeglądaj default.aspx.
2. Ustaw listę rozwijaną **False**.
3. Wyczyść dziennik aplikacji w Podglądzie zdarzeń.
4. Kliknij przycisk, aby zmienić atrybut debugowania dla aplikacji.
5. Odśwież dziennik aplikacji w Podglądzie zdarzeń. 

    1. Wszystkie zdarzenia były rejestrowane?
    2. Dlaczego lub dlaczego nie?
6. Ustaw listę rozwijaną **wartość True.**
7. Kliknij przycisk, aby przełączyć atrybut debugowania dla aplikacji.
8. Odśwież logowania aplikacji Podgląd zdarzeń. 

    1. Wszystkie zdarzenia były rejestrowane?
    2. Jaki był przyczyny zamknięcia aplikacji?
9. Eksperymentować, włączanie i wyłączanie rejestrowania i spójrz na zmiany wprowadzone w pliku web.config.

## <a name="more-information"></a>Więcej informacji:

Program ASP.NET w wersji 2.0 Provider model pozwala na tworzenie własnych dostawców nie tylko instrumentacji aplikacji, ale wiele innych zastosowań członkostwa, profile, np. Aby uzyskać szczegółowe informacje na temat pisania niestandardowego dostawcy do dziennika zdarzeń aplikacji do pliku tekstowego, odwiedź stronę [ten link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
