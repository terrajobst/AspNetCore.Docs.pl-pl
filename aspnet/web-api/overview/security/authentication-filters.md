---
uid: web-api/overview/security/authentication-filters
title: Filtry uwierzytelniania we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Filtr uwierzytelniania jest składnikiem, który uwierzytelnia żądanie HTTP. Składnika Web API 2 i MVC 5 obsługuje zarówno filtry uwierzytelniania, ale mogą się nieznacznie różnić...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: be2dcb246597f90ed7f00b2cf647b92e44aa254c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385680"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtry uwierzytelniania we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Filtr uwierzytelniania jest składnikiem, który uwierzytelnia żądanie HTTP. Składnika Web API 2 i MVC 5 obsługuje zarówno filtry uwierzytelniania, ale różnią się nieznacznie, przede wszystkim konwencje nazewnictwa dla interfejsu filtru. W tym temacie opisano filtry uwierzytelniania interfejsu API sieci Web.


Filtry uwierzytelniania umożliwiają ustawienie schematu uwierzytelniania dla poszczególnych kontrolerów i akcji. Dzięki temu aplikacja może obsługiwać różnych mechanizmów uwierzytelniania dla różnych zasobów HTTP.

W tym artykule zaprezentuję, kodu z [uwierzytelnianie podstawowe](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) próbki na [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). Przykładowy kod zawiera filtr uwierzytelniania zawierającej implementację schematu podstawowego uwierzytelniania dostępu do protokołu HTTP (RFC 2617). Filtr jest zaimplementowana w klasie o nazwie `IdentityBasicAuthenticationAttribute`. Czy mogę nie będzie zawierać cały kod z przykładu, tylko te części, które pokazują, jak napisać filtr uwierzytelniania.

## <a name="setting-an-authentication-filter"></a>Ustawienie filtru uwierzytelniania

Podobnie jak inne filtry filtry uwierzytelniania może być kontrolerem zastosowane, -action lub globalnie do wszystkich kontrolerów internetowych interfejsów API.

Aby zastosować filtr uwierzytelniania z kontrolerem, dekoracji klasy kontrolera przy użyciu atrybutu filtru. Poniższy kod ustawia `[IdentityBasicAuthentication]` Filtr klasy kontrolera, co umożliwia uwierzytelnianie podstawowe dla wszystkich akcji kontrolera.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Aby zastosować filtr, aby jedna akcja, dekoracji akcji z filtrem. Poniższy kod ustawia `[IdentityBasicAuthentication]` filtru na kontrolerze `Post` metody.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Aby zastosować filtr do wszystkich kontrolerów składnika Web API, dodaj go do **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementowanie filtr uwierzytelniania interfejsu API sieci Web

W interfejsie API sieci Web, implementuje filtry uwierzytelniania [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfejsu. Powinny one również dziedziczyć **klasy System.Attribute**, aby zostać zastosowana jako atrybuty.

**IAuthenticationFilter** interfejs ma dwie metody:

- **Metody AuthenticateAsync** uwierzytelnia żądanie przez sprawdzanie poprawności poświadczeń w żądaniu, jeśli jest obecny.
- **ChallengeAsync** dodaje żądanie uwierzytelniania dla odpowiedzi HTTP, jeśli to konieczne.

Te metody odpowiadają przepływ uwierzytelniania zdefiniowane w [RFC 2612](http://tools.ietf.org/html/rfc2616) i [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Klient wysyła poświadczenia w nagłówku autoryzacji. Zwykle dzieje się tak, po odebraniu odpowiedzi 401 (nieautoryzowane) z serwera. Jednak klient może wysłać poświadczenia z żadnym żądaniem nie tylko po 401.
2. Jeśli serwer nie akceptuje poświadczeń, zwraca odpowiedzi 401 (nieautoryzowany). Odpowiedź zawiera nagłówka Www-Authenticate, który zawiera co najmniej jeden wyzwania. Każdego żądania określa schematu uwierzytelniania rozpoznawane przez serwer.

Serwer może również zwracać 401 z anonimowego żądania. W rzeczywistości, który jest zazwyczaj jak jest inicjowane z procesem uwierzytelniania:

1. Klient wysyła żądania od użytkowników anonimowych.
2. Serwer zwraca 401.
3. Wysyła żądanie przy użyciu poświadczeń ponownie klientów.

Ten przepływ obejmuje zarówno *uwierzytelniania* i *autoryzacji* kroki.

- Uwierzytelnianie potwierdza tożsamość klienta.
- Autoryzacja Określa, czy klient może uzyskiwać dostęp do określonego zasobu.

W interfejsie API sieci Web filtry uwierzytelniania obsługi uwierzytelniania, ale nie autoryzacji. Autoryzacja powinna być podejmowana przez filtr autoryzacji lub wewnątrz akcji kontrolera.

Poniżej przedstawiono przepływ w potoku Web API 2:

1. Przed wywołaniem akcję, internetowy interfejs API tworzy się lista filtrów uwierzytelniania dla tej akcji. W tym filtry z zakresu akcji, zakresem kontrolera i zakresu globalnego.
2. Wywołania interfejsu API w sieci Web **metody AuthenticateAsync** dla każdego filtru na liście. Każdy filtr można zweryfikować poświadczeń w żądaniu. Jeśli jakikolwiek filtr, który pomyślnie sprawdza poprawność poświadczeń, filtr tworzy **IPrincipal** i dołącza je do żądania. Filtr może także wyzwolić błąd w tym momencie. Jeśli tak, pozostałego potoku nie działa.
3. Przy założeniu, że nie ma błędów, żądanie przepływów przy użyciu interfejsu rest z potoku.
4. Na koniec interfejsu API sieci Web wywołuje każdy filtr uwierzytelniania **ChallengeAsync** metody. Filtry ta metoda umożliwia dodawanie wezwanie do odpowiedzi, jeśli to konieczne. Zazwyczaj (ale nie zawsze), będzie możliwe w odpowiedzi na błąd 401 — dostęp.

Na poniższych diagramach przedstawiono dwa możliwe przypadki. W pierwszym filtr uwierzytelniania pomyślnie uwierzytelnia żądanie filtru autoryzacji autoryzuje żądanie i akcji kontrolera zwraca 200 (OK).

![](authentication-filters/_static/image1.png)

W drugim przykładzie filtr uwierzytelniania uwierzytelnia żądanie, ale filtr autoryzacji zwraca wartość 401 (bez autoryzacji). W tym przypadku nie jest wywoływany akcji kontrolera. Filtr uwierzytelniania dodaje do odpowiedzi nagłówek Www-Authenticate.

![](authentication-filters/_static/image2.png)

Możliwe są inne kombinacje&mdash;na przykład, jeśli akcja kontrolera zezwala na anonimowe żądania, Niewykluczone, że filtr uwierzytelniania, ale brak autoryzacji.

## <a name="implementing-the-authenticateasync-method"></a>Implementacja metody metody AuthenticateAsync

**Metody AuthenticateAsync** metoda próbuje się uwierzytelnić żądania. W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**Metody AuthenticateAsync** metody należy wykonać jedną z następujących czynności:

1. Brak elementów (pusta).
2. Tworzenie **IPrincipal** i ustaw ją na żądanie.
3. Ustaw wynik błędu.

Opcja (1) oznacza, że żądanie nie ma żadnych poświadczeń, które rozumie filtru. Opcja (2) oznacza, że filtr pomyślnym uwierzytelnieniu żądanie. Opcja (3) oznacza, że żądanie było nieprawidłowe poświadczenia (np. nieprawidłowe hasło), powoduje wyzwolenie odpowiedź o błędzie.

Poniżej przedstawiono ogólny zarys implementowania **metody AuthenticateAsync**.

1. Zwróć uwagę na poświadczenia w żądaniu.
2. Jeśli nie istnieją żadne poświadczenia nie, nic nie rób i powrócić (pusta).
3. Jeśli istnieją poświadczenia, ale filtru nie może rozpoznać schemat uwierzytelniania, nic nie rób i zwracać (pusta). Inny filtr w potoku może zrozumieć schemat.
4. W przypadku poświadczeń, które rozumie filtr, spróbuj je uwierzytelnić.
5. Jeśli poświadczenia są nieprawidłowe, zwraca 401, ustawiając `context.ErrorResult`.
6. Jeśli poświadczenia są prawidłowe, Utwórz **IPrincipal** i ustaw `context.Principal`.

Poniżej przedstawiono kod **metody AuthenticateAsync** metody z [uwierzytelnianie podstawowe](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) próbki. Komentarze wskazują każdego kroku. Ten kod zawiera kilka rodzajów błędów: nagłówek autoryzacji bez poświadczeń, poświadczenia źle sformułowane i nieprawidłowo podawali nazwę użytkownika/hasło.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Ustawienie wynik błędu

Jeśli poświadczenia są nieprawidłowe, musisz ustawić filtr `context.ErrorResult` do **IHttpActionResult** tworząca odpowiedź o błędzie. Aby uzyskać więcej informacji na temat **IHttpActionResult**, zobacz [wyniki akcji w sieci Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

Przykład uwierzytelniania Basic obejmuje `AuthenticationFailureResult` klasę, która jest odpowiednia do tego celu.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementowanie ChallengeAsync

Celem **ChallengeAsync** metody jest dodanie wezwań do uwierzytelnienia do odpowiedzi, jeśli to konieczne. W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Metoda jest wywoływana w każdej filtr uwierzytelniania w Potok żądań.

Należy pamiętać, że **ChallengeAsync** nosi nazwę *przed* odpowiedź HTTP jest utworzony i prawdopodobnie nawet przed uruchomieniem akcji kontrolera. Gdy **ChallengeAsync** jest wywoływana, `context.Result` zawiera **IHttpActionResult**, który jest używany później utworzyć odpowiedź HTTP. W takim przypadku **ChallengeAsync** jest wywoływana, nie wiesz nic o odpowiedzi HTTP jeszcze. **ChallengeAsync** metody należy zastąpić oryginalnej wartości elementu `context.Result` dzięki nowemu **IHttpActionResult**. To **IHttpActionResult** należy opakować oryginalny `context.Result`.

![](authentication-filters/_static/image3.png)

Będzie zadzwonić, oryginalnym **IHttpActionResult** *wewnętrzne wyników*, a nowe **IHttpActionResult** *zewnętrzne wynik*. Wynik zewnętrzne, wykonaj następujące czynności:

1. Wywołaj wewnętrzne wyników do tworzenia odpowiedzi HTTP.
2. Sprawdź odpowiedzi.
3. W razie potrzeby należy dodać wezwanie do uwierzytelnienia w odpowiedzi.

Poniższy przykład jest pobierana z próbki uwierzytelnianie podstawowe. Definiuje on **IHttpActionResult** zewnętrzne wynik.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Właściwość przechowuje wewnętrzny **IHttpActionResult**. `Challenge` Właściwość reprezentuje nagłówka Www-uwierzytelniania. Należy zauważyć, że **ExecuteAsync** najpierw wywołuje `InnerResult.ExecuteAsync` do tworzenia odpowiedzi HTTP, a następnie dodaje żądania, jeśli to konieczne.

Sprawdź kod odpowiedzi przed dodaniem wyzwanie. Większość schematów uwierzytelniania tylko dodać wyzwanie w przypadku odpowiedzi 401, jak pokazano poniżej. Jednak niektóre schematy uwierzytelniania dodać wyzwanie odpowiedź sukcesu. Na przykład zobacz [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Biorąc pod uwagę `AddChallengeOnUnauthorizedResult` rzeczywisty kod w klasie **ChallengeAsync** jest proste. Po prostu utworzyć wynik i dołącz je do `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Uwaga: Przykładowych uwierzytelnianie podstawowe przenosi tę logikę nieco, umieszczając go w metodzie rozszerzenia.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Łącząc filtry uwierzytelniania z uwierzytelnieniem na poziomie hosta

"Uwierzytelnianie na poziomie hosta" jest uwierzytelnianie przeprowadzane przez hosta (np. usługi IIS), zanim żądanie framework przypada interfejsu API sieci Web.

Często warto włączyć uwierzytelnianie na poziomie hosta w pozostałej części aplikacji, ale ją wyłączyć dla kontrolerów Web API. Na przykład typowym scenariuszem jest włączyć uwierzytelnianie formularzy na poziomie hosta, ale używanie uwierzytelniania opartego na tokenach dla interfejsu API sieci Web.

Aby wyłączyć uwierzytelnianie na poziomie hosta wewnątrz potok składnika Web API, należy wywołać `config.SuppressHostPrincipal()` w konfiguracji. Powoduje to, że internetowy interfejs API usunąć **IPrincipal** z każde żądanie, które wprowadza potok składnika Web API. Skutecznie, jego &quot;un-uwierzytelnia&quot; żądania.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Filtry zabezpieczeń interfejsu API sieci Web platformy ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
