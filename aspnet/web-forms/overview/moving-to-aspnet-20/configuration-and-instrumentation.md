---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfiguracja i Instrumentacji | Dokumentacja firmy Microsoft
author: microsoft
description: "Są istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET pozwala na pr wprowadzanie zmian w konfiguracji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="configuration-and-instrumentation"></a>Konfiguracja i Instrumentacji
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Są istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET pozwala na programowo wprowadzanie zmian w konfiguracji. Ponadto istnieje wiele nowe ustawienia konfiguracji umożliwiają nowe konfiguracje i instrumentacji.


Są istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET pozwala na programowo wprowadzanie zmian w konfiguracji. Ponadto istnieje wiele nowe ustawienia konfiguracji umożliwiają nowe konfiguracje i instrumentacji.

W tym module omówimy ASP.NET interfejs API konfiguracji dotyczy odczytywanie z oraz zapisywanie do plików konfiguracji ASP.NET, a także omówimy Instrumentacji ASP.NET. Omówimy również nowe funkcje dostępne w śledzenie na platformie ASP.NET.

## <a name="aspnet-configuration-api"></a>Interfejs API konfiguracji ASP.NET

Interfejs API konfiguracji ASP.NET pozwala na tworzenie, wdrażanie i zarządzanie danymi konfiguracji aplikacji za pomocą jednego interfejsu programowania. Interfejs API konfiguracji umożliwia rozwijać i modyfikować Dokończ konfigurację programu ASP.NET programowo bez konieczności bezpośredniego edytowania pliku XML w plikach konfiguracji. Ponadto można użyć konfiguracji interfejsu API w aplikacji konsoli i skrypty, które tworzysz, narzędzia do zarządzania opartych na sieci Web i przystawek programu Microsoft Management Console (MMC).

Następujące dwa narzędzia zarządzania konfiguracją korzystania z konfiguracji interfejsu API i dostępnych w .NET Framework w wersji 2.0:

- Przystawkę MMC platformy ASP.NET, która korzysta z konfiguracji interfejsu API, aby uprościć zadania administracyjne, zapewniając zintegrowany widok danych konfiguracji lokalnej na wszystkich poziomach hierarchii konfiguracji.
- Narzędzia administrowania witryną sieci Web, który umożliwia zarządzanie ustawieniami konfiguracji dla aplikacji lokalnych i zdalnych, w tym obsługiwanych witryn.

Interfejs API konfiguracji ASP.NET zawiera zestaw obiektów zarządzania platformy ASP.NET, które umożliwiają programowe konfigurowanie witryn sieci Web i aplikacji. Obiekty zarządzania są zaimplementowane jako biblioteka klas programu .NET Framework. Model programowania interfejs API konfiguracji zapewnia spójność kodu i niezawodność wymuszając typów danych w czasie kompilacji. Aby ułatwić zarządzanie konfiguracjami aplikacji, interfejs API konfiguracji umożliwia wyświetlenie danych, który będzie scalony ze wszystkich punktów w hierarchii konfiguracji jako jednej kolekcji, zamiast przeglądania danych jako oddzielne kolekcje z różnych pliki konfiguracji. Ponadto interfejs API konfiguracji umożliwia manipulowania konfiguracje dla całej aplikacji bez konieczności bezpośredniego edytowania pliku XML w plikach konfiguracji. Na koniec interfejsu API upraszcza zadania konfiguracji dzięki obsłudze narzędzi administracyjnych, takich jak narzędzie do administrowania witryną sieci Web. Interfejs API konfiguracji upraszcza wdrażanie obsługi tworzenia plików konfiguracyjnych na komputerze i uruchamiania skryptów konfiguracji na wielu komputerach.

> [!NOTE]
> Konfiguracja interfejsu API nie obsługuje tworzenia aplikacji programu IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Praca z ustawieniami konfiguracji lokalnych i zdalnych

Obiekt konfiguracji reprezentuje widok scalony ustawienia konfiguracji, które mają zastosowanie do określonej jednostki fizycznych, takich jak komputer, lub do jednostki logicznej, takie jak aplikacja lub witryna sieci Web. Określonej jednostki logicznej może istnieć na komputerze lokalnym lub na serwerze zdalnym. Gdy plik konfiguracji nie istnieje dla określonej jednostki, obiekt konfiguracji reprezentuje ustawienia konfiguracji domyślnej, zgodnie z definicją w pliku Machine.config.

Obiekt konfiguracji można uzyskać za pomocą jednej z metod otwórz konfigurację z następujących klas:

1. Klasy ConfigurationManager, jeśli jednostka jest aplikacją kliencką.
2. Klasa WebConfigurationManager, jeśli jednostki to aplikacja sieci Web.

Tych metod będzie zwracać obiekt konfiguracji, co z kolei zapewnia wymaganych metod i właściwości do obsługi plików konfiguracji podstawowej. Można uzyskać dostępu do tych plików do odczytu lub zapisu.

### <a name="reading"></a>Odczyt

Odczytywanie informacji o konfiguracji przy użyciu metody GetSection lub GetSectionGroup. Użytkownik lub proces, która odczytuje musi mieć uprawnienia do wszystkich plików konfiguracji w hierarchii odczytu.

> [!NOTE]
> Jeśli używasz metody statycznej GetSection, która przyjmuje parametr path parametr path musi odwoływać się do aplikacji, w którym wykonywany jest kod. W przeciwnym razie wartość parametru jest ignorowana, a informacje o konfiguracji dla aktualnie uruchomionych aplikacji jest zwracany.


### <a name="writing"></a>Zapisywanie

Użyj jednej z metod zapisywania można zapisać informacji o konfiguracji. Użytkownik lub proces, który musi mieć zapisy zapisać uprawnienia do pliku konfiguracji i katalog na bieżącym poziomie konfiguracji w hierarchii, a także uprawnienia do odczytu z wszystkie pliki konfiguracji w hierarchii.

Aby wygenerować plik konfiguracji, który reprezentuje ustawienia konfiguracji dziedziczone dla określonej jednostki, użyj jednej z następujących metod konfiguracji Zapisz:

1. Metody Zapisz, aby utworzyć nowy plik konfiguracji.
2. Metoda SaveAs Wygeneruj nowy plik konfiguracji w innej lokalizacji.

## <a name="configuration-classes-and-namespaces"></a>Klasy konfiguracji i przestrzenie nazw

Wiele konfiguracji klasy i metody są podobne do siebie. W poniższej tabeli opisano klasy najczęściej używane konfiguracji i przestrzenie nazw.

| **Konfiguracja klasy lub przestrzeni nazw** | **Opis** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) przestrzeni nazw | Zawiera klasy głównym konfiguracji dla wszystkich aplikacji .NET Framework. Klasy obsługi sekcji są używane do uzyskania danych konfiguracyjnych z sekcji z metod, takich jak GetSection i GetSectionGroup. Te dwie metody są niestatycznego. |
| System.Configuration.Configuration class | Reprezentuje zestaw danych o konfiguracji dla komputera, aplikacji, katalogów sieci Web lub innego zasobu. Ta klasa zawiera przydatne metody, takie jak GetSection i GetSectionGroup, aktualizowania ustawień konfiguracji i uzyskaniu odwołania do sekcji i grupy sekcji. Ta klasa jest używana jako typ zwracany dla metod, które uzyskują dane konfiguracji w czasie projektowania, takie jak metod klas WebConfigurationManager i program Configuration Manager. |
| System.Web.Configuration namespace | Zawiera klasy programu obsługi sekcji dla sekcji konfiguracji ASP.NET zdefiniowany w [ustawienia konfiguracji ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Klasy obsługi sekcji są używane do uzyskania danych konfiguracyjnych z sekcji z metod, takich jak GetSection i GetSectionGroup. |
| System.Web.Configuration.WebConfigurationManager class | Udostępnia metody przydatne dla uzyskania odwołania do ustawień konfiguracji środowiska wykonawczego i czasu projektowania. Te metody należy użyć klasy System.Configuration.Configuration jako typ zwracany. Zamiennie można użyć metody statycznej GetSection tej klasy lub Metoda niestatyczna GetSection klasy System.Configuration.ConfigurationManager. W przypadku konfiguracji aplikacji sieci Web zaleca się klasy System.Web.Configuration.WebConfigurationManager zamiast klasy System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | Umożliwia dostosowywanie i rozszerzanie dostawcę konfiguracji. Jest to klasa podstawowa dla wszystkich klas dostawców w systemie konfiguracji. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Zawiera klasy i interfejsy do zarządzania i monitorowania kondycji aplikacji sieci Web. Mówiąc ściślej ta przestrzeń nazw nie jest uznawane za część konfiguracji interfejsu API. Na przykład zdarzenie wywołujące i śledzenie odbywa się przez klasy w tej przestrzeni nazw. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) namespace | Udostępnia klasy, które są niezbędne do instrumentacji aplikacji do udostępnienia ich informacji dotyczących zarządzania i zdarzeń za pomocą Instrumentacji zarządzania Windows (WMI) do potencjalnych klientów. Monitorowanie kondycji programu ASP.NET używa usługi WMI do dostarczania zdarzeń. Mówiąc ściślej ta przestrzeń nazw nie jest uznawane za część konfiguracji interfejsu API. |

## <a name="reading-from-aspnet-configuration-files"></a>Odczyt z plików konfiguracji ASP.NET

Klasa WebConfigurationManager jest klasą core odczytu z plików konfiguracji ASP.NET. Dostępne są zasadniczo trzy kroki umożliwiające odczyt plików konfiguracji ASP.NET:

1. Pobierz obiekt konfiguracji przy użyciu metody OpenWebConfiguration.
2. Odwołać się do żądanego sekcji w pliku konfiguracji.
3. Żądane informacje do odczytu z pliku konfiguracji.

Konfigurację, którą reprezentuje obiekt nie reprezentuje pliku określonej konfiguracji. Zamiast tego reprezentuje widok scalony konfiguracji komputera, aplikacji lub witryny sieci Web. Poniższy przykład kodu tworzy obiekt konfiguracji reprezentujący konfigurację aplikacji sieci Web o nazwie *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Należy pamiętać, że jeśli /ProductInfo ścieżka nie istnieje, powyższy kod zwróci domyślnej konfiguracji określone w pliku machine.config.


Po utworzeniu obiektu konfiguracji, następnie umożliwia metody GetSection lub GetSectionGroup przejść do szczegółów w ustawieniach konfiguracji. Poniższy przykład pobiera odwołanie do ustawienia personifikacji aplikacji ProductInfo powyżej:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Zapisywanie w plikach konfiguracji ASP.NET

Jak Odczyt z plików konfiguracyjnych, klasa WebConfigurationManager jest podstawową na zapisywanie w plikach konfiguracji Asp.NET. Istnieją trzy kroki umożliwiające zapisywanie w plikach konfiguracji ASP.NET.

1. Pobierz obiekt konfiguracji przy użyciu metody OpenWebConfiguration.
2. Odwołać się do żądanego sekcji w pliku konfiguracji.
3. Zapis odpowiednie informacje z pliku konfiguracji przy użyciu Zapisz lub Zapisz jako metoda.

Poniższy kod zmiany **debugowania** atrybutu &lt;kompilacji&gt; elementu na wartość false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Po wykonaniu tego kodu **debugowania** atrybutu &lt;kompilacji&gt; element zostanie ustawiona na wartość false dla *aplikacji sieci Web* pliku web.config aplikacji.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management przestrzeń nazw zawiera klasy i interfejsy do zarządzania i monitorowania kondycji aplikacji ASP.NET.

Rejestrowanie odbywa się przez zdefiniowanie regułę, która kojarzy zdarzenia z dostawcą. Reguła określa typ zdarzenia, które są wysyłane do dostawcy. Następujące zdarzenia podstawowej są dostępne dla logowania:

| **WebBaseEvent** | Klasa podstawowa zdarzenia dla wszystkich zdarzeń. Zawiera wymagane właściwości dla wszystkich zdarzeń, takich jak: kod zdarzenia, kod szczegóły zdarzenia, Data i godzina zdarzenia został zgłoszony, sekwencji numer komunikatu o zdarzeniu i szczegóły zdarzenia. |
| --- | --- |
| **WebManagementEvent** | Klasa podstawowa zdarzenia dla zarządzania zdarzeniami, na przykład cykl życia aplikacji, żądanie błędów i zdarzeń inspekcji. |
| **WebHeartbeatEvent** | Zdarzenia generowane przez aplikację w regularnych odstępach czasu w celu przechwytywania runtime przydatne informacje o stanie. |
| **WebAuditEvent** | Klasa podstawowa dla zdarzenia inspekcji zabezpieczeń, które służą do oznaczania warunków, takich jak błąd autoryzacji, błąd odszyfrowywania *itp.* |
| **WebRequestEvent** | Klasa podstawowa dla wszystkich zdarzenia informacyjną żądania. |
| **WebBaseErrorEvent** | Klasa podstawowa dla wszystkich zdarzeń wskazujących warunki błędów. |

Typy dostawców, które są dostępne umożliwia wysyłanie danych wyjściowych zdarzeń do podglądu zdarzeń, SQL Server, Instrumentacja zarządzania Windows (WMI) i wiadomości e-mail. Wstępnie skonfigurowanych dostawców i mapowania zdarzeń zmniejszyć ilość pracy niezbędne, aby uzyskać dane wyjściowe zdarzenia rejestrowane.

Platforma ASP.NET 2.0 używa dziennika zdarzeń dostawcy out-of--box mają być rejestrowane zdarzenia na podstawie domen aplikacji, uruchamianie i zatrzymywanie, a także rejestrowanie wszelkich nieobsługiwanych wyjątków. Pozwala to obejmować niektórych scenariuszy podstawowych. Załóżmy na przykład, że aplikacja zgłasza wyjątek, ale użytkownik nie zostanie zapisany błąd i nie można go odtworzyć. Domyślna reguła dziennika zdarzeń będzie mógł zbierać informacje wyjątek i stos, aby lepiej zrozumieć, z jakiego rodzaju błąd wystąpił. Innym przykładem ma zastosowanie, gdy aplikacja jest utraty stanu sesji. W takim przypadku można sprawdzić w dzienniku zdarzeń, aby określić, czy domena aplikacji jest odtwarzanie i przyczyny zatrzymania domeny aplikacji w pierwszej kolejności.

Ponadto system monitorowania kondycji jest otwarty. Można na przykład definiowanie zdarzeń niestandardowych w sieci Web, wyzwalać je w aplikacji, a następnie zdefiniuj zasadę, aby wysyłać informacje dotyczące zdarzeń do dostawcy, takie jak adres e-mail. Dzięki temu można łatwo powiązanie z Instrumentacji do monitorowania dostawców kondycji. Inny przykład może wyzwalać zdarzeń zawsze kolejności jest przetwarzany i skonfigurować regułę, która wysyła każdego zdarzenia do bazy danych programu SQL Server. Można również uruchomić zdarzenie, gdy użytkownik zakończy się niepowodzeniem, zaloguj się na kilka razy pod rząd i skonfigurować zdarzenia do używania dostawcy pocztą e-mail.

Konfiguracja domyślnych dostawców i zdarzenia znajduje się w pliku Web.config globalnego. Globalne pliku Web.config są przechowywane wszystkie opartych na sieci Web ustawienia, które były przechowywane w pliku Machine.config w programie ASP.NET 1 x. Globalne plik Web.config znajduje się w następującym katalogu:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt; globalnego pliku Web.config zawiera domyślne ustawienia konfiguracji. Można zastąpić te ustawienia lub skonfigurować własne ustawienia zaimplementowanie &lt;healthMonitoring&gt; sekcji w pliku Web.config aplikacji.

&lt;HealthMonitoring&gt; globalnego pliku Web.config zawiera następujące elementy:

| **dostawców** | Zawiera dostawców dla podglądu zdarzeń, usługi WMI i SQL Server. |
| --- | --- |
| **eventMappings** | Zawiera mapowania dla różnych klas WebBase. Można rozszerzyć tę listę, jeśli Generowanie klasy zdarzeń. Generowanie klasy zdarzeń zapewnia bardziej szczegółowy za pośrednictwem dostawcy, możesz wysłać informacje. Na przykład można skonfigurować nieobsługiwanych wyjątków, które mają być wysyłane do programu SQL Server podczas wysyłania zdarzeń niestandardowych do poczty e-mail. |
| **reguły** | Łącza eventMappings dla dostawcy. |
| **buforowanie** | Używane z dostawcami programu SQL Server i wiadomości e-mail do określania, jak często opróżnić zdarzenia dla dostawcy. |

Poniżej podano przykładowy kod z globalnego pliku Web.config.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Sposobu przechowywania zdarzeń w Podglądzie zdarzeń

Jak wspomniano wcześniej, dostawcy do rejestrowania zdarzeń zdarzeń podglądu skonfigurowano dla Ciebie w pliku Web.config globalnego. Domyślnie na podstawie wszystkich zdarzeń **WebBaseErrorEvent** i **WebFailureAuditEvent** są rejestrowane. Możesz dodać dodatkowe reguły rejestrować dodatkowe informacje w dzienniku zdarzeń. Na przykład, jeśli chcesz rejestrować wszystkie zdarzenia (*tj.*, na podstawie każde zdarzenie **WebBaseEvent**), można dodać następujące reguły do pliku Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Ta zasada połączyć **wszystkie zdarzenia** mapę zdarzeń, aby dostawca dziennika zdarzeń. Zarówno eventMapping, jak i dostawcy są uwzględnione w pliku Web.config globalnego.

## <a name="how-to-store-events-to-sql-server"></a>Jak przechowywać zdarzenia z programem SQL Server

Ta metoda używa **ASPNETDB** bazy danych, która jest generowana przez Aspnet\_regsql.exe narzędzia. Domyślny dostawca używa LocalSqlServer ciąg połączenia, który używa albo na podstawie pliku bazy danych w aplikacji\_folderu danych lub lokalne wystąpienie programu SQLExpress programu SQL Server. Zarówno LocalSqlServer ciąg połączenia i SqlProvider są konfigurowane w pliku Web.config globalnego.

LocalSqlServer parametry połączenia w pliku Web.config globalnego wygląda następująco:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Jeśli chcesz użyć innego wystąpienia programu SQL Server, musisz użyć Aspnet\_regsql.exe narzędzia, które znajdują się w lokalizacji % windir%\Microsoft.Net\Framework\v2.0.\* folderu. Użyj Aspnet\_regsql.exe narzędzie do generowania niestandardowego **ASPNETDB** bazę danych w wystąpieniu programu SQL Server, a następnie dodaj parametry połączenia do pliku konfiguracji aplikacji, a następnie dodać dostawcę przy użyciu nowej Parametry połączenia. Po utworzeniu **ASPNETDB** utworzone bazy danych, musisz ustawić zasady, aby połączyć eventMapping sqlProvider.

Czy używać domyślnych SqlProvider lub skonfigurować własnego dostawcę, należy dodać regułę dostawca z mapą zdarzenia połączeń. Poniższa reguła łączy nowego dostawcę, który utworzone powyżej do **wszystkie zdarzenia** Mapa zdarzeń. Ta zasada będzie rejestrować wszystkie zdarzenia na podstawie **WebBaseEvent** i wysyłać je do MySqlWebEventProvider, który będzie używany ciąg połączenia MYASPNETDB. Poniższy kod dodaje regułę do łączenia się dostawcy z mapę zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Jeśli chcesz tylko błędy wysyłania do programu SQL Server, można dodać następujące reguły:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Jak przesłać zdarzenia do usługi WMI

Zdarzenia można również przekazywać do usługi WMI. Dostawca WMI jest skonfigurowany dla Ciebie w pliku Web.config globalnego domyślnie.

Poniższy przykładowy kod dodaje regułę do przekazywania zdarzenia do usługi WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Musisz dodać zasadę, aby skojarzyć eventMapping dostawcy i aplikacją odbiornika usługi WMI do nasłuchiwania zdarzeń. Poniższy przykładowy kod dodaje regułę do dostawcy WMI do łączenia **wszystkie zdarzenia** Mapa zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Jak przesłać zdarzenia do poczty e-mail

Można również przekazywania zdarzeń do poczty e-mail. Należy rozważnie reguły zdarzeń należy mapować dostawcę poczty e-mail mogą przypadkowo wysłania samodzielnie wiele informacji które mogą być lepiej nadaje się do programu SQL Server i dzienniku zdarzeń. Dwóch dostawców poczty e-mail; SimpleMailWebEventProvider i TemplatedMailWebEventProvider. Każdy ma takie same atrybuty konfiguracji, z wyjątkiem "szablon" i "detailedTemplateErrors" atrybuty, które są dostępne tylko na TemplatedMailWebEventProvider.

> [!NOTE]
> Żadna z tych dostawców poczty e-mail nie jest skonfigurowana za użytkownika. Należy dodać je do pliku Web.config.


Główną różnicą między tych dwóch dostawcy jest SimpleMailWebEventProvider i wysyła wiadomości e-mail w ogólnym szablonie, który nie może być modyfikowany. Przykładowy plik Web.config dodaje ten dostawca poczty e-mail do listy dostawców skonfigurowany za pomocą następujących reguł:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Poniższa reguła jest także dodawane do dostawcy poczty e-mail, aby powiązać **wszystkie zdarzenia** Mapa zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Platforma ASP.NET 2.0 śledzenia

Istnieją trzy najważniejsze ulepszenia do śledzenia w programie ASP.NET 2.0.

1. Śledzenie zintegrowane funkcje
2. Programowy dostęp do komunikatów śledzenia
3. Ulepszone śledzenie na poziomie aplikacji

## <a name="integrated-tracing-functionality"></a>Zintegrowane śledzenie funkcji

Można teraz rozesłać komunikaty emitowane przez klasę System.Diagnostics.Trace do śledzenia danych wyjściowych programu ASP.NET i kierowania wiadomości emitowane przez śledzenie na platformie ASP.NET do System.Diagnostics.Trace. Można również przekazywać zdarzeń Instrumentacji ASP.NET do System.Diagnostics.Trace. Ta funkcjonalność jest udostępniana przez nowy **writeToDiagnosticsTrace** atrybutu &lt;śledzenia&gt; elementu. Jeśli ta wartość logiczna ma wartość true, komunikaty śledzenia ASP.NET są przekazywane do infrastruktury śledzenia System.Diagnostics do użytku przez wszystkie obiekty nasłuchujące zarejestrowanych w taki sposób, aby wyświetlić komunikaty śledzenia.

## <a name="programmatic-access-to-trace-messages"></a>Programowy dostęp do komunikatów śledzenia

Programowy dostęp do wszystkich komunikatów śledzenia za pomocą programu ASP.NET 2.0 umożliwia **TraceContextRecord** klasy i **///TraceRecords** kolekcji. Jest najbardziej wydajny sposób uzyskiwania dostępu do śledzenia komunikatów do zarejestrowania **TraceContextEventHandler** delegata (również nowe w programie ASP.NET 2.0) do obsługi nowej **TraceFinished** zdarzeń. Następnie można przeglądać komunikaty śledzenia, który ma.

Poniższy przykładowy kod przedstawia to:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

W powyższym przykładzie I pętli ///TraceRecords kolekcji, a następnie wpisz każdy komunikat do strumienia odpowiedzi.

## <a name="improved-application-level-tracing"></a>Ulepszone śledzenie na poziomie aplikacji

Śledzenie na poziomie aplikacji zostały ulepszone za pośrednictwem wprowadzenie nowego **mostRecent** atrybutu &lt;śledzenia&gt; elementu. Ten atrybut określa, czy jest wyświetlany najnowsze dane wyjściowe śledzenia na poziomie aplikacji i starsze dane niemieszczące ograniczeń, które są oznaczone requestLimit zostaną odrzucone. W przypadku wartości FAŁSZ, aż do osiągnięcia atrybut requestLimit dane śledzenia są wyświetlane dla żądań.

## <a name="aspnet-command-line-tools"></a>Narzędzia wiersza polecenia platformy ASP.NET

Istnieje kilka narzędzi wiersza polecenia, aby ułatwić konfigurację programu ASP.NET. Deweloperów platformy ASP.NET, należy się zapoznać z aspnet\_regiis.exe narzędzia. ASP.NET 2.0 zawiera trzy inne narzędzia wiersza polecenia do pomocy w konfiguracji.

Dostępne są następujące narzędzia wiersza polecenia:

| **Narzędzie** | **Użyj** |
| --- | --- |
| **aspnet\_regiis.exe** | Umożliwia rejestracji programu ASP.NET w usługach IIS. Istnieją dwie wersje tego narzędzia dostarczonymi z programu ASP.NET 2.0, jeden dla systemów 32-bitowych (w folderze Framework) i jeden dla 64-bitowe systemy (w folderze Framework64.) 64-bitowa wersja nie zostanie zainstalowana na 32-bitowego systemu operacyjnego. |
| **aspnet\_regsql.exe** | Narzędzie rejestracji serwera SQL programu ASP.NET jest używany do tworzenia bazy danych programu Microsoft SQL Server do użytku przez dostawców programu SQL Server w programie ASP.NET, lub do dodawania lub usuwania opcji z istniejącej bazy danych. Aspnet\_regsql.exe plik znajduje się w [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber folder na serwerze sieci Web. |
| **aspnet\_regbrowsers.exe** | Narzędzie rejestracji programu ASP.NET w przeglądarce analizuje i kompiluje wszystkich definicji przeglądarki systemowe w zestawie i instaluje zestawu w globalnej pamięci podręcznej zestawów. Narzędzie używa plików definicji przeglądarki (. Pliki PRZEGLĄDARKI) z podkatalogu .NET Framework przeglądarki. Narzędzie znajduje się w katalogu %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | ASP.NET Compilation tool umożliwia kompilowanie aplikacji sieci Web ASP.NET w miejscu lub wdrożenia do lokalizacji docelowej, takich jak serwer produkcyjny. W miejscu kompilacji pomaga wydajność aplikacji, ponieważ użytkownicy końcowi czy nie występują opóźnienia na pierwsze żądanie do aplikacji, podczas kompilowania aplikacji. |

Ponieważ aspnet\_narzędzia regiis.exe nie jesteś nowym użytkownikiem programu ASP.NET 2.0, nie omówimy go tutaj.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Narzędzie rejestracji serwera SQL programu ASP.NET - aspnet\_regsql.exe

Można ustawić różne opcje za pomocą narzędzia rejestracji serwera SQL programu ASP.NET. Można określić połączenie SQL, określić które usługami aplikacji ASP.NET przy użyciu programu SQL Server zarządzanie informacjami, określ, które bazy danych lub tabeli jest używane dla zależności bufora SQL i dodać lub usunąć obsługę przy użyciu programu SQL Server do przechowywania procedur i stanu sesji.

Kilka usług aplikacji ASP.NET korzystają z dostawcy zarządzania przechowywania i pobierania danych ze źródła danych. Każdy dostawca jest specyficzna dla źródła danych. Program ASP.NET zawiera dostawcy programu SQL Server, następujące funkcje platformy ASP.NET:

- Członkostwo ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) klasy).
- Zarządzanie rolami ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) klasy).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) klasy).
- Personalizacja części w sieci Web ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) klasy).
- Zdarzenia w sieci Web ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) klasy).

Po zainstalowaniu programu ASP.NET pliku Machine.config serwera zawiera elementy konfiguracji, które określają dostawcy programu SQL Server dla każdej funkcji programu ASP.NET, które zależą od dostawcy. Ci dostawcy są konfigurowane domyślnie, aby nawiązać połączenie użytkownika lokalnego wystąpienia programu SQL Server Express 2005. Jeśli zmienisz domyślny ciąg połączenia używany przez dostawców, przed użyciem dowolnej funkcji programu ASP.NET w konfiguracji maszyny należy zainstalować bazy danych programu SQL Server i elementy bazy danych z wybranej funkcji przy użyciu Aspnet\_regsql.exe. Jeśli bazy danych przez użytkownika za pomocą narzędzia rejestracji SQL jeszcze nie istnieje (aspnetdb będzie domyślna baza danych, jeśli nie został określony w wierszu polecenia), a następnie bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w programie SQL Server oraz do tworzenia schematu e lements w bazie danych.

### <a name="sql-cache-dependency"></a>Zależności bufora SQL

Zaawansowanych funkcji buforowania danych wyjściowych programu ASP.NET jest zależności bufora SQL. Zależności bufora SQL obsługuje dwa różne tryby działania: korzystającą z implementacją ASP.NET sondowania tabeli i drugi tryb, który korzysta z funkcji powiadomień zapytania programu SQL Server 2005. Narzędzie rejestracji SQL można skonfigurować tryb sondowania tabeli operacji.

### <a name="session-state"></a>Stan sesji

Domyślnie wartości stanu sesji i informacje są przechowywane w pamięci w ramach procesu ASP.NET. Alternatywnie można przechowywać dane sesji w bazie danych programu SQL Server, gdzie mogą być współużytkowane przez wiele serwerów sieci Web. Jeśli jeszcze nie istnieje określony dla stanu sesji za pomocą narzędzia rejestracji SQL bazy danych, bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w programie SQL Server oraz do tworzenia elementów schematu w bazie danych. Jeśli baza danych istnieje, bieżący użytkownik musi mieć uprawnienia do tworzenia elementów schemat z istniejącej bazy danych.

Aby zainstalować bazę danych stanu sesji na serwerze SQL, uruchom Aspnet\_narzędzie regsql.exe i podaj następujące informacje przy użyciu polecenia:

- Nazwa programu SQL Server wystąpienia przy użyciu **-S** opcji.
- Poświadczenia logowania dla konta, które ma uprawnienia do tworzenia bazy danych na komputerze z uruchomionym programem SQL Server. Użyj **-E** opcję, aby użyć aktualnie zalogowanego użytkownika lub **- U** opcję, aby określić identyfikator użytkownika wraz z **-P** opcję, aby określić hasła.
- **- Ssadd** opcji wiersza polecenia, aby dodać bazy danych stanów sesji.

Domyślnie nie można użyć Aspnet\_regsql.exe narzędzia do zainstalowania bazy danych stanu sesji na komputerze z systemem SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET Browser Registration Tool - aspnet\_regbrowsers.exe

W programie ASP.NET w wersji 1.1 pliku Machine.config zawarte sekcji o nazwie &lt;browserCaps&gt;. Ta sekcja zawiera szereg wpisów XML, które zdefiniowane konfiguracje dla różnych przeglądarkach, na podstawie wyrażenia regularnego. Dla platformy ASP.NET w wersji 2.0 nowy. Plik PRZEGLĄDARKI definiuje parametry przeglądarki przy użyciu wpisów XML. Możesz dodać informacje w nowym oknie przeglądarki, dodając nową. PRZEGLĄDARKA plików do folderu zlokalizowanego w %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers w systemie.

Ponieważ aplikacja nie jest odczytywanie pliku .config, za każdym razem, gdy wymaga informacji przeglądarki, możesz utworzyć nowy. PRZEGLĄDARKA plików i wykonywania Aspnet\_regbrowsers.exe Aby dodać wymagane zmiany do zestawu. Dzięki temu serwer bezpośrednio dostęp do nowych informacji przeglądarki, dzięki czemu nie trzeba zamknąć którekolwiek aplikacje do pobrania informacji. Aplikacja ma dostęp do funkcji przeglądarki za pośrednictwem przeglądarki właściwości bieżącego elementu HttpRequest.

Dostępne są następujące opcje, podczas uruchamiania aspnet\_regbrowser.exe:

| **Opcja** | **Opis** |
| --- | --- |
| **-?** | Wyświetla Aspnet\_regbbrowsers.exe tekst pomocy w oknie wiersza polecenia. |
| **-i** | Tworzy zestaw funkcji wykonywania przeglądarki i instaluje je w pamięci podręcznej GAC. |
| **-u** | Odinstalowuje zestaw funkcji wykonywania przeglądarki z globalnej pamięci podręcznej zestawów. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET Compilation Tool - aspnet\_compiler.exe

Narzędzia kompilacji platformy ASP.NET można używać na dwa sposoby ogólne: dla kompilacji w miejscu i kompilacja do wdrożenia, gdy określono katalog wyjściowy docelowej.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilowanie aplikacji w miejscu](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET Compilation tool można skompilować aplikację w miejscu, czyli naśladuje zachowanie dokonywania wiele żądań do aplikacji, powodując regularne kompilacji. Użytkowników witryny wstępnie skompilowanym nie będą występować opóźnienia spowodowane kompilowania strony na pierwsze żądanie.

Prekompilowanie lokacji w miejscu, zastosuj następujące elementy:

- Witryny zachowuje swoje pliki i struktury katalogów.
- Musi mieć kompilatory dla wszystkich języków programowania, używane przez witryny na serwerze.
- W przypadku niepowodzenia kompilacji dowolnego pliku całej witryny niepowodzenia kompilacji.

Można również ponowne skompilowanie aplikacji w miejscu po dodaniu nowych plików źródłowych do niego. Narzędzie kompiluje tylko nowe lub zmienione pliki, chyba że uwzględniasz **- c** opcji.

> [!NOTE]
> Kompilacja aplikacji, która zawiera zagnieżdżone aplikacji nie kompiluje się zagnieżdżonych aplikacji. Zagnieżdżone aplikacji muszą być skompilowane oddzielnie.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilowanie aplikacji do wdrożenia](https://msdn.microsoft.com/library/ms229863.aspx)

Określenie parametru targetDir kompilowania aplikacji do wdrożenia (kompilacji do lokalizacji docelowej). TargetDir może być lokalizacji końcowej dla aplikacji sieci Web lub skompilowanej aplikacji można wdrożyć dodatkowe. Przy użyciu **-u** opcji kompiluje aplikacji w taki sposób, że bez konieczności ponownego kompilowania go można wprowadzić zmiany niektórych plików w skompilowanej aplikacji. ASPNET\_compiler.exe rozróżnia typów plików statycznych i dynamicznych i obsługuje je inaczej, podczas tworzenia aplikacji wynikowy.

- Typy plików statycznych są tymi, które nie mają skojarzonych kompilatora lub utworzyć dostawcy, takich jak pliki źródłowe nazwanego mają rozszerzenia, takie jak CSS, GIF, htm, HTML, jpg, js i tak dalej. Te pliki po prostu są kopiowane do lokalizacji docelowej, ich względne miejsc w strukturze katalogów zachowane.
- Typy plików dynamiczne są tymi, które mają skojarzone kompilatora lub utworzyć dostawcy, w tym pliki z rozszerzeniami specyficzne dla platformy ASP.NET, takich jak .asax, .ascx ashx, .aspx, .browser, .master i tak dalej. ASP.NET Compilation tool generuje zestawy z tych plików. Jeśli **-u** opcja zostanie pominięta, narzędzie tworzy także pliki z rozszerzeniem nazwy pliku. COMPILED mapujące oryginalnych plików źródłowych do ich zestawu. W celu zapewnienia zachowywanie struktury katalogu źródłowego aplikacji, narzędzie generuje pliki symbol zastępczy w odpowiednich lokalizacjach w aplikacji docelowej.

Należy użyć **-u** opcję, aby wskazać, czy zawartość skompilowana aplikacja może być modyfikowany. W przeciwnym razie wartość kolejne zmiany są ignorowane lub powodować błędy środowiska wykonawczego.

W poniższej tabeli opisano, jak inny plik uchwyty narzędzia kompilacji platformy ASP.NET typy kiedy **-u** znajduje się opcja.

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .ascx, .aspx, .master | Te pliki są podzielone na znaczników i źródła kodu, który obejmuje zarówno plików z kodem i kodu, który jest ujęta w &lt;skryptu runat = "server"&gt; elementów. Kod źródłowy jest kompilowany do zestawów z nazwami pochodzących z algorytmem wyznaczania wartości skrótu i zestawy są umieszczane w katalogu Bin. Każdy kod wbudowanego, ujętą w kodzie  **&lt; %**  i  **% &gt;**  nawiasy, znajduje się kod znaczników i nie został skompilowany. Nowe pliki z taką samą nazwę jak pliki źródłowe są zawiera kod znaczników i umieszczane w odpowiednich katalogów wyjściowych. |
| .ashx, .asmx | Pliki te nie są kompilowane i zostaną przeniesione do katalogów wyjściowych i nie został skompilowany. Jeśli chcesz, aby skompilować kod obsługi, umieść kod do plików kodu źródłowego w aplikacji\_katalog kodów. |
| .CS, .vb, .jsl, .cpp (nie w tym pliki CodeBehind dla podanych wcześniej typów plików) | Te pliki są kompilowane i uwzględniane jako zasób w zestawy, które odwołują się do nich. Pliki źródłowe nie są kopiowane do katalogu wyjściowego. Jeśli nie odwołuje się do pliku kodu, jest nie skompilowany. |
| Niestandardowe typy plików | Te pliki nie zostały skompilowane. Te pliki są kopiowane do odpowiednich katalogów wyjściowych. |
| Pliki kodu w aplikacji źródłowe\_podkatalog kodu | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. |
| pliki resx i .resource w aplikacji\_podkatalogu GlobalResources | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. Nie aplikacji\_podkatalogu GlobalResources jest tworzony w katalogu głównym danych wyjściowych, a żadne pliki resx lub .resources, znajduje się w katalogu źródłowym są kopiowane do katalogów wyjściowych. |
| pliki resx i .resource w aplikacji\_podkatalogu LocalResources | Pliki te nie są kompilowane i są kopiowane do odpowiednich katalogów wyjściowych. |
| pliki .skin w aplikacji\_podkatalogu motywów | Pliki .skin i pliki statyczne motywu nie są kompilowane i są kopiowane do odpowiednich katalogów wyjściowych. |
| .Browser plik Web.config statyczne typy zestawy znajduje się już w katalogu Bin | Te pliki są kopiowane jako katalogów wyjściowych. |

W poniższej tabeli opisano, jak inny plik uchwyty narzędzia kompilacji platformy ASP.NET typy kiedy **-u** opcja zostanie pominięta.

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Te pliki są podzielone na znaczników i źródła kodu, który obejmuje zarówno plików z kodem i kodu, który jest ujęta w &lt;skryptu runat = "server"&gt; elementów. Kod źródłowy jest kompilowany do zestawów z nazwami pochodzących z algorytmu wyznaczania wartości skrótu. Wynikowa zestawy są umieszczane w katalogu Bin. Każdy kod wbudowanego, ujętą w kodzie  **&lt; %**  i  **% &gt;**  nawiasy, znajduje się kod znaczników i nie został skompilowany. Kompilator tworzy nowe pliki zawierają znaczników z taką samą nazwę jak plików źródłowych. Te pliki wynikowe są umieszczane w katalogu Bin. Kompilator również tworzy pliki, z taką samą nazwę jak pliki źródłowe, ale z rozszerzeniem. COMPILED, które zawierają informacje dotyczące mapowania. . SKOMPILOWANE pliki są umieszczane w katalogów wyjściowych odpowiadającego do oryginalnej lokalizacji plików źródłowych. |
| .ascx | Te pliki są dzielone na znaczników i kod źródłowy. Kod źródłowy jest skompilowany do zestawów i umieszczane w katalogu Bin o nazwach, pochodzących z algorytmu wyznaczania wartości skrótu. Są generowane żadne pliki znaczników. |
| .CS, .vb, .jsl, .cpp (nie w tym pliki CodeBehind dla podanych wcześniej typów plików) | Kod źródłowy, który odwołuje się do niego zestawów wygenerowanych z .ascx, ashx lub plików aspx kompilacji do zestawów i umieszczane w katalogu Bin. Nie pliki źródłowe zostają skopiowane. |
| Niestandardowe typy plików | Te pliki są kompilowane takich jak pliki dynamiczne. Zależnie od typu plików, które są oparte na kompilator można umieścić mapowania plików w katalogach danych wyjściowych. |
| Pliki w aplikacji\_podkatalog kodu | Pliki kodu źródłowego w tym podkatalogu kompilowania do zestawów i umieszczane w katalogu Bin. |
| Pliki w aplikacji\_podkatalogu GlobalResources | Te pliki są kompilowane do zestawów i umieszczane w katalogu Bin. Nie aplikacji\_podkatalogu GlobalResources jest tworzony w katalogu głównym danych wyjściowych. Jeśli plik konfiguracji określa appliesTo = "All", pliki resx i .resources są kopiowane do katalogów wyjściowych. Nie są kopiowane jeśli odwołują się [elementu BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| pliki resx i .resource w aplikacji\_podkatalogu LocalResources | Pliki te są łączone w zestawy, używając unikatowej nazwy i umieszczane w katalogu Bin. Żadne pliki resx lub .resource są kopiowane do katalogów wyjściowych. |
| pliki .skin w aplikacji\_podkatalogu motywów | Kompozycje są kompilowane do zestawów i umieszczane w katalogu Bin. Pliki szczątkowe są tworzone dla plików .skin i umieszczane w odpowiedniej katalogu wyjściowego. Pliki statyczne (na przykład .css) są kopiowane do katalogów wyjściowych. |
| .Browser plik Web.config statyczne typy zestawy znajduje się już w katalogu Bin | Te pliki są kopiowane, ponieważ jest do katalogu wyjściowego. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nazw zestawów stałej](https://msdn.microsoft.com/library/ms229863.aspx##)

Niektóre scenariusze, takie jak wdrażanie aplikacji sieci Web za pomocą pliku MSI Instalatora Windows, wymagają użycia nazwy plików spójne i zawartość, a także struktur katalogów spójne do identyfikowania zestawów lub ustawienia konfiguracji dla aktualizacji. W takich przypadkach można użyć **- fixednames** opcję, aby określić, że ASP.NET Compilation tool należy skompilować zestawu dla każdego pliku źródłowego, zamiast where wiele stron są kompilowane do zestawów. Może to prowadzić do wielu zestawów, dlatego jeśli dotyczy skalowalność tej opcji należy używać ostrożnie.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Strong-Name Compilation](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysign**, **- keycontainer** i **- keyfile** opcje są dostępne, aby mogli używać Aspnet\_ zestawy o nazwie Compiler.exe można zdecydowanie utworzyć bez użycia [silnej nazwy narzędzia (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) oddzielnie. Te opcje odpowiadają odpowiednio do **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**i  **AssemblyKeyFileAttribute**.

Omówienie tych atrybutów jest poza zakresem tego kursu.

## <a name="labs"></a>Labs

Każdy z poniższych labs opiera się na poprzedniej labs. Należy wykonać je w kolejności.

## <a name="lab-1-using-the-configuration-api"></a>Laboratorium 1: Interfejs API konfiguracji przy użyciu

1. Utwórz nową witrynę sieci Web o nazwie *mod9lab*.
2. Dodaj nowy plik konfiguracji sieci Web do witryny.
3. Dodaj następującą wartość do pliku web.config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Zapewni to, że masz uprawnienia do zapisywania zmian w pliku web.config.

1. Dodaj nową kontrolkę etykiety do Default.aspx i zmień identyfikator **lblDebugStatus**.
2. Dodaj nową kontrolkę przycisku do Default.aspx.
3. Zmień identyfikator formantu przycisku do **btnToggleDebug** i tekst, który **stan debugowania Przełącz**.
4. Otwórz widok kodu dla kodu powiązanego pliku Default.aspx i Dodaj **przy użyciu** instrukcji dla **System.Web.Configuration** w następujący sposób:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Dodaj dwie zmienne prywatnej klasy i stronę\_metody Init, jak pokazano poniżej:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Dodaj następujący kod do strony\_obciążenia:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Zapisz i Przeglądaj default.aspx. Zwróć uwagę, że kontrolka etykiety wyświetla bieżący stan debugowania.
2. Kliknij dwukrotnie formantu przycisku w Projektancie i Dodaj następujący kod do Zdarzenie kliknięcia formantu przycisku:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Zapisz i Przeglądaj default.aspx i kliknij przycisk.
2. Otwórz plik web.config po każdym przycisku kliknij i obserwować **debugowania** atrybutu w &lt;kompilacji&gt; sekcji.

## <a name="lab-2-logging-application-restarts"></a>Laboratorium 2: Rejestrowanie aplikacja zostanie uruchomiona ponownie

W tym laboratorium spowoduje utworzenie kodu, który umożliwi Przełącz rejestrowanie zamknięcia aplikacji, uruchomienia i ponowne kompilacje w Podglądzie zdarzeń.

1. Dodaj DropDownList do default.aspx i zmień identyfikator ddlLogAppEvents.
2. Ustaw **AutoPostBack** właściwość DropDownList do **true**.
3. Dodaj trzy elementy do kolekcji elementów dla DropDownList. Wprowadź **tekst** pierwszego elementu *Select Value* i wartość -1. Należy **tekst** i **wartość** drugiego elementu **True** i **tekst** i **wartość** trzeciego elementu **False**.
4. Dodaj nową etykietę do default.aspx. Zmień identyfikator do **lblLogAppEvents**.
5. Otwórz widok CodeBehind dla default.aspx i dodać nowe oświadczenie dla zmiennej typu HealthMonitoringSection, jak pokazano poniżej:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Dodaj następujący kod do istniejącego kodu na stronie\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Kliknij dwukrotnie DropDownList i Dodaj następujący kod do zdarzenia SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Przeglądaj default.aspx.
2. Wartość elementu dropdown **False**.
3. Wyczyść dziennik aplikacji w Podglądzie zdarzeń.
4. Kliknij przycisk, aby zmienić atrybut debugowania dla aplikacji.
5. Odśwież dziennik aplikacji w Podglądzie zdarzeń. 

    1. Zarejestrowano zdarzenia?
    2. Dlaczego lub dlaczego nie?
6. Wartość elementu dropdown **wartość True.**
7. Kliknij przycisk, aby przełączyć atrybutu debugowania dla aplikacji.
8. Odśwież logowania aplikacji Podgląd zdarzeń. 

    1. Zarejestrowano zdarzenia?
    2. Jaki był przyczynę zamknięcia aplikacji?
9. Eksperymentować Włączanie i wyłączanie rejestrowania i przyjrzyj się zmiany wprowadzone w pliku web.config.

## <a name="more-information"></a>Więcej informacji:

Platforma ASP.NET 2.0 w dostawcy modelu służy do tworzenia własnych dostawców dla nie tylko instrumentacji aplikacji, ale dla wielu innych zastosowań członkostwa, profile, np. Aby uzyskać szczegółowe informacje na temat pisania niestandardowego dostawcy do dziennika zdarzeń aplikacji do pliku tekstowego, odwiedź stronę [to łącze](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
