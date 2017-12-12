---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 9 przedstawiono sposób dodawania uwierzytelniania i autoryzacji, aby zabezpieczyć naszej aplikacji NerdDinner, dzięki czemu użytkownicy będą musieli zarejestrować i zaloguj się do witryny, aby utworzyć..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 9 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 9 przedstawiono sposób dodawania uwierzytelniania i autoryzacji, aby zabezpieczyć naszej aplikacji NerdDinner, dzięki czemu użytkownicy będą musieli zarejestrować i zaloguj się do witryny, aby utworzyć nowy kolacji i tylko użytkownik, który jest hostem obiad można edytować go później.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner krok 9: Uwierzytelnianie i autoryzacja

W tej chwili naszych NerdDinner aplikacja udziela każdy tej witrynie możliwość tworzenia i edytowania szczegółów dowolnego obiad. Umożliwia to zmienić, aby użytkownicy musieli zarejestrować i zaloguj się do witryny, aby utworzyć nowy kolacji i Dodaj ograniczenie, tak aby tylko użytkownik, który jest hostem obiad można go później edytować.

Aby włączyć tę użyjemy do naszej aplikacji bezpiecznego uwierzytelniania i autoryzacji.

### <a name="understanding-authentication-and-authorization"></a>Opis uwierzytelniania i autoryzacji

*Uwierzytelnianie* to proces identyfikacji i weryfikacji tożsamości klienta podczas uzyskiwania dostępu do aplikacji. Po prostu umieść, jest dotyczące identyfikowania "będącego użytkownik końcowy po odwiedzeniu witryny sieci Web". Program ASP.NET obsługuje wiele sposobów uwierzytelniania użytkowników w przeglądarce. Dla aplikacji sieci web internetowych najbardziej typowe metody uwierzytelniania używane jest nazywany "Uwierzytelnianie formularzy". Uwierzytelnianie formularzy umożliwia deweloperom tworzenie formularza HTML logowania w aplikacjach oraz późniejsze weryfikowanie nazwy użytkownika/hasła przez użytkownika końcowego składa się z bazą danych lub innym magazynie poświadczeń hasła. Jeśli kombinacja nazwy użytkownika i hasła jest prawidłowa, deweloper może następnie poprosić ASP.NET do wystawiania zaszyfrowanego pliku cookie HTTP do identyfikowania użytkownika dla przyszłych żądań. Wyślemy wiadomość przy użyciu uwierzytelniania formularzy z naszą aplikacją NerdDinner.

*Autoryzacji* jest proces określania, czy uwierzytelniony użytkownik ma uprawnienia dostępu do określonego adresu URL/zasobu lub wykonania akcji. Na przykład w naszej aplikacji NerdDinner chcemy, do autoryzacji, czy tylko użytkowników, którzy są zalogowani do dostęp */kolacji/Utwórz* adresu URL i Utwórz nowe kolacji. Chcemy też dodać logikę autoryzacji, dzięki czemu tylko użytkownik, który jest hostem obiad można edytować go — i odmawiania dostępu edycji do innych użytkowników.

### <a name="forms-authentication-and-the-accountcontroller"></a>Uwierzytelnianie formularzy i elementu AccountController

Domyślny szablon projektu Visual Studio dla platformy ASP.NET MVC automatycznie włącza uwierzytelnianie formularzy, podczas tworzenia nowej aplikacji ASP.NET MVC. Implementacja strony logowania konta wbudowanych on również automatycznie dodaje do projektu — która naprawdę ułatwia zintegrowane zabezpieczenia w obrębie lokacji.

Domyślnej strony wzorcowej Site.master Wyświetla link "Logowanie" w prawym górnym rogu strony, gdy użytkownik uzyskuje do niej dostęp nie jest uwierzytelniony:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Kliknij łącze "Logowanie" przyjmuje użytkownikowi */Account/logowania* adresu URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Odwiedzający, którzy nie został zarejestrowany można to zrobić, klikając łącze "Register" — co spowoduje przejście do */Account/rejestru* adresu URL i zezwolić im na wprowadź szczegóły konta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Kliknięcie przycisku "Register" zostanie utworzenie nowego użytkownika w ramach systemu członkostwa programu ASP.NET i uwierzytelnianie użytkowników na lokacji przy użyciu uwierzytelniania formularzy.

Jeśli użytkownik jest zalogowany, Site.master zmiany w prawym górnym rogu strony, aby dane wyjściowe "Witaj [username]!" komunikat i renderuje "Wyloguj" łącze zamiast "Logowanie" jeden. Kliknij łącze "Wyloguj" wylogowaniem użytkownika:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Powyższe funkcje logowania, wylogowywania i rejestracji jest zaimplementowana w klasie elementu AccountController, która została dodana do naszej projektu przez program Visual Studio, podczas tworzenia projektu. Interfejs użytkownika dla elementu AccountController jest implementowane za pomocą szablonów widoków w katalogu \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Klasa elementu AccountController używa systemu uwierzytelnianie formularzy aplikacji ASP.NET do wystawiania plików cookie uwierzytelniania szyfrowanego i interfejs API członkostwa ASP.NET do przechowywania i sprawdź poprawność nazwy użytkownika/hasła. Interfejs API członkostwa ASP.NET jest rozszerzony i umożliwia dowolnego magazynu poświadczeń hasło do użycia. Program ASP.NET jest dostarczany z implementacjami dostawcy członkostwa wbudowanych, które przechowywania nazwy użytkownika/hasła w bazie danych SQL lub w usłudze Active Directory.

Można skonfigurować dostawcy członkostwa, które naszej aplikacji NerdDinner należy użyć przez otwarcie pliku "web.config" w katalogu głównym projektu i wyszukując &lt;członkostwa&gt; sekcji w niej. Web.config domyślnej dodane podczas tworzenia projektu rejestruje dostawcę członkostwa SQL i konfiguruje się go do używania ciągu połączenia o nazwie "ApplicationServices" Określ lokalizację bazy danych.

Domyślny ciąg połączenia "ApplicationServices" (który jest określony w ramach &lt;connectionStrings&gt; sekcji w pliku web.config) jest skonfigurowany do używania programu SQL Express. Wskazuje na bazie danych programu SQL Express o nazwie "ASPNETDB. MDF"w aplikacji" aplikacja\_danych "katalogu. Jeśli ta baza danych nie istnieje podczas pierwszego członkostwo interfejsu API jest używany w aplikacji, program ASP.NET automatycznie utworzyć bazę danych i udostępnić odpowiednie członkostwa schematu bazy danych znajdujące się w nim:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Jeśli zamiast korzystać z programu SQL Express, możemy użyć pełnego wystąpienia programu SQL Server (lub Połącz ze zdalną bazą danych), wszystkie będą potrzebujemy zadań do wykonania jest zaktualizować parametry połączenia "ApplicationServices" w pliku web.config i upewnij się, że schemat odpowiednie członkostwa dodano do bazy danych, który wskazuje na. Można uruchomić "aspnet\_regsql.exe" narzędzia w katalogu \Windows\Microsoft.NET\Framework\v2.0.50727\, aby dodać odpowiedniego schematu dla członkostwa i inne usługi aplikacji ASP.NET do bazy danych.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autoryzowanie kolacji/Utwórz adres URL przy użyciu filtru [Authorize]

Nie mamy do pisania kodu umożliwienie bezpiecznego uwierzytelniania i implementacji zarządzania konta dla aplikacji NerdDinner. Użytkownicy mogą zarejestrować nowe konta z naszej aplikacji i logowania/wylogowania witryny.

Teraz możemy dodać logikę autoryzacji do aplikacji i korzystać ze stanu uwierzytelniania i username odwiedzin kontrolować, jakie mogą i nie można wykonać w witrynie. Zacznijmy przez dodanie logiki autoryzacji do metod akcji "Utwórz" klasy Nasze DinnersController. W szczególności firma Microsoft będzie wymagają, aby użytkownicy uzyskujący dostęp do */kolacji/Utwórz* należy być zalogowanym adresu URL. Jeśli nie są one rejestrowane w firma Microsoft będzie przekierowanie do strony logowania, aby ich można logowania.

Implementacja tej logiki jest dość proste. Wszystkie potrzebne do wykonania jest dodanie filtru atrybutu [Authorize] do naszej tworzenia metod akcji w następujący sposób:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC obsługuje możliwość tworzenia "filtry akcji", które mogą służyć do implementacji logiki wielokrotnego użytku, które można deklaratywnie zastosować do metody akcji. Filtr [Authorize] jest jeden z filtrów wbudowanych akcji dostarczane przez platformę ASP.NET MVC i umożliwia deweloperom deklaratywnie dotyczą reguły autoryzacji klasy kontrolera i metody akcji.

Po zastosowaniu bez żadnych parametrów (jak powyżej) filtru [Authorize] wymusza użytkownika zgłaszającego żądanie metody akcji, należy być zalogowanym — czy zostanie automatycznie przekierowania przeglądarki do adresu URL logowania, jeśli nie są one. Podczas wykonywania tego przekierowania pierwotnie żądanego adresu URL jest przekazywany jako querystring argument (na przykład: / Account/logowania? ReturnUrl = % 2fDinners % 2fCreate). Elementu AccountController następnie przekierowuje użytkownika do pierwotnie żądanego adresu URL po ich logowania.

Filtr [Authorize] obsługuje opcjonalnie umożliwia określenie właściwości "Użytkownicy" lub "Role", która może służyć do wymagają, że użytkownik jest zarówno zalogowany w i na liście dozwolonych użytkowników lub członek roli zabezpieczeń dozwolony. Na przykład poniższy kod umożliwia tylko dwóch określonych użytkowników, "scottgu" i "billg" dostępu do adresu URL kolacji/Utwórz:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Osadzanie nazwy określonego użytkownika kodem zwykle można jednak dość cofanie łatwy w obsłudze. Lepszym rozwiązaniem jest określenie wyższego poziomu "role" sprawdzającą kod, a następnie mapować użytkowników do roli przy użyciu bazy danych lub systemu usługi active directory (Włączanie lista mapowania rzeczywistego użytkownika mają być przechowywane zewnętrznie z kodu). Program ASP.NET zawiera interfejsu API zarządzania wbudowanych ról, a także zestaw wbudowanych dostawcy roli (między innymi SQL i usługi Active Directory), które mogą ułatwić wykonywanie tego mapowania użytkowników/ról. Firma Microsoft może następnie zaktualizuj kod, aby umożliwić tylko przez użytkowników w ramach roli określonych "admin" dostępu do adresu URL kolacji/Utwórz:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Za pomocą właściwości User.Identity.Name, tworząc kolacji

Można pobrać nazwy użytkownika aktualnie zalogowanego użytkownika żądania przy użyciu właściwości User.Identity.Name ujawnionymi w klasie podstawowej kontrolera.

Wcześniej podczas wprowadziliśmy wersję HTTP POST naszych metody akcji Create() było zapisane na stałe z właściwością "HostedBy" obiad statyczny ciąg. Firma Microsoft może teraz zaktualizować ten kod, aby zamiast tego użyj właściwości User.Identity.Name, a także automatyczne dodawanie RSVP hosta tworzenia obiad:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Ponieważ atrybut [Authorize] dodano do metody Create(), ASP.NET MVC gwarantuje, że metody akcji wykonywana tylko wtedy, gdy użytkownik odwiedzający kolacji/Utwórz adres URL jest zalogowany w witrynie. W efekcie wartość właściwości User.Identity.Name zawsze zawiera prawidłową nazwę użytkownika.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Za pomocą właściwości User.Identity.Name podczas edytowania kolacji

Teraz Dodajmy ograniczającej użytkowników, tak aby ich można edytować tylko właściwości kolacji, które znajdują się one same logikę autoryzacji.

Ułatwiające pracę z tym najpierw dodamy metody pomocnika "IsHostedBy(username)" do naszej obiad obiektu (Dinner.cs częściowej klasy budujemy wcześniej). Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy zgodna właściwość HostedBy obiad podanej nazwie użytkownika i hermetyzuje logiki niezbędne do wykonania porównania bez uwzględniania wielkości liter ciągu z nich:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Następnie dodamy atrybutu [Authorize] Edit() metod akcji w obrębie klasy Nasze DinnersController. Daje to pewność, że użytkowników musi być zalogowany do żądanie */Dinners/Edit / [id]* adresu URL.

Firma Microsoft można dodać kod do naszej edycji metod, które używa metody pomocnika Dinner.IsHostedBy(username) w celu sprawdź, czy zalogowany użytkownik odpowiada obiad hosta. Jeśli użytkownik nie ma hosta, firma Microsoft będzie wyświetlić "InvalidOwner" i zakończyć żądania. Kod w tym celu wygląda jak poniżej:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Firma Microsoft następnie kliknij prawym przyciskiem myszy w katalogu \Views\Dinners i wybierz polecenie Dodaj -&gt;wyświetlić menu polecenie, aby utworzyć nowy widok "InvalidOwner". Firma Microsoft będzie go wypełnić poniżej komunikat o błędzie:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

I teraz, gdy użytkownik próbuje edytować obiad, które nie są właścicielami, ich zostanie wyświetlony komunikat o błędzie:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Firma Microsoft Powtórz te same kroki dla operacji Delete() metod akcji w obrębie kontrolera do blokowania uprawnienie do usuwania również kolacji i upewnij się, czy tylko hosta obiad można było go usunąć.

### <a name="showinghiding-edit-and-delete-links"></a>Pokazywanie i ukrywanie edytowania i usuwania łącza

Firma Microsoft łącze do metody akcji edytowanie i usuwanie klasy Nasze DinnersController z naszych szczegóły adresu URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Obecnie możemy są wyświetlane łącza akcji edytowanie i usuwanie niezależnie od tego, czy obiekt odwiedzający do adresu URL szczegóły hosta obiad. Umożliwia to zmienić łącza są wyświetlane tylko wtedy, jeśli odwiedzającego użytkownika jest właścicielem obiad.

Metody akcji Details() w naszym DinnersController pobiera obiekt obiad i przekazuje ją jako obiekt modelu do naszej szablonu widoku:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Aktualizujemy nasze szablon widoku warunkowo Pokaż/Ukryj łącza edytowanie i usuwanie przy użyciu Dinner.IsHostedBy() metody pomocniczej, takich jak poniżej:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Następne kroki

Teraz Przyjrzyjmy się jak można włączyć uwierzytelnionym użytkownikom RSVP dla kolacji za pomocą interfejsu AJAX.

>[!div class="step-by-step"]
[Poprzednie](implement-efficient-data-paging.md)
[dalej](use-ajax-to-deliver-dynamic-updates.md)
