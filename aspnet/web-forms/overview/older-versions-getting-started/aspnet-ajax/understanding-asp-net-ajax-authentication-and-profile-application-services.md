---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Opis uwierzytelniania AJAX programu ASP.NET i usług aplikacji w profilu | Dokumentacja firmy Microsoft
author: scottcate
description: Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania, a to Usługa bramy, aby umożliwić użytkownikowi niestandardowe...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 6c08cffacb9ebde6f29398f53b2e568b4bd59d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831683"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Opis uwierzytelniania AJAX programu ASP.NET i usług aplikacji w profilu
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania i usługi bramy, aby umożliwić profilów użytkownika niestandardowego jest dostarczane przez platformę ASP.NET. Korzystanie z usługi Uwierzytelnianie ASP.NET AJAX jest zgodny z standardowe uwierzytelnianie formularzy programu ASP.NET, dzięki czemu aplikacje, które obecnie z uwierzytelniania formularzy (na przykład za pomocą identyfikatora logowania kontrolować) będzie nie podzielony przez uaktualnienie do usługi uwierzytelniania AJAX.


## <a name="introduction"></a>Wprowadzenie

W ramach programu .NET Framework 3.5 Microsoft świadczy usługi uaktualniania środowiska może być zmieniany. nie tylko nowe środowisko opracowywania dostępne, ale zapowiadane są nowe funkcje Language-Integrated Query (LINQ) i inne rozszerzenia języka. Ponadto niektóre znane funkcje inne zestawy narzędzi, szczególnie rozszerzenia AJAX programu ASP.NET, są uwzględniane jako najwyższej jakości członkowie Biblioteka klas programu .NET Framework Base. Te rozszerzenia umożliwiają wiele nowych funkcji rozbudowanych aplikacji klienckich, w tym częściowe renderowanie stron bez konieczności odświeżenie całej strony, możliwość dostępu do usług sieci Web za pośrednictwem skrypt po stronie klienta (w tym ASP.NET, profilowania API) i rozbudowane API po stronie klienta przeznaczony do utworzenia duplikatów, wiele systemów kontroli w zestaw formantów po stronie serwera ASP.NET.

Tym oficjalnym dokumencie patrzy na wdrożenie i stosowanie profilowania ASP.NET i usług uwierzytelniania formularzy, ponieważ są one dostępne po rozszerzenia AJAX programu Microsoft ASP.NET AJAX ExtensionsThe upewnij uwierzytelniania formularzy niezwykle łatwo jest obsługiwać, ponieważ (także Usługi profilowania) jest dostępna za pośrednictwem skryptu serwera proxy usługi sieci Web. Rozszerzenia AJAX również obsługuje niestandardowe uwierzytelnianie za pośrednictwem klasy AuthenticationServiceManager.

Ten dokument jest oparty na wersji Beta 2 programu Visual Studio 2008 i programu .NET Framework 3.5. Tym oficjalnym dokumencie przyjęto założenie, będzie on pracować przy użyciu programu Visual Studio 2008 Beta 2, nie Visual Web Developer Express, i zapewnia wskazówki, zgodnie z interfejsu użytkownika programu Visual Studio. Przykłady kodu mogą korzystać z szablonów projektów w Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profile i uwierzytelniania*

Microsoft ASP.NET profilów i usług uwierzytelniania są dostarczane przez system uwierzytelnianie formularzy programu ASP.NET i są składniki standardowe programu ASP.NET. Rozszerzenia AJAX programu ASP.NET umożliwiają skryptu dostęp do tych usług za pomocą skryptu serwerów proxy, za pomocą bardzo prosta modelu w przestrzeni nazw Sys.Services biblioteki klienta AJAX.

Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania i usługi bramy, aby umożliwić profilów użytkownika niestandardowego jest dostarczane przez platformę ASP.NET. Korzystanie z usługi Uwierzytelnianie ASP.NET AJAX jest zgodny z standardowe uwierzytelnianie formularzy programu ASP.NET, dzięki czemu aplikacje, które obecnie z uwierzytelniania formularzy (na przykład za pomocą identyfikatora logowania kontrolować) będzie nie podzielony przez uaktualnienie do usługi uwierzytelniania AJAX.

Usługa profilu umożliwia automatyczne integracji i przechowywanie danych użytkownika na podstawie przynależności do udostępnionych przez usługę uwierzytelniania. Przechowywanych danych jest określona przez plik web.config i różnych dostawców usług profilowania obsługi zarządzania danymi. Podobnie jak w przypadku usługi uwierzytelniania usługa AJAX profilu jest zgodny z standardowa usługą profilów platformy ASP.NET, tak, aby nie należy dzielić stron obecnie włączenie funkcji usługi profil programu ASP.NET, łącznie z obsługą technologii AJAX.

Dołączanie do aplikacji uwierzytelnianie programu ASP.NET i uwierzytelnienia usługi profilowania, wykracza poza zakres tego dokumentu. Aby uzyskać więcej informacji na temat, zobacz biblioteki MSDN Library odwoływać się do artykułu Zarządzanie użytkownikami przy użyciu członkostwa w [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). Program ASP.NET zawiera również narzędzia, aby automatycznie skonfigurować członkostwo z programem SQL Server, która jest domyślnego dostawcę usługi uwierzytelniania dla członkostwa ASP.NET. Aby uzyskać więcej informacji, zobacz artykuł narzędzia rejestracji serwera SQL platformy ASP.NET (Aspnet\_regsql.exe) na [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Przy użyciu usługi uwierzytelniania platformy ASP.NET AJAX*

Usługa uwierzytelniania AJAX programu ASP.NET, należy włączyć w pliku web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Usługa uwierzytelniania wymaga uwierzytelniania formularzy programu ASP.NET, aby włączyć i wymaga plików cookie w celu włączenia w przeglądarce klienta (skrypt nie można włączyć cookieless sesji, ponieważ cookieless sesji wymaga parametrów adresu URL.).

Gdy usługa uwierzytelniania AJAX jest włączone i skonfigurowane, skrypt po stronie klienta mogą od razu korzystać Sys.Services.AuthenticationService obiektu. Skrypt po stronie klienta będzie przede wszystkim chcesz korzystać z zalet `login` metody i `isLoggedIn` właściwości. Istnieje kilka właściwości do zapewnienia wartości domyślne metody logowania, która może akceptować dużą liczbą parametrów.

*Sys.Services.AuthenticationService members*

*Metoda logowania:*

Metoda: login() rozpoczyna żądanie uwierzytelnienia poświadczeń użytkownika. Ta metoda jest asynchroniczne i nie blokuje wykonywania.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| userName | Wymagana. Nazwa użytkownika do uwierzytelniania. |
| Hasło | Atrybut opcjonalny (wartość domyślna to null). Hasło użytkownika. |
| isPersistent | Atrybut opcjonalny (wartość domyślna to false). Czy plik cookie uwierzytelniania użytkownika ma utrwalić między sesjami. W przypadku wartości FAŁSZ użytkownika powoduje wylogowanie, gdy nastąpi zamknięcie okna przeglądarki lub ważności sesji. |
| redirectUrl | Atrybut opcjonalny (wartość domyślna to null). Adres URL do przekierowania wyszukiwarki na pomyślne uwierzytelnienie. Jeśli ten parametr ma wartość null lub pusty ciąg, przekierowanie nie występuje. |
| customInfo | Atrybut opcjonalny (wartość domyślna to null). Ten parametr jest aktualnie używana i jest zarezerwowany do użytku w przyszłości. |
| loginCompletedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania po pomyślnym ukończeniu logowania. Jeśli zostanie określony, ten parametr zastępuje właściwość defaultLoginCompleted. |
| failedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja wywoływana, gdy nazwa logowania nie powiodła się. Jeśli zostanie określony, ten parametr zastępuje właściwość defaultFailedCallback. |
| userContext | Atrybut opcjonalny (wartość domyślna to null). Dane kontekstu użytkownika niestandardowego, które powinny być przekazywane do funkcji wywołania zwrotnego. |

*Wartość zwracana:*

Ta funkcja nie ma wartości zwracanej. Jednak wiele zachowań są uwzględniane na wykonanie wywołania tej funkcji:

- Bieżąca strona albo zostanie odświeżona lub zmienić, jeśli `redirectUrl` parametr ma wartość null ani pusty ciąg.
- Jednakże, jeśli parametr ma wartość null lub pusty ciąg, `loginCompletedCallback` parametru lub `defaultLoginCompletedCallback` nosi nazwę właściwości.
- Jeśli wywołanie usługi sieci web ulegnie awarii, `failedCallback` parametru `defaultFailedCallback` nosi nazwę właściwości.

*Metoda wylogowania:*

Metoda logout() usuwa plik cookie poświadczenia i wylogowuje bieżącego użytkownika z aplikacji sieci web.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| redirectUrl | Atrybut opcjonalny (wartość domyślna to null). Adres URL do przekierowania wyszukiwarki na pomyślne uwierzytelnienie. Jeśli ten parametr ma wartość null lub pusty ciąg, przekierowanie nie występuje. |
| logoutCompletedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania po pomyślnym ukończeniu wylogowanie. Jeśli zostanie określony, ten parametr zastępuje właściwość defaultLogoutCompleted. |
| failedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja wywoływana, gdy nazwa logowania nie powiodła się. Jeśli zostanie określony, ten parametr zastępuje właściwość defaultFailedCallback. |
| userContext | Atrybut opcjonalny (wartość domyślna to null). Dane kontekstu użytkownika niestandardowego, które powinny być przekazywane do funkcji wywołania zwrotnego. |

*Wartość zwracana:*

Ta funkcja nie ma wartości zwracanej. Jednak wiele zachowań są uwzględniane na wykonanie wywołania tej funkcji:

- Bieżąca strona albo zostanie odświeżona lub zmienić, jeśli `redirectUrl` parametr ma wartość null ani pusty ciąg.
- Jednakże, jeśli parametr ma wartość null lub pusty ciąg, `logoutCompletedCallback` parametru lub `defaultLogoutCompletedCallback` nosi nazwę właściwości.
- Jeśli wywołanie usługi sieci web ulegnie awarii, `failedCallback` parametru `defaultFailedCallback` nosi nazwę właściwości.

*Właściwość defaultFailedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana, jeśli wystąpi błąd podczas komunikowania się z usługą sieci web. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| Błąd | Określa informacje o błędzie. |
| userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas logowania lub wylogowywania funkcja została wywołana. |
| methodName | Nazwa metody wywołującej. |

*Właściwość defaultLoginCompletedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana, gdy wywołanie usługi sieci web logowania zostało zakończone. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| validCredentials | Określa, czy użytkownik podał prawidłowe poświadczenia. `true` Jeśli użytkownik pomyślnie zalogował się; w przeciwnym razie `false`. |
| userContext | Określa informacje kontekstu użytkownika, które są dostarczane, gdy wywołana została funkcja logowania. |
| methodName | Nazwa metody wywołującej. |

*Właściwość defaultLogoutCompletedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu wywołania usługi sieci web wylogowania. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| wynik | Ten parametr będzie zawsze `null`; jest zarezerwowany do użytku w przyszłości. |
| userContext | Określa informacje kontekstu użytkownika, które są dostarczane, gdy wywołana została funkcja logowania. |
| methodName | Nazwa metody wywołującej. |

*Właściwość isLoggedIn (get):*

Tej właściwości pobiera bieżący stan uwierzytelniania użytkownika; jest ustawiony przez obiekt ScriptManager w trakcie żądania strony.

Ta właściwość zwraca `true` Jeśli użytkownik jest aktualnie zalogowany w; w przeciwnym razie, zwracany jest `false`.

*PATH — właściwość (get, set):*

Ta właściwość określa programowo lokalizację uwierzytelniania usługi sieci web. Może służyć do zastępowania domyślnego dostawcę uwierzytelniania, a także jedną deklaratywne Ustawianie we właściwości ścieżki formantu ScriptManager AuthenticationService podrzędnym (Aby uzyskać więcej informacji, zobacz Używanie niestandardowego dostawcę usługi uwierzytelniania temat poniżej).

Pamiętaj, że nie zmienia lokalizację domyślnej usługi uwierzytelnienia. ASP.NET AJAX pozwala jednak określić lokalizację usługi sieci web, który zawiera ten sam interfejs klasy jako serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy zauważyć, że nie można ustawić tę właściwość na wartość, która kieruje żądanie skryptu zniżki w stosunku do bieżącej lokacji. Ponieważ bieżąca aplikacja nie otrzyma poświadczenia uwierzytelniania, jest bezcelowe; Ponadto AJAX podstawowych technologii nie zadać żądań między witrynami i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest `String` obiekt reprezentujący ścieżkę do usługi sieci web uwierzytelniania.

*Właściwość limitu czasu (get, set):*

Ta właściwość określa, że czas oczekiwania dla usługi uwierzytelniania przed zakładając, że żądanie logowania nie powiodła się. Jeśli upłynie limit czasu podczas oczekiwania na wywołanie, aby zakończyć, zostanie wywołana metoda wywołania zwrotnego żądanie nie powiodło się, a połączenie nie zostanie ukończona.

Ta właściwość jest `Number` obiekt reprezentujący liczbę milisekund oczekiwania na wyniki z usługi uwierzytelniania.

*Przykładowy kod: Logowanie do usługi uwierzytelniania*

Następujące znaczniki to przykładowa strona ASP.NET za pomocą wywołania prosty skrypt do zalogowania i wylogowania metod klasy AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Uzyskiwanie dostępu do danych za pomocą technologii AJAX profilowania ASP.NET

ASP.NET profilowanie usługi również jest dostępna za pośrednictwem rozszerzenia AJAX programu ASP.NET. Ponieważ usługa profilowania ASP.NET udostępnia precyzyjnie interfejsu API za pomocą którego do przechowywania i pobierania danych użytkownika, może to być narzędziem doskonałej produktywności.

Musi być włączona usługa profilu w pliku web.config; nie jest domyślnie. Aby to zrobić, upewnij się, że `profileService` element podrzędny ma włączone = true określonych w pliku web.config i określeniu właściwości, które może odczytać lub zapisywane w następujący sposób:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Usługa profilu muszą również zostać skonfigurowane. Mimo że konfiguracji usługi profilowania wykracza poza zakres tego dokumentu, warto pamiętać, grupy, zgodnie z definicją w ustawieniach konfiguracji profilu będą dostępne jako właściwości podrzędnych nazwy grupy. Na przykład z poniższej sekcji profilu określony:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Skrypt po stronie klienta będzie mogła uzyskiwać dostęp do nazwy, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip i BackgroundColor jako właściwości pola właściwości klasy ProfileService.

Po skonfigurowaniu usługi profilowania AJAX będą natychmiast dostępne w stron. jednak ma być załadowana raz przed użyciem.

*Sys.Services.ProfileService members*

*pole właściwości:*

Pole właściwości udostępnia wszystkie dane profilu skonfigurowanego jako właściwości podrzędnych, które mogą być przywoływane zgodnie z Konwencją nazwa kropka operator. Właściwości, które są elementami podrzędnymi grup właściwości są określane jako GroupName.PropertyName. W konfiguracji profilu przykład przedstawiony powyżej Aby uzyskać stan użytkownika, można użyć następującego identyfikatora:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metoda obciążenia:*

Ładuje właściwości wszystkich lub wybranej listy z serwera.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyNames | Atrybut opcjonalny (wartość domyślna to null). Właściwości, które ma zostać załadowane z serwera. |
| loadCompletedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania po zakończeniu ładowania. |
| failedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania w przypadku wystąpienia błędu. |
| userContext | Atrybut opcjonalny (wartość domyślna to null). Informacje o kontekście mają być przekazane do funkcji wywołania zwrotnego. |

Funkcja obciążenia nie ma wartości zwracanej. Jeśli wywołanie zostało ukończone pomyślnie, a jej będzie wywoływać parametr albo `loadCompletedCallback` parametru lub `defaultLoadCompletedCallback` właściwości. Jeśli wywołanie nie powiodło się lub upłynął limit czasu, albo `failedCallback` parametru lub `defaultFailedCallback` właściwość zostanie wywołana.

Jeśli `propertyNames` parametr nie jest podany, wszystkie właściwości skonfigurowane odczytu są pobierane z serwera.

*Zapisz metody:*

Metoda Zapisz() zapisuje listę właściwości określonego (lub wszystkie właściwości), do profilu ASP.NET.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyNames | Atrybut opcjonalny (wartość domyślna to null). Właściwości, które mają być zapisane na serwerze. |
| saveCompletedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania podczas zapisywania zostało zakończone. |
| failedCallback | Atrybut opcjonalny (wartość domyślna to null). Funkcja do wywołania w przypadku wystąpienia błędu. |
| userContext | Atrybut opcjonalny (wartość domyślna to null). Informacje o kontekście mają być przekazane do funkcji wywołania zwrotnego. |

Zapisz funkcja nie ma wartości zwracanej. Jeśli wywołanie zakończy się pomyślnie, go, wywoła albo `saveCompletedCallback` parametru lub `defaultSaveCompletedCallback` właściwości. Jeśli wywołanie nie powiodło się lub upłynął limit czasu, albo `failedCallback` lub `defaultFailedCallback` właściwość zostanie wywołana.

Jeśli `propertyNames` parametr ma wartość null, wszystkie właściwości profilu zostaną wysłane do serwera i serwera podejmie decyzję, właściwości, które mogą być zapisywane, i które nie mogą.

*Właściwość defaultFailedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana, jeśli wystąpi błąd podczas komunikowania się z usługą sieci web. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| Błąd | Określa informacje o błędzie. |
| userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| methodName | Nazwa metody wywołującej. |

*Właściwość defaultSaveCompleted (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu zapisując dane profilu. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsSaved | Określa liczbę właściwości, które zostały zapisane. |
| userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| methodName | Nazwa metody wywołującej. |

*Właściwość defaultLoadCompleted (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu ładowania danych profilu użytkownika. Otrzymasz delegata (lub odwołanie do funkcji).

Dokumentacja funkcji określonej przez tę właściwość powinna mieć następujący podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsLoaded | Określa liczbę właściwości ładowane. |
| userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| methodName | Nazwa metody wywołującej. |

*PATH — właściwość (get, set):*

Ta właściwość określa programowo lokalizacja profilem usługi sieci web. Może służyć do zastępowania domyślnego dostawcę usługi profilu, a także jedną deklaratywne Ustawianie we właściwości ścieżki węzeł podrzędny ProfileService formantu ScriptManager.

Pamiętaj, że nie zmienia lokalizację domyślnej usługi profilu. ASP.NET AJAX pozwala jednak określić lokalizację usługi sieci web, który zawiera ten sam interfejs klasy jako serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy zauważyć, że nie można ustawić tę właściwość na wartość, która kieruje żądanie skryptu zniżki w stosunku do bieżącej lokacji. AJAX podstawowych technologii nie zadać żądań między witrynami i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest `String` obiekt reprezentujący ścieżka do profilu usługi sieci web.

*Właściwość limitu czasu (get, set):*

Ta właściwość określa, że czas oczekiwania dla usługi profilu zakładając, że obciążenie, lub Zapisz żądanie nie powiodło się. Jeśli upłynie limit czasu podczas oczekiwania na wywołanie, aby zakończyć, zostanie wywołana metoda wywołania zwrotnego żądanie nie powiodło się, a połączenie nie zostanie ukończona.

Ta właściwość jest `Number` obiekt reprezentujący liczbę milisekund oczekiwania na wyniki z usługi profilu.

*Przykładowy kod: podczas ładowania profilu danych ładowania strony*

Poniższy kod zostanie Sprawdź, czy użytkownik jest uwierzytelniany i jeśli tak, ładuje kolor tła preferowany przez użytkownika jako strony.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Za pomocą dostawcy usług uwierzytelniania niestandardowego*

Rozszerzenia AJAX programu ASP.NET umożliwiają tworzenie dostawcy usługi uwierzytelniania niestandardowego skryptu, zapewniając swoje funkcje za pośrednictwem usługi sieci web niestandardowego. Aby możliwe było użycie usługi sieci web należy ujawnić dwie metody `Login` i `Logout`; i te metody musi być określona za pomocą tego samego podpisy metod jako domyślnej usługi sieci web ASP.NET AJAX uwierzytelniania.

Po utworzeniu usługi sieci web niestandardowego należy określić ścieżkę do niego deklaratywne na stronie, programowo w kodzie lub za pośrednictwem skryptu klienta.

*Aby ustawić ścieżkę w sposób deklaratywny:*

Aby ustawić ścieżkę w sposób deklaratywny, obejmują podrzędnych AuthenticationService obiektu ScriptManager na stronie ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Aby ustawić ścieżkę w kodzie:*

Aby programowo ustawić ścieżkę, należy określić ścieżkę za pośrednictwem wystąpienia menedżera skryptu:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Aby ustawić ścieżkę w skrypcie:*

Aby programowo ustawić ścieżkę w skrypcie, wykorzystywać `path` właściwość klasy AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Przykładowe usługi sieci Web dla uwierzytelniania niestandardowego*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Podsumowanie

Usługi ASP.NET — w szczególności usługi profilowania, członkostwo i uwierzytelnianie — łatwe są widoczne dla języka JavaScript w przeglądarce klienta. Dzięki temu deweloperzy mogą bezproblemowo integrują się ich kod po stronie klienta przy użyciu mechanizmu uwierzytelniania, bez zależności od kontrolek, takich jak UpdatePanels celu ciężkie obciążenia. Dane profilowe mogą być chronione od klienta, jak również przy użyciu ustawień konfiguracji sieci web; żadne dane nie są domyślnie dostępne, a deweloperzy muszą wyrazić zgodę na właściwości profilu.

Ponadto tworząc implementacji usługi uproszczone sieci web za pomocą podpisów metody równoważne, deweloperzy mogą tworzyć dostawcy niestandardowego skryptu dla tych wewnętrznych usług ASP.NET. Obsługa tych technik upraszcza tworzenie zaawansowanych aplikacji klienckich, przy jednoczesnym zapewnieniu deweloperom z szerokiej gamy elastyczności w celu spełnienia określonych wymagań.

## <a name="bio"></a>*Bio*

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-updatepanel-triggers.md)
> [dalej](understanding-asp-net-ajax-localization.md)
