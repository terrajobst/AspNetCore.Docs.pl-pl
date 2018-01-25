---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: "Opis profilu aplikacji usługi ASP.NET AJAX uwierzytelniania i | Dokumentacja firmy Microsoft"
author: scottcate
description: "Usługa uwierzytelniania umożliwia użytkownikom o podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania i Usługa bramy umożliwia użytkownikom niestandardowych..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 182276f9f91b99beb1ce0fc40dcda1f19376669a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Uwierzytelnianie ASP.NET AJAX i profilu usługi aplikacji
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Usługa uwierzytelniania umożliwia użytkownikom o podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania, a Usługa bramy umożliwia profilów użytkownika niestandardowego jest dostarczane przez platformę ASP.NET. Korzystanie z usługi uwierzytelniania ASP.NET AJAX jest zgodna ze standardowego uwierzytelniania formularzy ASP.NET, więc aktualnie używa uwierzytelniania formularzy aplikacji (na przykład za pomocą nazwy logowania kontrolować) będzie nie podzielony przez uaktualnienie do usługi uwierzytelniania AJAX.


## <a name="introduction"></a>Wprowadzenie

W ramach programu .NET Framework 3.5 Microsoft jest dostarczanie uaktualniania środowiska może być zmieniany. nie tylko są dostępne nowe środowisko deweloperskie, ale nadchodzącego są nowe funkcje język Language-Integrated zapytania (LINQ) i inne rozszerzenia języka. Ponadto niektóre funkcje zapoznać się z innymi procesami, szczególnie rozszerzeń programu ASP.NET AJAX, są dołączaniu jako najwyższej jakości członkami Biblioteka klas programu .NET Framework Base. Te rozszerzenia włączyć wiele nowych funkcji wzbogaconego klienta, w tym częściowe renderowanie stron bez konieczności odświeżania strony pełne, możliwość dostępu za pośrednictwem skrypt po stronie klienta (w tym profilowania API ASP.NET) i szeroką gamę po stronie klienta interfejsu API usług sieci Web przeznaczony do duplikatów wiele systemów kontroli w zestawie formantu po stronie serwera ASP.NET.

Ten dokument sprawdza wdrożenia i stosowania profilowania ASP.NET i usługi uwierzytelniania formularzy, ponieważ są one udostępniane przez Microsoft ASP.NET AJAX ExtensionsThe AJAX rozszerzenia ułatwiają uwierzytelnianie formularzy niezwykle obsługi, ponieważ (a także Usługi profilowania) jest za pośrednictwem skryptu serwera proxy usługi sieci Web. Rozszerzenia interfejsu AJAX również obsługiwać niestandardowe uwierzytelnianie za pośrednictwem klasy AuthenticationServiceManager.

Ten dokument jest oparty na wersji Beta 2 programu Visual Studio 2008 i .NET Framework 3.5. Ten dokument również założono, że będzie działać z programu Visual Studio 2008 w wersji Beta 2 nie Visual Web Developer Express i zapewnia wskazówki zgodnie z interfejsu użytkownika programu Visual Studio. Przykładowy kod, mogą używać szablonów projektu jest niedostępne w przypadku programu Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profile i uwierzytelniania*

Microsoft ASP.NET profile i usługami uwierzytelniania są dostarczane przez system uwierzytelnianie formularzy aplikacji ASP.NET i są standardowe składniki programu ASP.NET. Rozszerzenia programu ASP.NET AJAX zapewniają skryptu dostęp do tych usług za pomocą skryptu proxy, za pomocą bardzo prosta modelu w przestrzeni nazw Sys.Services biblioteki klienckiej AJAX.

Usługa uwierzytelniania umożliwia użytkownikom o podanie poświadczeń w celu odbierania pliku cookie uwierzytelniania, a Usługa bramy umożliwia profilów użytkownika niestandardowego jest dostarczane przez platformę ASP.NET. Korzystanie z usługi uwierzytelniania ASP.NET AJAX jest zgodna ze standardowego uwierzytelniania formularzy ASP.NET, więc aktualnie używa uwierzytelniania formularzy aplikacji (na przykład za pomocą nazwy logowania kontrolować) będzie nie podzielony przez uaktualnienie do usługi uwierzytelniania AJAX.

Usługa profilu umożliwia automatyczne integracji i przechowywania danych użytkownika na podstawie członkostwa dostarczone przez usługę uwierzytelniania. Określono przechowywanych danych przez plik web.config i profilowania różnych dostawców usług obsługi zarządzania danymi. Podobnie jak w przypadku usługi uwierzytelniania usługa AJAX profilu jest zgodny z standardowe usługą profilów platformy ASP.NET, aby nie należy dzielić stron obecnie włączenia funkcji usługi profilu platformy ASP.NET dzięki dodaniu obsługi interfejsu AJAX.

Dołączanie do aplikacji ASP.NET uwierzytelnienia i uwierzytelnienia usługi profilowania, wykracza poza zakres tego dokumentu. Aby uzyskać więcej informacji na temat można znaleźć w bibliotece MSDN odwołania artykule Zarządzanie użytkownikami przy użyciu członkostwa w [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). Program ASP.NET zawiera również narzędzia, aby automatycznie skonfigurować członkostwa z programem SQL Server, który jest domyślnego uwierzytelniania usługi dostawcy członkostwa ASP.NET. Aby uzyskać więcej informacji, zobacz artykuł ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) na [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Przy użyciu usługi uwierzytelniania AJAX ASP.NET*

Musi być włączona usługa Uwierzytelnianie ASP.NET AJAX w pliku web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Usługa uwierzytelniania wymaga można włączyć uwierzytelnianie formularzy ASP.NET i wymaga plików cookie w przeglądarce klienta (skryptu nie można włączyć sesji bez plików cookie, ponieważ sesje bez plików cookie wymaga parametrów adresu URL.).

Gdy usługa AJAX uwierzytelniania jest włączona i skonfigurowana, skrypt po stronie klienta można natychmiast zalet obiektu Sys.Services.AuthenticationService. Przede wszystkim skrypt po stronie klienta będzie przeprowadzać `login` — metoda i `isLoggedIn` właściwości. Istnieje kilka właściwości zapewnienie wartości domyślnych w przypadku metody logowania może akceptować dużej liczby parametrów.

*Sys.Services.AuthenticationService members*

*Metoda logowania:*

Metoda: login() rozpoczyna żądanie uwierzytelnienia poświadczeń użytkownika. Ta metoda jest asynchroniczne, a nie blokuje wykonywania.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| userName | Wymagany. Nazwa użytkownika do uwierzytelniania. |
| Hasło | Opcjonalnie (wartość domyślna to null). Hasło użytkownika. |
| isPersistent | Opcjonalnie (wartość domyślna to false). Określa, czy plik cookie uwierzytelniania użytkownika powinien być zachowywane między sesjami. W przypadku wartości FAŁSZ, użytkownik będzie Wyloguj się przy zamykaniu przeglądarki lub wygaśnięcia sesji. |
| redirectUrl | Opcjonalnie (wartość domyślna to null). Adres URL przekierowania przeglądarkę, aby po pomyślnym uwierzytelnieniu. Jeśli ten parametr ma wartość null lub pusty ciąg, przekierowanie nie występuje. |
| customInfo | Opcjonalnie (wartość domyślna to null). Ten parametr jest aktualnie używana i jest zarezerwowany do użytku w przyszłości. |
| loginCompletedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy logowanie zostało pomyślnie ukończone. Jeśli jest określony, ten parametr zastępuje właściwość defaultLoginCompleted. |
| failedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy logowanie nie powiodło się. Jeśli jest określony, ten parametr zastępuje właściwość defaultFailedCallback. |
| Parametr userContext | Opcjonalnie (wartość domyślna to null). Dane kontekstu użytkownika niestandardowego, który powinien zostać przekazany do funkcji wywołania zwrotnego. |

*Zwracana wartość:*

Ta funkcja nie zawiera wartości zwracanej. Jednak liczbę zachowań znajdują się po zakończeniu wywołania tej funkcji:

- Bieżąca strona albo zostaną odświeżone lub zmienić, jeśli `redirectUrl` parametr ma wartość null ani pustym ciągiem.
- Jednakże, jeśli parametr ma wartość null lub pusty ciąg, `loginCompletedCallback` parametru lub `defaultLoginCompletedCallback` nosi nazwę właściwości.
- Jeśli wystąpi błąd wywołania usługi sieci web, `failedCallback` parametr `defaultFailedCallback` nosi nazwę właściwości.

*Metoda wylogowania.*

Metoda logout() usuwa plik cookie poświadczeń i wylogowaniem bieżącego użytkownika z aplikacji sieci web.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| redirectUrl | Opcjonalnie (wartość domyślna to null). Adres URL przekierowania przeglądarkę, aby po pomyślnym uwierzytelnieniu. Jeśli ten parametr ma wartość null lub pusty ciąg, przekierowanie nie występuje. |
| logoutCompletedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy wylogowanie zostało zakończone pomyślnie. Jeśli jest określony, ten parametr zastępuje właściwość defaultLogoutCompleted. |
| failedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy logowanie nie powiodło się. Jeśli jest określony, ten parametr zastępuje właściwość defaultFailedCallback. |
| Parametr userContext | Opcjonalnie (wartość domyślna to null). Dane kontekstu użytkownika niestandardowego, który powinien zostać przekazany do funkcji wywołania zwrotnego. |

*Zwracana wartość:*

Ta funkcja nie zawiera wartości zwracanej. Jednak liczbę zachowań znajdują się po zakończeniu wywołania tej funkcji:

- Bieżąca strona albo zostaną odświeżone lub zmienić, jeśli `redirectUrl` parametr ma wartość null ani pustym ciągiem.
- Jednakże, jeśli parametr ma wartość null lub pusty ciąg, `logoutCompletedCallback` parametru lub `defaultLogoutCompletedCallback` nosi nazwę właściwości.
- Jeśli wystąpi błąd wywołania usługi sieci web, `failedCallback` parametr `defaultFailedCallback` nosi nazwę właściwości.

*Właściwość defaultFailedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana w przypadku niepowodzenia do komunikowania się z usługą sieci web. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| Błąd | Określa informacje o błędzie. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas wywoływania funkcji logowania lub wylogowania. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość defaultLoginCompletedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po ukończeniu wywołania usługi sieci web logowania. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| validCredentials | Określa, czy użytkownik podał prawidłowe poświadczenia. `true`Jeśli użytkownik pomyślnie zalogował się; w przeciwnym razie `false`. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas wywoływania funkcji logowania. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość defaultLogoutCompletedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu Wyloguj wywołania usługi sieci web. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| wynik | Ten parametr będzie zawsze równa `null`; jest on zarezerwowany do użytku w przyszłości. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas wywoływania funkcji logowania. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość isLoggedIn (get):*

Ta właściwość pobiera bieżący stan uwierzytelniania użytkownika; jest on ustawiony przez obiekt ScriptManager podczas żądania strony.

Ta właściwość zwraca `true` Jeśli użytkownik jest zalogowany, w przeciwnym, zwraca `false`.

*Właściwość Path (get, set):*

Ta właściwość określa programowo lokalizację usługi sieci web uwierzytelniania. Może służyć do zastępowania domyślnego dostawcę uwierzytelniania, a także jeden deklaratywnie ustawić we właściwości ścieżka węzła podrzędnego AuthenticationService formantu ScriptManager (Aby uzyskać więcej informacji, zobacz Używanie niestandardowego dostawcę usługi uwierzytelniania temat poniżej).

Należy pamiętać, że nie zmienia lokalizację domyślną usługą uwierzytelniania. Jednak ASP.NET AJAX umożliwia określenie lokalizacji usługi sieci web, który zawiera ten sam interfejs klasy jako serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy pamiętać, że nie można ustawić tę właściwość na wartość kieruje żądanie skryptu wylogowuje bieżącej lokacji. Ponieważ bieżąca aplikacja nie może pobrać poświadczenia uwierzytelniania, będzie bezcelowe; Ponadto AJAX podstawowych technologii nie zadać żądań cross-site i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest `String` obiekt reprezentujący ścieżkę do usługi sieci web uwierzytelniania.

*Właściwość limitu czasu (get, set):*

Ta właściwość określa czas oczekiwania na usługę uwierzytelniania przed zakładając, że żądanie logowania nie powiodła się. Po przekroczeniu limitu czasu podczas oczekiwania na zakończenie wywołania, żądanie nie powiodło się wywołanie zwrotne zostanie wywołana, a połączenie nie zostanie ukończona.

Ta właściwość jest `Number` obiekt reprezentujący wyrażony w milisekundach czas oczekiwania na wyniki z usługą uwierzytelniania.

*Przykładowy kod: Logowanie do usługi uwierzytelniania*

Następujący kod znaczników jest przykładową stronę ASP.NET przy użyciu prostego skryptu wywołania do zalogowania i wylogowania metod klasy AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Uzyskiwanie dostępu do profilowania danych za pomocą technologii AJAX ASP.NET

Profilowania usługi ASP.NET jest również dostępne za pośrednictwem rozszerzeń AJAX programu ASP.NET. Ponieważ usługi profilowania ASP.NET udostępnia rozbudowane, szczegółowego interfejsu API za pomocą której do przechowywania i pobierania danych użytkownika, może to być narzędziem doskonałą wydajność.

Musi być włączona usługa profil w pliku web.config; nie jest domyślnie. Aby to zrobić, upewnij się, że `profileService` włączył element podrzędny = true określony w pliku web.config i określeniu właściwości, które mogły być odczytywane i zapisywane w następujący sposób:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Należy również skonfigurować usługę profilu. Mimo że konfiguracji usługi profilowania poza zakres tego dokumentu, warto zauważyć, że grup zgodnie z definicją w ustawieniach profilu konfiguracji będzie dostępna jako podrzędne właściwości nazwy grupy. Na przykład z poniższej sekcji profilu określony:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Skrypt po stronie klienta będzie mogła uzyskiwać dostęp do nazwy, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip i BackgroundColor jako właściwości pola właściwości klasy elementu ProfileService.

Po skonfigurowaniu usługi profilowania AJAX będzie natychmiast dostępny na stronach; jednak ma być załadowana raz przed użyciem.

*Sys.Services.ProfileService members*

*pole właściwości:*

W polu właściwości udostępnia wszystkie dane skonfigurowanego profilu jako właściwości podrzędnej, które mogą odwoływać się Konwencji nazwa kropka operator. Właściwości, które są elementami podrzędnymi grup właściwości są określane jako GroupName.PropertyName. W konfiguracji profilu przykład przedstawionych powyżej Aby uzyskać stan użytkownika, można użyć następującego identyfikatora:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metoda obciążenia:*

Ładuje właściwości wszystkich lub wybranej listy z serwera.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyNames | Opcjonalnie (wartość domyślna to null). Właściwości, które mają zostać załadowane z serwera. |
| loadCompletedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy ładowanie zostało zakończone. |
| failedCallback | Opcjonalnie (wartość domyślna to null). Funkcji do wywołania w przypadku wystąpienia błędu. |
| Parametr userContext | Opcjonalnie (wartość domyślna to null). Informacje o kontekście mają być przekazane do funkcji wywołania zwrotnego. |

Funkcja obciążenia nie ma wartości zwracanej. Jeśli wywołanie zostało ukończone pomyślnie, zostanie wywołany albo `loadCompletedCallback` parametru lub `defaultLoadCompletedCallback` właściwości. Jeśli nie powiodło się lub upłynął limit czasu, albo `failedCallback` parametru lub `defaultFailedCallback` właściwość zostanie wywołana.

Jeśli `propertyNames` nie podano parametru, wszystkie skonfigurowane odczytu właściwości są pobierane z serwera.

*Zapisz metody:*

Metoda Zapisz() zapisuje listę właściwości określonego (lub wszystkie właściwości) do profilu ASP.NET.

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyNames | Opcjonalnie (wartość domyślna to null). Właściwości, które mają być zapisywane na serwerze. |
| saveCompletedCallback | Opcjonalnie (wartość domyślna to null). Funkcja wywoływana, gdy zapisywania została ukończona. |
| failedCallback | Opcjonalnie (wartość domyślna to null). Funkcji do wywołania w przypadku wystąpienia błędu. |
| Parametr userContext | Opcjonalnie (wartość domyślna to null). Informacje o kontekście mają być przekazane do funkcji wywołania zwrotnego. |

Zapisz funkcja nie ma wartości zwracanej. Jeśli wywołanie zostało ukończone pomyślnie, zostanie wywołany albo `saveCompletedCallback` parametru lub `defaultSaveCompletedCallback` właściwości. Jeśli nie powiodło się lub upłynął limit czasu, albo `failedCallback` lub `defaultFailedCallback` właściwość zostanie wywołana.

Jeśli `propertyNames` parametr ma wartość null, wszystkie właściwości profilu zostaną wysłane do serwera i serwera podejmie decyzję, właściwości, które mogą być zapisywane i które nie.

*Właściwość defaultFailedCallback (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana w przypadku niepowodzenia do komunikowania się z usługą sieci web. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| Błąd | Określa informacje o błędzie. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość defaultSaveCompleted (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu zapisywania danych profilu użytkownika. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsSaved | Określa liczbę właściwości, które zostały zapisane. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość defaultLoadCompleted (get, set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu ładowania danych profilu użytkownika. Klient powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określonej przez tę właściwość powinien mieć następującą sygnaturą:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametry:*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsLoaded | Określa liczbę właściwości załadowane. |
| Parametr userContext | Określa informacje o kontekście użytkownika, które zostały podane podczas ładowania lub zapisywania funkcja została wywołana. |
| MethodName | Nazwa metody wywołującej. |

*Właściwość Path (get, set):*

Ta właściwość określa programowo lokalizację profilu usługi sieci web. Może służyć do zastępowania domyślnego dostawcy profilu usługi, a także jedną deklaratywnie ustawić we właściwości ścieżka węzła podrzędnego elementu ProfileService formantu ScriptManager.

Należy pamiętać, że nie zmienia lokalizację usługi profil domyślny. Jednak ASP.NET AJAX umożliwia określenie lokalizacji usługi sieci web, który zawiera ten sam interfejs klasy jako serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy pamiętać, że nie można ustawić tę właściwość na wartość kieruje żądanie skryptu wylogowuje bieżącej lokacji. AJAX podstawowych technologii nie zadać żądań cross-site i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest `String` obiekt reprezentujący ścieżkę profilu usługi sieci web.

*Właściwość limitu czasu (get, set):*

Ta właściwość określa czas oczekiwania na usługę profilu przed przy założeniu obciążenia lub Zapisz żądanie nie powiodło się. Po przekroczeniu limitu czasu podczas oczekiwania na zakończenie wywołania, żądanie nie powiodło się wywołanie zwrotne zostanie wywołana, a połączenie nie zostanie ukończona.

Ta właściwość jest `Number` obiekt reprezentujący wyrażony w milisekundach czas oczekiwania na wyniki z usługi profilu.

*Przykładowy kod: podczas ładowania danych profilu na załadowanie strony*

Następujący kod, aby zobaczyć, czy użytkownik jest uwierzytelniany i jeśli tak, załadowanie kolor tła preferowanym przez użytkownika jako strony.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Przy użyciu dostawcy usługi uwierzytelniania niestandardowego*

Rozszerzenia AJAX ASP.NET umożliwiają tworzenie dostawcę usług uwierzytelniania niestandardowego skryptu w przypadku wystawianego z funkcji za pośrednictwem usługi sieci web niestandardowego. Aby mogły być używane, usługi sieci web musi ujawniać dwie metody `Login` i `Logout`; i z tego samego podpisy metod jako domyślna usługa sieci web ASP.NET AJAX uwierzytelniania należy określić te metody.

Po utworzeniu usługi sieci web niestandardowe, należy określić ścieżkę do niego, deklaratywnego na stronie programowo w kodzie, lub za pomocą skryptu klienta.

*Aby ustawić deklaratywnie ścieżki:*

Można deklaratywnie ustawić ścieżki, obejmują podrzędnych AuthenticationService obiektu ScriptManager na stronie ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Aby ustawić ścieżkę w kodzie:*

Aby ustawić ścieżkę programowo, określ ścieżkę za pomocą wystąpienia menedżera skryptu:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Aby ustawić ścieżkę w skrypcie:*

Ustawianie ścieżki programowo w skrypcie, należy skorzystać `path` właściwość klasy AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Przykład usługi sieci Web dla uwierzytelniania niestandardowego*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Podsumowanie

Usługi ASP.NET — w szczególności usług profilowania, członkostwo i uwierzytelnianie — łatwe są widoczne dla języka JavaScript w przeglądarce klienta. Dzięki temu deweloperzy mogą integrują swój kod po stronie klienta z mechanizmu uwierzytelniania, bez zależności od formanty, takie jak UpdatePanels celu lifting ciężki. Dane profilu mogą być chronione od klienta, jak również przy użyciu ustawień konfiguracji sieci web; nie są domyślnie dostępne żadne dane, a deweloperzy musi wyrazić zgodę na właściwości profilu.

Ponadto tworząc implementacji usługi sieci web uproszczony z sygnaturami metod równoważne, deweloperzy mogą tworzyć skryptu niestandardowego dostawcy dla tych wewnętrznych usług platformy ASP.NET. Obsługę tych technik upraszcza tworzenie zaawansowanych aplikacji klienckich, jednocześnie zapewniając deweloperzy szeroką gamę elastyczność do konkretnych potrzeb.

## <a name="bio"></a>*Bio*

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Poprzednie](understanding-asp-net-ajax-updatepanel-triggers.md)
[dalej](understanding-asp-net-ajax-localization.md)
