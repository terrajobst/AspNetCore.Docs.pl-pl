---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Część 4: Dodawanie widoku administratora | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: ff46c7cbdf13a048e57ad88ab39077357e35c5b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753999"
---
<a name="part-4-adding-an-admin-view"></a>Część 4: Dodawanie widoku administratora
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Dodawanie widoku administratora

Teraz utworzymy sięgają po stronie klienta i dodamy strony, którą może wykorzystać dane z kontrolera administratora. Strona Użytkownicy mogą tworzyć, edytować lub usunąć produktów, przez wysyłanie żądań AJAX do kontrolera.

W Eksploratorze rozwiązań rozwiń folder kontrolerów, a następnie otwórz plik o nazwie HomeController.cs. Ten plik zawiera kontroler MVC. Dodaj metodę o nazwie `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** metoda tworzy identyfikator URI w internetowym interfejsie API i przechowujemy to w zbiorze widok na później.

Następnie umieść kursor tekstu w obrębie `Admin` metody akcji, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

W **Dodaj widok** okno dialogowe, nazwę widoku, "Admin". Zaznacz pole wyboru przy opcji **utworzyć widok silnie typizowane**. W obszarze **klasy modelu**, wybierz pozycję "Product (ProductStore.Models)". Pozostaw wartości domyślne innych opcji.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Klikając **Dodaj** dodaje plik o nazwie Admin.cshtml widoków/głównej. Otwórz ten plik i Dodaj poniższy kod HTML. Kod HTML definiuje strukturę strony, ale funkcjonalność nie jest jeszcze gotowe.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Utwórz łącze do strony administratora

W Eksploratorze rozwiązań rozwiń folder widoki, a następnie rozwiń folder udostępniony. Otwórz plik o nazwie \_Layout.cshtml. Znajdź **ul** element o identyfikatorze = "menu", a łącze akcji dla widoku administratora:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> W przykładowym projekcie kilka innych kosmetycznych zmiany wprowadzone, takich jak zastępując ciąg "Twoje logo". Nie mają one wpływu na funkcjonalność aplikacji. Można pobrać projektu i porównywanie plików.


Uruchom aplikację, a następnie kliknij link "Admin", który pojawia się w górnej części strony głównej. Strona administratora powinny wyglądać następująco:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Po prawej stronie teraz strony nie wprowadza żadnych zmian. W następnej sekcji użyjemy struktura Knockout.js do tworzenia dynamicznego interfejsu użytkownika.

## <a name="add-authorization"></a>Dodaj autoryzację

Strona administratora jest obecnie dostępna dla każdego, kto w witrynie. Zmieńmy ten element, aby ograniczyć uprawnienia do administratorów.

Rozpocznij, dodając rolę "Administrator" i użytkownika administrator. W Eksploratorze rozwiązań rozwiń folder filtry, a następnie otwórz plik o nazwie InitializeSimpleMembershipAttribute.cs. Znajdź `SimpleMembershipInitializer` konstruktora. Po wywołaniu **WebSecurity.InitializeDatabaseConnection**, Dodaj następujący kod:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Jest quick-and-dirty sposób, aby dodać rolę "Administrator" i Utwórz użytkownika dla roli.

W Eksploratorze rozwiązań rozwiń folder kontrolerów, a następnie otwórz plik HomeController.cs. Dodaj **Autoryzuj** atrybutu `Admin` metody.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Otwórz plik AdminController.cs i Dodaj **Autoryzuj** atrybutu do całej `AdminController` klasy.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC i internetowy interfejs API zarówno zdefiniować **Autoryzuj** atrybutów w różnych obszarach nazw. Używa MVC **System.Web.Mvc.AuthorizeAttribute**, podczas gdy interfejs API sieci Web używa **System.Web.Http.AuthorizeAttribute**.


Tylko administratorzy mogą teraz wyświetlać strony administratora. Ponadto Jeśli wyślesz żądanie HTTP do kontrolera administratora żądanie musi zawierać plik cookie uwierzytelniania. Jeśli nie, serwer wysyła odpowiedź HTTP 401 (bez autoryzacji). Można to zobaczyć w narzędziu Fiddler, wysyłając żądanie GET w celu `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-3.md)
> [dalej](using-web-api-with-entity-framework-part-5.md)
