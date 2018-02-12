---
uid: web-api/overview/security/authentication-filters
title: "Filtry uwierzytelniania w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Filtr uwierzytelniania jest składnikiem, który uwierzytelnia żądanie HTTP. Interfejs API sieci Web 2 i MVC 5 obsługuje zarówno filtry uwierzytelniania, ale różnią się nieco..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtry uwierzytelniania w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Filtr uwierzytelniania jest składnikiem, który uwierzytelnia żądanie HTTP. Interfejs API sieci Web 2 i MVC 5 obsługuje zarówno filtry uwierzytelniania, ale różnią się nieznacznie, przede wszystkim konwencje nazewnictwa dla interfejsu filtru. W tym temacie opisano filtry uwierzytelniania interfejsu API sieci Web.


Filtry uwierzytelniania pozwalają na ustawienie schematu uwierzytelniania do poszczególnych kontrolerów lub akcji. W ten sposób aplikacja może obsługiwać różnych mechanizmów uwierzytelniania dla różnych zasobów HTTP.

W tym artykule opisano I kod z [uwierzytelnianie podstawowe](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) w [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Przykład obejmuje filtr uwierzytelniania, który implementuje schemat uwierzytelniania dostępu do podstawowego HTTP (RFC 2617). Filtr jest zaimplementowana w klasie o nazwie `IdentityBasicAuthenticationAttribute`. I nie będzie zawierać cały kod z próbki, tylko części ilustrujące jak napisać filtr uwierzytelniania.

## <a name="setting-an-authentication-filter"></a>Ustawienie filtru uwierzytelniania

Podobnie jak inne filtry filtry uwierzytelniania może być zastosowane na kontrolerze, -action lub globalnie do wszystkich kontrolerów składnika Web API.

Aby zastosować filtr uwierzytelniania kontrolera, dekoracji klasy kontrolera przy użyciu atrybutu filtru. Poniższy kod ustawia `[IdentityBasicAuthentication]` Filtr klasy kontrolera, który umożliwia uwierzytelnianie podstawowe dla wszystkich akcji kontrolera.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Aby zastosować filtr do jednej akcji, dekoracji akcji z filtrem. Poniższy kod ustawia `[IdentityBasicAuthentication]` filtru na kontrolerze `Post` metody.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Aby zastosować filtr do wszystkich kontrolerów interfejsu API sieci Web, dodaj go do **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementacja filtru uwierzytelniania interfejsu API sieci Web

W składniku Web API implementuje filtry uwierzytelniania [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfejsu. Powinny one również dziedziczyć **System.Attribute**, aby można zastosować jako atrybuty.

**IAuthenticationFilter** interfejs ma dwóch metod:

- **Metody AuthenticateAsync** uwierzytelnia żądanie przez sprawdzanie poprawności poświadczeń w żądaniu, jeśli jest obecny.
- **ChallengeAsync** dodaje żądanie uwierzytelniania do odpowiedzi HTTP, jeśli to konieczne.

Te metody odpowiadają przepływ uwierzytelniania zdefiniowane w [RFC 2612](http://tools.ietf.org/html/rfc2616) i [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Klient wysyła poświadczenia w nagłówku autoryzacji. Dzieje się tak zazwyczaj po odebraniu odpowiedzi 401 (nieautoryzowane) z serwera. Jednak klient może wysyłać poświadczenia z żadnym żądaniem nie tylko po pierwsze 401.
2. Jeśli serwer nie akceptuje poświadczeń, zwraca odpowiedzi 401 (nieautoryzowanych). Odpowiedź zawiera nagłówek Www-Authenticate, który zawiera co najmniej jeden wyzwania. Każdego żądania Określa schemat uwierzytelniania rozpoznany przez serwer.

Serwer można również zwrócić 401 na podstawie żądania od użytkowników anonimowych. W rzeczywistości, który jest zwykle jak proces uwierzytelniania jest inicjowany:

1. Klient wysyła żądania od użytkowników anonimowych.
2. Serwer zwraca 401.
3. Wysyła żądanie przy użyciu poświadczeń ponownie klientów.

Obejmuje to przepływ *uwierzytelniania* i *autoryzacji* czynności.

- Uwierzytelnianie potwierdza tożsamość klienta.
- Autoryzacji określa, czy klient może uzyskiwać dostęp do określonego zasobu.

W składniku Web API filtry uwierzytelniania obsługi uwierzytelniania, ale nie autoryzacji. Autoryzacji powinno być wykonywane przez filtr autoryzacji lub wewnątrz akcji kontrolera.

Poniżej przedstawiono przepływ w potoku 2 interfejsu API sieci Web:

1. Przed wywołaniem akcji, interfejsu API sieci Web tworzy listę filtrów uwierzytelniania dla tej akcji. W tym filtrów z zakresu działania, kontrolera zakres i zasięg globalny.
2. Sieci Web interfejsu API **metody AuthenticateAsync** dla każdego filtru na liście. Każdy filtr można zweryfikować poświadczeń w żądaniu. Jeśli dowolny filtr, który pomyślnie sprawdza poprawność poświadczeń, tworzy filtr **IPrincipal** i dołącza je do żądania. W tym momencie filtru może także wyzwolić wystąpił błąd. Jeśli tak, pozostałego potoku nie działa.
3. Zakładając, że nie było błędu, żądanie przechodzi przez pozostałego potoku.
4. Ponadto interfejs API sieci Web wywołuje co filtr uwierzytelniania **ChallengeAsync** metody. Filtry można dodać żądanie do odpowiedzi, użyj tej metody, w razie potrzeby. Zazwyczaj (ale nie zawsze) które mogłyby wystąpić w odpowiedzi na 401 błąd.

Na poniższych diagramach przedstawiono dwa możliwe przypadki. W pierwszym filtr uwierzytelniania pomyślnie uwierzytelnia żądanie, filtr autoryzacji autoryzuje żądanie, i akcji kontrolera zwraca 200 (OK).

![](authentication-filters/_static/image1.png)

W drugim przykładzie filtr uwierzytelniania uwierzytelnia żądanie, ale filtr autoryzacji zwraca 401 (bez autoryzacji). W takim przypadku nie jest wywoływany akcji kontrolera. Filtr uwierzytelniania dodaje do odpowiedzi nagłówek Www-Authenticate.

![](authentication-filters/_static/image2.png)

Możliwe są inne kombinacje&mdash;na przykład, jeśli akcja kontrolera zezwala na anonimowe żądania, może być filtr uwierzytelniania, ale brak autoryzacji.

## <a name="implementing-the-authenticateasync-method"></a>Implementacja metody AuthenticateAsync — metoda

**Metody AuthenticateAsync** metody spróbuje się uwierzytelnić żądania. Podpis metody jest następujący:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**Metody AuthenticateAsync** metody należy wykonać jedną z następujących czynności:

1. Brak elementów (pusta).
2. Utwórz **IPrincipal** i ustaw ją na żądanie.
3. Ustaw wynik błędu.

Opcja (1) oznacza, że żądanie nie miał wszystkie poświadczenia, które rozumie filtru. Opcja (2) oznacza, że filtr pomyślnie uwierzytelnić żądania. Opcja (3) oznacza, że żądanie było nieprawidłowe poświadczenia (na przykład nieprawidłowe hasło), wyzwalające odpowiedzi na błąd.

Oto ogólny zarys wykonywania **metody AuthenticateAsync**.

1. Poszukaj poświadczeń w żądaniu.
2. Jeśli nie ma żadnych poświadczeń, nic nie rób i zwracać (pusta).
3. Jeśli istnieją poświadczeń, ale filtr nie może rozpoznać schematu uwierzytelniania, nic nie rób i zwracać (pusta). Inny filtr w potoku może zrozumieć schemat.
4. W przypadku poświadczenia, które obsługuje usługę filtr, spróbuj je uwierzytelniania.
5. Jeśli poświadczenia są nieprawidłowe, zwróć 401 przez ustawienie `context.ErrorResult`.
6. Jeśli poświadczenia są prawidłowe, Utwórz **IPrincipal** i ustaw `context.Principal`.

W kodzie wykonaj **metody AuthenticateAsync** metody z [uwierzytelnianie podstawowe](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) próbki. Komentarze wskazują każdego kroku. Ten kod zawiera błąd kilka typów: Nagłówek uwierzytelnienia bez poświadczeń, poświadczenia źle sformułowane i zły nazwy użytkownika i hasła.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Ustawienie wynik błędu

Jeśli poświadczenia są nieprawidłowe, należy ustawić filtr `context.ErrorResult` do **IHttpActionResult** tworzącą odpowiedzi na błąd. Aby uzyskać więcej informacji na temat **IHttpActionResult**, zobacz [wyniki akcji w sieci Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

Zawiera przykładowe uwierzytelnianie podstawowe `AuthenticationFailureResult` klasy, które jest odpowiednie dla tego celu.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementowanie ChallengeAsync

Celem **ChallengeAsync** metody jest dodanie wezwań do uwierzytelnienia do odpowiedzi, w razie potrzeby. Podpis metody jest następujący:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Metoda jest wywoływana w filtrze uwierzytelniania, co w potoku żądania.

Należy pamiętać, że **ChallengeAsync** jest nazywany *przed* odpowiedź HTTP został utworzony i być może nawet przed uruchomieniem akcji kontrolera. Gdy **ChallengeAsync** jest nazywany `context.Result` zawiera **IHttpActionResult**, później używany do tworzenia odpowiedzi HTTP. W takim przypadku **ChallengeAsync** jest wywoływana, nie wiesz nic o odpowiedzi HTTP jeszcze. **ChallengeAsync** metody należy zastąpić oryginalnej wartości elementu `context.Result` o nowe **IHttpActionResult**. To **IHttpActionResult** musi zawijać oryginalnej `context.Result`.

![](authentication-filters/_static/image3.png)

Będzie można wywołać oryginalnej **IHttpActionResult** *wewnętrzny wynik*i nowych **IHttpActionResult** *zewnętrzne wynik*. Wynik zewnętrzne, wykonaj następujące czynności:

1. Wywołania wewnętrznego wynik do tworzenia odpowiedzi HTTP.
2. Sprawdź, czy odpowiedź.
3. W razie potrzeby należy dodać żądania uwierzytelnienia na potrzeby odpowiedzi.

Poniższy przykład pochodzi z przykładowej uwierzytelnianie podstawowe. Definiuje **IHttpActionResult** dla wyniku zewnętrzne.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Właściwość przechowuje wewnętrzny **IHttpActionResult**. `Challenge` Właściwość reprezentuje nagłówek Www-uwierzytelniania. Zwróć uwagę, że **ExecuteAsync** najpierw wywołuje `InnerResult.ExecuteAsync` do tworzenia odpowiedzi HTTP, a następnie dodaje żądania, jeśli to konieczne.

Sprawdzenia kodu odpowiedzi przed dodaniem żądania. Większość schematy uwierzytelniania tylko dodać żądanie w przypadku odpowiedzi 401, jak pokazano poniżej. Jednak niektóre schematy uwierzytelniania Dodaj żądanie do odpowiedź sukcesu. Na przykład, zobacz [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Podane `AddChallengeOnUnauthorizedResult` rzeczywisty kod w klasie **ChallengeAsync** jest proste. Po prostu utworzyć wynik i dołącz je do `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Uwaga: Przykład uwierzytelnianie podstawowe abstracts tę logikę nieco, umieszczając je w metodzie rozszerzenia.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Łączenie filtry uwierzytelniania z uwierzytelnianiem na poziomie hosta

"Uwierzytelnianie na poziomie hosta" jest uwierzytelnianie przeprowadzane przez hosta (np. usługi IIS), przed framework osiągnie interfejsu API sieci Web żądania.

Często można włączyć uwierzytelnianie na poziomie hosta w pozostałej części aplikacji, ale ją wyłączyć dla kontrolerów interfejsu API sieci Web. Na przykład typowy scenariusz jest, aby włączyć uwierzytelnianie formularzy na poziomie hosta, ale używa uwierzytelniania opartego na tokenach do interfejsu API sieci Web.

Aby wyłączyć uwierzytelnianie na poziomie hosta wewnątrz potok składnika Web API, należy wywołać `config.SuppressHostPrincipal()` w konfiguracji. Powoduje to interfejs API sieci Web usunąć **IPrincipal** z żadnym żądaniem wprowadzanych potok składnika Web API. W rezultacie jego &quot;un-uwierzytelnia&quot; żądania.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Filtry zabezpieczeń interfejsu API sieci Web ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
