---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: "Część 4: Dodawanie widoku Admin | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2960eee37201655a9e4632bf0196ba18a0e2e82a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-4-adding-an-admin-view"></a>Część 4: Dodawanie widoku administratora
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Dodaj widok administratora

Firma Microsoft teraz będzie włączenie na komputerach klienckich i dodać stronę, która może wykorzystywać dane z kontrolera administratora. Strona Użytkownicy mogą tworzyć, edytować lub usunąć produkty, wysyłając żądania AJAX do kontrolera.

W Eksploratorze rozwiązań rozwiń folder kontrolery, a następnie otwórz plik o nazwie HomeController.cs. Ten plik zawiera kontroler MVC. Dodaj metodę o nazwie `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** metoda tworzy identyfikator URI do interfejsu API sieci web i przechowujemy to w zbiorze widok do użycia później.

Następnie umieść kursor tekstu w `Admin` metody akcji, a następnie kliknij prawym przyciskiem myszy i wybierz **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

W **Dodaj widok** okna dialogowego, nazwę widoku "Admin". Zaznacz pole wyboru **utworzyć widok jednoznacznie**. W obszarze **klasy modelu**, wybierz "Produktu (ProductStore.Models)". Inne opcje pozostaw wartości domyślne.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Kliknięcie przycisku **Dodaj** dodaje plik o nazwie Admin.cshtml widoków/głównej. Otwórz ten plik i Dodaj poniższy kod HTML. Kod HTML definiuje strukturę strony, ale żadne funkcje przewodowej się jeszcze.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Utwórz łącze do strony administratora

W Eksploratorze rozwiązań rozwiń folder widoków, a następnie rozwiń folder udostępniony. Otwórz plik o nazwie \_Layout.cshtml. Zlokalizuj **ul** elementu o identyfikatorze = "menu", a łącze akcji dla widoku administratora:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> W projekcie próbki kilka innych bardzo drobny zmiany wprowadzone, takich jak zastępując ciąg "Tutaj znak logo". Te nie mają wpływu na funkcjonalność aplikacji. Można pobrać projektu i porównać pliki.


Uruchom aplikację, a następnie kliknij łącze "Admin", który pojawia się w górnej części strony głównej. Strona Administrator powinien wyglądać następująco:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Prawej strony, strony nic nie robi. W następnej sekcji użyjemy Knockout.js do tworzenia dynamicznego interfejsu użytkownika.

## <a name="add-authorization"></a>Dodaj autoryzacji

Strony administratora jest obecnie dostępny dla wszystkich użytkowników w witrynie. Zmieńmy tutaj, aby ograniczyć uprawnienia do administratorów.

Rozpocznij, dodając rolę "Administrator" i użytkownika administrator. W Eksploratorze rozwiązań rozwiń folder filtry i Otwórz plik o nazwie InitializeSimpleMembershipAttribute.cs. Zlokalizuj `SimpleMembershipInitializer` konstruktora. Po wywołaniu **WebSecurity.InitializeDatabaseConnection**, Dodaj następujący kod:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Jest to quick-and-dirty sposób Dodaj rolę "Administrator", a następnie utworzenie roli użytkownika.

W Eksploratorze rozwiązań rozwiń folder kontrolery, a następnie otwórz plik HomeController.cs. Dodaj **autoryzacji** atrybutu `Admin` metody.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Otwórz plik AdminController.cs i Dodaj **autoryzacji** atrybutu do całej `AdminController` klasy.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC i interfejsu API sieci Web, zdefiniuj zarówno **autoryzacji** atrybutów w różnych przestrzeniach nazw. Używa MVC **System.Web.Mvc.AuthorizeAttribute**, podczas gdy używa interfejsu API sieci Web **System.Web.Http.AuthorizeAttribute**.


Tylko administratorzy mogą teraz wyświetlać strony administratora. Ponadto po wysłaniu żądania HTTP do kontrolera administratora żądanie musi zawierać pliku cookie uwierzytelniania. Jeśli nie, serwer wysyła komunikat odpowiedzi HTTP 401 (bez autoryzacji). Można to zobaczyć w narzędziu Fiddler, wysyłając żądanie GET `http://localhost:*port*/api/admin`.

>[!div class="step-by-step"]
[Poprzednie](using-web-api-with-entity-framework-part-3.md)
[dalej](using-web-api-with-entity-framework-part-5.md)
