---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "Uwierzytelnianie i autoryzacja w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Zawiera ogólne omówienie uwierzytelniania i autoryzacji w interfejsie API sieci Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2a4b5ed8a712b061b4afdf5a3adc9378dd72b37f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Uwierzytelnianie i autoryzacja w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Po utworzeniu interfejsu API sieci web, ale teraz chcesz kontrolować dostęp do niego. W tej serii artykułów przyjrzymy kilka opcji zabezpieczanie interfejsu API sieci web przed nieautoryzowanymi użytkownikami. Ta seria obejmuje zarówno uwierzytelniania i autoryzacji.

- *Uwierzytelnianie* jest znajomość tożsamości użytkownika. Na przykład Alicja zaloguje się za pomocą swojej nazwy użytkownika i hasła, a serwer używa hasła do uwierzytelnienia Alicji.
- *Autoryzacji* decyduje, czy użytkownik może wykonać akcję. Na przykład Alicja ma uprawnienia do uzyskać zasobu, ale nie tworzenia zasobu.

Pierwszy artykuł z serii zawiera ogólne omówienie uwierzytelniania i autoryzacji w interfejsie API sieci Web ASP.NET. Innych tematach opisano typowe scenariusze uwierzytelniania dla interfejsu API sieci Web.

> [!NOTE]
> Dzięki użyciu osoby sprawdzone tej serii i podać swojej opinii: Rick Anderson, Levi Broderick, Dorrans Marcin, Dykstra Tomasz, Hongmei Ge, Matson Dominik, Roth Danielowi, Teebken Timowi.


## <a name="authentication"></a>Uwierzytelnianie

Interfejs API sieci Web zakłada, że tego uwierzytelnianie odbywa się na hoście. Dla hostingu sieci web hosta jest usług IIS do uwierzytelniania używa moduły HTTP. Można skonfigurować pod kątem używania dowolnych moduły uwierzytelniania wbudowanej w usługi IIS lub ASP.NET projekt lub napisać własny moduł HTTP, aby wykonać niestandardowe uwierzytelnianie.

Gdy host uwierzytelnia użytkownika, tworzy *główna*, która jest [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) obiekt, który reprezentuje kontekstu zabezpieczeń, w którym wykonywany jest kod. Host dołącza podmiotu zabezpieczeń bieżącego wątku przez ustawienie **Thread.CurrentPrincipal**. Podmiot zabezpieczeń zawiera skojarzone **tożsamości** obiekt, który zawiera informacje o użytkowniku. Jeśli użytkownik jest uwierzytelniony, **Identity.IsAuthenticated** zwraca **true**. W przypadku żądań anonimowych **IsAuthenticated** zwraca **false**. Aby uzyskać więcej informacji na temat podmiotów zabezpieczeń, zobacz [opartej na rolach zabezpieczeń](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Programy obsługi komunikatów HTTP do uwierzytelniania

Zamiast używać hosta na potrzeby uwierzytelniania, które można wprowadzić logika uwierzytelniania do [Obsługa komunikatów HTTP](../advanced/http-message-handlers.md). W takim przypadku program obsługi komunikatów sprawdza, czy żądanie HTTP i ustawia podmiot zabezpieczeń.

Kiedy korzystać z obsługi komunikatów do uwierzytelniania? Poniżej przedstawiono niektóre wady i zalety:

- Moduł HTTP widzi wszystkie żądania, które przechodzą przez potoku ASP.NET. Program obsługi komunikatów widoczny jest tylko żądania kierowane do interfejsu API sieci Web.
- Można określić, programy obsługi komunikatów dla trasy, który pozwala na zastosowanie schematu uwierzytelniania do określonej trasy.
- Moduły HTTP są specyficzne dla usług IIS. Programy obsługi komunikatów są pochodzącego od dowolnego hosta, dzięki mogą być używane z hostingu sieci web i własnym hostingu.
- Moduły HTTP uczestniczyć w rejestrowanie usług IIS, inspekcji i tak dalej.
- Uruchom moduły HTTP wcześniej w potoku. Jeśli obsługiwać uwierzytelnianie programu obsługi wiadomości, podmiot zabezpieczeń nie pobrać ustawiona, dopóki program obsługi jest uruchamiany. Ponadto podmiot zabezpieczeń powróci do poprzedniego podmiot zabezpieczeń podczas odpowiedzi pozostawia program obsługi komunikatów.

Ogólnie rzecz biorąc nie należy do obsługi hostingu samodzielnego HTTP jest lepszym rozwiązaniem. Jeśli potrzebujesz do obsługi hostingu samodzielnego, należy wziąć pod uwagę program obsługi komunikatów.

### <a name="setting-the-principal"></a>Ustawianie podmiotu zabezpieczeń

Jeśli aplikacja przeprowadza wszelka logika uwierzytelniania niestandardowego, należy ustawić podmiot zabezpieczeń w dwóch miejscach:

- **Thread.CurrentPrincipal**. Ta właściwość jest standardowym sposobem, aby ustawić podmiot wątku w .NET.
- **HttpContext.Current.User**. Ta właściwość jest specyficzne dla platformy ASP.NET.

Poniższy kod przedstawia sposób ustawić podmiot:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Hostingu sieci web, należy ustawić podmiot zabezpieczeń w obu miejscach; w przeciwnym razie kontekstu zabezpieczeń może stać się niespójna. Dla hostingu samodzielnego, jednak **właściwość HttpContext.Current** ma wartość null. Aby upewnić się, kod jest niezależny od hosta, w związku z tym Wyszukaj null przed przypisaniem do **właściwość HttpContext.Current**, jak pokazano.

## <a name="authorization"></a>Autoryzacja

Autoryzacja odbywa się później w potoku, zbliżonej do kontrolera. Gwarantowaną bardziej szczegółowego wyborów podczas udzielania dostępu do zasobów.

- *Filtry autoryzacji* uruchamiane przed akcji kontrolera. Jeśli żądanie nie jest autoryzowane, filtr zwraca odpowiedź o błędzie, a nie wywołaniu akcji.
- W ramach akcji kontrolera, możesz uzyskać bieżący podmiot zabezpieczeń z **ApiController.User** właściwości. Na przykład można filtrować listę zasobów na podstawie nazwy użytkownika zwracanie tylko tych zasobów, które należą do tego użytkownika.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Przy użyciu [autoryzować] atrybutu

Interfejs API sieci Web udostępnia filtr autoryzacji wbudowanych [klasy AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ten filtr sprawdza, czy użytkownik jest uwierzytelniony. W przeciwnym razie zwraca kod stanu HTTP 401 (bez autoryzacji), bez wywoływania akcji.

Można zastosować filtr globalny, na poziomie kontrolera lub na poziomie akcji inidivual.

**Globalny**: Aby ograniczyć dostęp dla każdego kontrolera interfejsu API sieci Web, Dodaj **klasy AuthorizeAttribute** filtr do listy filtrów globalnych:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Kontroler**: Aby ograniczyć dostęp dla określonego kontrolera, Dodaj filtr jako atrybut do kontrolera:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Akcja**: Aby ograniczyć dostęp dla określonych akcji, Dodaj atrybut do metody akcji:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternatywnie można ograniczyć kontrolera i następnie zezwolić na dostęp anonimowy do określonych akcji za pomocą `[AllowAnonymous]` atrybutu. W poniższym przykładzie `Post` metody jest ograniczona, ale `Get` metody zezwala na dostęp anonimowy.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

W poprzednich przykładach filtr zezwala każdemu użytkownikowi uwierzytelniony dostęp ograniczony metod; do tylko użytkownicy anonimowi są przechowywane. Można również ograniczyć dostęp dla określonych użytkowników lub dla użytkowników w określonych ról:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **Klasy AuthorizeAttribute** filtru dla kontrolerów interfejsu API sieci Web znajduje się w **System.Web.Http** przestrzeni nazw. Ma podobnych filtru dla kontrolerów MVC w **System.Web.Mvc** przestrzeni nazw, które nie są zgodne z kontrolerów interfejsu API sieci Web.


### <a name="custom-authorization-filters"></a>Filtry autoryzacji niestandardowej

Aby zapisać filtr autoryzacji niestandardowej, pochodzi z jednego z następujących typów:

- **Klasy AuthorizeAttribute**. Rozszerzenia tej klasy do wykonywania logiki autoryzacji na podstawie bieżącego użytkownika i ról użytkownika.
- **AuthorizationFilterAttribute**. Rozszerzenia tej klasy do wykonywania logiki synchroniczne autoryzacji, które nie jest zawsze oparty na bieżący użytkownik lub rola.
- **IAuthorizationFilter**. Implementuje ten interfejs do wykonywania asynchronicznych autoryzacji logiki; na przykład, jeśli logiki autoryzacji wywołań asynchronicznych We/Wy lub sieci. (Jeśli logika autoryzacji jest związany z Procesora, jest łatwiejsze pochodzi od **AuthorizationFilterAttribute**, ponieważ wówczas nie trzeba zapisać metody asynchronicznej.)

Na poniższym diagramie przedstawiono hierarchii klas dla **klasy AuthorizeAttribute** klasy.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autoryzacja w akcji kontrolera

W niektórych przypadkach mogą zezwalać żądanie, aby kontynuować, ale należy zmienić to zachowanie, oparte na podmiot zabezpieczeń. Na przykład można zwrócić informacje może zmieniać się w zależności od roli użytkownika. W metodzie kontrolera, możesz uzyskać bieżące zasady z **ApiController.User** właściwości.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
