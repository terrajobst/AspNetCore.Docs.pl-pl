---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Uwierzytelnianie i autoryzacja w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: Zawiera ogólne omówienie uwierzytelniania i autoryzacji w interfejsie API sieci Web platformy ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a78606a74b2149e68e3b01f4fe204f4a13edf4b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756443"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Uwierzytelnianie i autoryzacja w składniku ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Internetowy interfejs API został utworzony, ale teraz chcesz kontrolować dostęp do niego. W tej serii artykułów przyjrzymy niektóre opcje zabezpieczania internetowego interfejsu API przed nieautoryzowanymi użytkownikami. W tej serii opisano zarówno uwierzytelniania i autoryzacji.

- *Uwierzytelnianie* jest wiedza, tożsamość użytkownika. Na przykład Alicja zaloguje się za pomocą swojej nazwy użytkownika i hasła i hasło używane do uwierzytelniania Alicji.
- *Autoryzacja* decyduje, czy użytkownik może wykonać akcję. Na przykład gdy Alicja ma uprawnienia do pobieranie zasobu, ale nie tworzenie zasobu.

Pierwszy artykuł z serii zapewnia ogólne omówienie uwierzytelniania i autoryzacji w interfejsie API sieci Web platformy ASP.NET. Innych tematach opisano typowe scenariusze uwierzytelniania dla interfejsu API sieci Web.

> [!NOTE]
> Dzięki gotowej do osoby w tej serii przeglądu i udostępniane opinii: Rick Anderson Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Uwierzytelnianie

Interfejsu API sieci Web przyjęto założenie, że uwierzytelnianie odbywa się na hoście. Dla hostingu w sieci web hosta jest usług IIS, które są używane moduły HTTP do uwierzytelniania. Można skonfigurować projekt do żadnych modułów uwierzytelniania wbudowanej w program IIS lub ASP.NET lub napisać własny moduł HTTP, aby wykonać niestandardowe uwierzytelnianie.

Gdy host uwierzytelnia użytkownika, tworzy *jednostki*, czyli [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) obiekt, który reprezentuje kontekstu zabezpieczeń, w którym kod działa. Host dołącza podmiotu zabezpieczeń bieżącego wątku, ustawiając **SE vlastnost Thread.CurrentPrincipal**. Podmiot zabezpieczeń zawiera skojarzoną **tożsamości** obiekt, który zawiera informacje o użytkowniku. Jeśli użytkownik jest uwierzytelniony, **Identity.IsAuthenticated** właściwość zwraca **true**. W przypadku żądań anonimowych **właściwości** zwraca **false**. Aby uzyskać więcej informacji na temat jednostek, zobacz [opartej na rolach zabezpieczeń](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Programy obsługi komunikatów HTTP na potrzeby uwierzytelniania

Zamiast używania hosta na potrzeby uwierzytelniania, mogą umieścić logiki uwierzytelniania do [program obsługi komunikatów HTTP](../advanced/http-message-handlers.md). W takim przypadku program obsługi komunikatów sprawdza żądania HTTP i ustawia podmiot zabezpieczeń.

Kiedy należy używać obsługi komunikatów do uwierzytelniania? Poniżej przedstawiono niektóre wady i zalety:

- Moduł HTTP widzi wszystkie żądania, które przechodzą przez potoku ASP.NET. Program obsługi komunikatów widoczny jest tylko żądania kierowane do interfejsu API sieci Web.
- Można ustawić programy obsługi komunikatów dla trasy, które można stosować schematu uwierzytelniania do określonej trasy.
- Moduły HTTP są specyficzne dla usług IIS. Programy obsługi komunikatów są pochodzącego od dowolnego hosta, więc można ich używać z hostingu w sieci web i własnym hostingu.
- Moduły HTTP uczestniczyć w rejestrowania w usługach IIS, inspekcji i tak dalej.
- Uruchom moduły HTTP wcześniej w potoku. Jeśli możesz obsługiwać uwierzytelnianie programu obsługi wiadomości, podmiot zabezpieczeń nie uzyskać ustawiony, dopóki program obsługi jest uruchamiany. Ponadto główny powróci do poprzedniej nazwy głównej gdy odpowiedź pozostawia program obsługi komunikatów.

Ogólnie rzecz biorąc Jeśli nie potrzebujesz do obsługi hostingu samodzielnego modułu HTTP jest lepszym rozwiązaniem. Jeśli potrzebujesz do obsługi hostingu samodzielnego, należy wziąć pod uwagę program obsługi komunikatów.

### <a name="setting-the-principal"></a>Ustawienie podmiotu zabezpieczeń

Jeśli Twoja aplikacja działa wszelka logika uwierzytelniania niestandardowego, musisz ustawić podmiot zabezpieczeń w dwóch miejscach:

- **SE vlastnost Thread.CurrentPrincipal**. Ta właściwość jest standardowym sposobem, aby ustawić podmiot wątku na platformie .NET.
- **HttpContext.Current.User**. Ta właściwość jest specyficzne dla platformy ASP.NET.

Poniższy kod przedstawia sposób ustawić podmiot:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Hosting sieci web należy ustawić podmiot zabezpieczeń w obu miejscach; w przeciwnym razie kontekstu zabezpieczeń może stać się niespójna. Dla hostingu samodzielnego, jednak **HttpContext.Current** ma wartość null. Aby upewnić się, Twój kod jest niezależny od hosta, w związku z tym, sprawdzaj wartość null przed przypisaniem do **HttpContext.Current**, jak pokazano.

## <a name="authorization"></a>Autoryzacja

Autoryzacja odbywa się później w potoku, bliżej kontrolera. Który umożliwia podejmowanie bardziej szczegółowego wyborów, gdy udzielisz jej dostępu do zasobów.

- *Filtry autoryzacji* uruchamiane przed akcji kontrolera. Jeśli żądanie nie jest autoryzowane, filtr zwraca odpowiedź na błąd i akcja nie jest wywoływany.
- W ramach akcji kontrolera, można uzyskać bieżący podmiot zabezpieczeń z **ApiController.User** właściwości. Na przykład można filtrować listę zasobów, w oparciu o nazwę użytkownika, zwracanie tylko tych zasobów, które należą do tego użytkownika.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Przy użyciu [autoryzować] atrybutu

Interfejs API sieci Web zawiera filtr autoryzacji wbudowanych [klasy AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ten filtr sprawdza, czy użytkownik jest uwierzytelniony. W przeciwnym razie zwraca kod stanu HTTP 401 (bez autoryzacji) bez wywoływania akcji.

Można zastosować filtr, na całym świecie, na poziomie kontrolera lub na poziomie poszczególnych działań.

**Globalnie**: Aby ograniczyć dostęp dla każdego kontrolera interfejsu API sieci Web, należy dodać **klasy AuthorizeAttribute** filtr do listy filtrów globalnych:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Kontroler**: Aby ograniczyć dostęp dla określonego kontrolera, Dodaj filtr jako atrybut do kontrolera:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Akcja**: Aby ograniczyć dostęp dla określonych akcji, Dodaj atrybut do metody akcji:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternatywnie, można ograniczyć kontrolera i następnie zezwolić na dostęp anonimowy do określonych akcji za pomocą `[AllowAnonymous]` atrybutu. W poniższym przykładzie `Post` metodą jest ograniczona, ale `Get` metoda zezwala na dostęp anonimowy.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

W poprzednich przykładach filtr zezwala każdemu uwierzytelnionemu użytkownikowi na dostęp do metod ograniczone; tylko użytkownicy anonimowi są przechowywane w. Można również ograniczyć dostęp do określonych użytkowników lub dla użytkowników w określonych ról:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **Klasy AuthorizeAttribute** filtr dla kontrolerów internetowych interfejsów API znajduje się w **System.Web.Http** przestrzeni nazw. Ma podobnych filtru dla kontrolerów MVC w **System.Web.Mvc** przestrzeń nazw, która nie jest zgodny z kontrolerów internetowych interfejsów API.


### <a name="custom-authorization-filters"></a>Filtry autoryzacji niestandardowej

Aby zapisać filtr autoryzacja niestandardowa, pochodzi z jednego z następujących typów:

- **Klasy AuthorizeAttribute**. Rozszerzenia tej klasy w celu wykonania logiki autoryzacji na podstawie bieżącego użytkownika i ról użytkownika.
- **AuthorizationFilterAttribute**. Rozszerzenia tej klasy w celu wykonania logiki synchroniczne autoryzacji, który nie zawsze jest oparty na bieżący użytkownik lub rola.
- **IAuthorizationFilter**. Implementować ten interfejs w celu wykonania logiki asynchronicznego autoryzacji; na przykład, jeśli logika autoryzacji sprawia, że wywołania asynchroniczne operacje We/Wy lub sieci. (Jeśli logika autoryzacji jest zależne od Procesora CPU, jest łatwiejsze w dziedziczyć **AuthorizationFilterAttribute**, ponieważ wówczas nie trzeba napisać metodę asynchroniczną.)

Poniższy diagram przedstawia hierarchii klas dla **klasy AuthorizeAttribute** klasy.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autoryzacja wewnątrz akcji kontrolera

W niektórych przypadkach może być Zezwalaj na żądanie, aby kontynuować, ale zmienia zachowanie w zależności od podmiotu zabezpieczeń. Na przykład można zwrócić informacje mogą ulec zmianie w zależności od roli użytkownika. Wewnątrz metody kontrolera, można uzyskać bieżący podmiot zabezpieczeń z **ApiController.User** właściwości.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
