---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 9 przedstawiono sposób dodawania uwierzytelniania i autoryzacji, aby zabezpieczyć naszej aplikacji NerdDinner, dzięki czemu użytkownicy będą musieli zarejestrować i zaloguj się do witryny w celu utworzenia...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d5f1b26312f11fd6d4ab500c7f24a4d89d428e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754979"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 9 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 9 przedstawiono sposób dodawania uwierzytelniania i autoryzacji, aby zabezpieczyć naszej aplikacji NerdDinner, dzięki czemu użytkownicy będą musieli zarejestrować i zaloguj się do witryny Aby utworzyć nowy kolacji i tylko użytkownik, który jest hostem obiad można edytować go później.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner krok 9: Uwierzytelnianie i autoryzacja

Obecnie nasz NerdDinner przyznaje aplikacji, każda osoba, odwiedzając witrynę możliwość tworzenia i edytowania szczegółów dowolnego obiad. Wybierzmy tak, aby użytkownicy musieli zarejestrować i zaloguj się do witryny Aby utworzyć nowy kolacji i Dodaj ograniczenie, aby tylko użytkownik, który jest hostem obiad można go później edytować.

Aby włączyć tę użyjemy do naszej aplikacji bezpiecznego uwierzytelniania i autoryzacji.

### <a name="understanding-authentication-and-authorization"></a>Opis uwierzytelniania i autoryzacji

*Uwierzytelnianie* to proces identyfikowania i sprawdzania poprawności tożsamości klienta dostępu do aplikacji. Po prostu przełączyć, to kwestia identyfikowanie "będącego przez użytkownika końcowego podczas odwiedzania witryny sieci Web". Program ASP.NET obsługuje wiele sposobów, aby uwierzytelniać użytkowników w przeglądarce. Dla aplikacji sieci web w Internecie najbardziej typowe metody uwierzytelniania używane nosi nazwę "Uwierzytelnianie formularzy". Uwierzytelnianie formularzy umożliwia deweloperom tworzenie formularza logowania HTML w swoich aplikacjach, a następnie sprawdź poprawność nazwy użytkownika/hasła, które użytkownik końcowy przesyła względem bazy danych lub w innym magazynie poświadczeń hasła. Kombinacja nazwy użytkownika i hasła jest poprawna, deweloper następnie poprosić ASP.NET do wysyłania zaszyfrowanego pliku cookie HTTP do identyfikacji użytkownika w przyszłych żądań. Firma Microsoft będzie przy użyciu uwierzytelniania formularzy z naszej aplikacji NerdDinner.

*Autoryzacja* jest proces określania, czy uwierzytelniony użytkownik ma uprawnienia do uzyskania dostępu do określonego adresu URL/zasobu lub wykonanie akcji. Na przykład w ramach naszej aplikacji NerdDinner będzie chcemy autoryzować dostęp tylko użytkownicy, którzy są zalogowani */kolacji/tworzenie* adresu URL i Utwórz nowe kolacji. Chcemy będzie również dodać logikę autoryzacji, tak aby tylko użytkownik, który jest hostem obiad można edytować — i nie zezwoli na dostęp do edycji do innych użytkowników.

### <a name="forms-authentication-and-the-accountcontroller"></a>Uwierzytelnianie formularzy i elementu AccountController

Domyślny szablon projektu Visual Studio dla platformy ASP.NET MVC automatycznie włącza uwierzytelnianie formularzy, podczas tworzenia nowej aplikacji platformy ASP.NET MVC. Dodaje również automatycznie z implementacją strony logowania wstępnie utworzone konta do projektu — która naprawdę ułatwia integrowanie zabezpieczeń w ramach lokacji.

Domyślna strona wzorcowa Site.master Wyświetla łącze "Logowanie" w prawym górnym rogu witryny przypadku nieuwierzytelnionego użytkownika dostępu do niego:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Kliknięcie linku "Logowanie" przyjmuje użytkownikowi */konta/logowania* adresu URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Goście, którzy jeszcze nie dokonano rejestracji można to zrobić, klikając link "Register" — co spowoduje przejście do */konto/Register* adresu URL i zezwolić im na wprowadź szczegóły konta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Klikając przycisk "Zarejestruj" spowoduje utworzenie nowego użytkownika w ramach systemu członkostwa ASP.NET i uwierzytelniania użytkownika do witryny za pomocą uwierzytelniania formularzy.

Gdy użytkownik jest zalogowany, Site.master zmiany w prawym górnym rogu strony aby output "Witaj [username]!" wiadomości i renderuje "Wyloguj" link zamiast "Logowanie" jeden. Kliknięcie linku "Wyloguj" Wylogowuje użytkownika:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Powyższe funkcje logowania, wylogowania i rejestracji jest zaimplementowana w klasie elementu AccountController, który został dodany do naszych projektów przez program Visual Studio, podczas tworzenia projektu. W interfejsie użytkownika dla elementu AccountController jest implementowane za pomocą szablonów widoku katalogu \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Klasa elementu AccountController korzysta z systemu uwierzytelnianie formularzy programu ASP.NET do wysyłania plików cookie uwierzytelniania szyfrowanego, a interfejs API członkostwa platformy ASP.NET do przechowywania i sprawdź poprawność nazwy użytkownika i hasła. Interfejs API członkostwa platformy ASP.NET jest rozszerzalny i umożliwia dowolnym magazynu poświadczeń haseł ma być używany. ASP.NET jest dostarczany z implementacji dostawcy członkostwa wbudowanych, które przechowywania nazwy użytkownika i hasła w bazie danych SQL lub w usłudze Active Directory.

Można skonfigurować co dostawcy członkostwa, otwierając plik "web.config" w katalogu głównym projektu i szukasz skorzystaj z naszej aplikacji NerdDinner &lt;członkostwa&gt; sekcji znajdujący się w nim. Pliku web.config domyślnej dodany podczas tworzenia projektu rejestruje dostawcy członkostwa SQL i konfiguruje go do korzystania ze ciąg połączenia o nazwie "ApplicationServices" Aby określić lokalizację bazy danych.

Domyślne parametry połączenia "ApplicationServices" (która jest określona w ramach &lt;connectionStrings&gt; sekcja pliku web.config) jest skonfigurowana do używania programu SQL Express. Wskazuje bazę danych programu SQL Express, o nazwie "ASPNETDB. MDF"w ramach aplikacji" aplikacja\_dane "katalogu. Jeśli ta baza danych nie istnieje podczas pierwszego interfejsu API członkostwa jest używany w aplikacji, program ASP.NET automatycznie utworzyć bazę danych i obsługi administracyjnej schematu bazy danych członkostwa odpowiednie znajdujący się w nim:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Jeśli zamiast korzystać z programu SQL Express, Chcieliśmy, aby użyć pełnego wystąpienia programu SQL Server (lub połączenia ze zdalną bazą danych), wszystkie będą potrzebujemy zadań do wykonania jest zaktualizować parametry połączenia "ApplicationServices" w pliku web.config i upewnij się, że schemat właściwe członkostwo dodano do bazy danych, który wskazuje na. Można uruchomić "aspnet\_regsql.exe" narzędzia w katalogu \Windows\Microsoft.NET\Framework\v2.0.50727\, aby dodać odpowiedni schemat członkostwa i innych usług aplikacji ASP.NET z bazą danych.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autoryzowanie kolacji/tworzenia adresu URL przy użyciu filtru [Authorize]

Nie mamy do pisania jakiegokolwiek kodu, aby umożliwić bezpieczne uwierzytelnianie i implementacji zarządzania konta dla aplikacji NerdDinner. Użytkownicy mogą rejestrować nowe konta, korzystając z aplikacji i logowania/wylogowania witryny.

Teraz możemy dodać logikę autoryzacji do aplikacji i stosowania stanu uwierzytelniania i nazwa użytkownika osoby odwiedzające do kontrolowania, co mogą i nie można wykonać w witrynie. Zacznijmy od dodania logika autoryzacji do metod akcji "Utwórz" klasy Nasze DinnersController. W szczególności firma Microsoft będzie wymagającą użytkowników uzyskujących dostęp do */kolacji/tworzenie* adres URL musi być zalogowany. Jeśli nie są one rejestrowane w firma Microsoft będzie przekierowywać je do strony logowania, aby można logowania.

Implementowanie logiki ten jest całkiem proste. Wszystkie potrzebne do wykonania jest dodanie filtru atrybutu [Authorize] do naszych tworzenia metod akcji w następujący sposób:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC obsługuje możliwość tworzenia "filtry akcji", które można zaimplementować logikę do ponownego użycia, które mogą być w sposób deklaratywny stosowane do metody akcji. Filtr [Authorize] jest jednym z filtrów wbudowanych akcji, dostarczone przez platformę ASP.NET MVC i umożliwia deweloperom deklaratywne stosuje reguły autoryzacji do klasy kontrolera i metody akcji.

Po zastosowaniu bez żadnych parametrów (na przykład powyżej) filtru [Authorize] wymusza, że musi być zalogowany użytkownika zgłaszającego żądanie metody akcji — i spowoduje automatyczne przekierowanie przeglądarki do adresu URL logowania, jeśli nie są. Podczas wykonywania tego przekierowania, które pierwotnie żądanego adresu URL jest przekazywany jako argument ciąg zapytania (na przykład: / konta/logowania? ReturnUrl = % 2fDinners % 2fCreate). Elementu AccountController następnie przekierowuje użytkownika do pierwotnie żądanego adresu URL po ich logowania.

Filtr [Authorize] opcjonalnie obsługuje możliwość określenia właściwości "Użytkownicy" lub "Role", która może służyć do wymagają, że użytkownik jest zarówno zalogowany w i w ramach listę uprawnionych użytkowników albo być członkiem roli zabezpieczeń dozwolone. Na przykład poniższy kod umożliwia tylko dwóch określonych użytkowników, "scottgu" i "billg" dostępu do adresu URL kolacji/tworzenia:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Osadzanie nazwy określonego użytkownika w ramach kodu zwykle odbywać się dość niezaznaczone łatwego w utrzymaniu. Lepszym rozwiązaniem jest zdefiniowanie wyższego poziomu "role" sprawdzający kodu przed, a następnie mapowania użytkowników do roli przy użyciu bazy danych lub systemu usługi active directory (Włączanie lista mapowania rzeczywistego użytkownika mają być przechowywane zewnętrznie z kodu). Program ASP.NET zawiera interfejs API zarządzania wbudowana rola, a także zestaw wbudowanych dostawców ról (w tym te, dla języków SQL i usługi Active Directory), które mogą ułatwić wykonywanie tego mapowania użytkownika/roli. Firma Microsoft może następnie zaktualizować kod, aby zezwalać tylko na użytkowników w ramach roli określonych "admin" dostępu do adresu URL kolacji/tworzenia:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Przy użyciu właściwości User.Identity.Name, tworząc kolacji

Firma Microsoft może pobrać nazwy użytkownika aktualnie zalogowanego użytkownika żądania przy użyciu właściwości User.Identity.Name widoczne w klasie bazowej kontrolera.

Wcześniej gdy wdrożyliśmy żądania HTTP POST wersję naszych metody akcji Create() mieliśmy zapisane na stałe właściwości "HostedBy" obiad statyczny ciąg. Firma Microsoft może teraz zaktualizować ten kod, aby zamiast tego należy użyć właściwości User.Identity.Name, a także automatyczne dodawanie odpowiedź na zaproszenie dla tego hosta tworzenia Dinner:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Ponieważ dodaliśmy atrybutu [Authorize] do metody Create(), ASP.NET MVC zapewnia, że metody akcji wykonywana tylko wtedy, gdy użytkownik odwiedzający kolacji/tworzenia adresu URL jest zalogowany w witrynie. Jako takie wartość właściwości User.Identity.Name zawsze będzie zawierać prawidłową nazwę użytkownika.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Przy użyciu właściwości User.Identity.Name podczas edytowania kolacji

Teraz Dodajmy logikę autoryzacji, który ogranicza możliwość użycia użytkowników, dzięki czemu można ich edytować tylko właściwości kolacji, które hostują oni sami.

Aby to ułatwić, najpierw dodamy metodę pomocnika "IsHostedBy(username)" nasze obiektowi obiad (w ramach klasy częściowej Dinner.cs, którą utworzyliśmy wcześniej). Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy podana nazwa użytkownika jest zgodna właściwości HostedBy obiad i hermetyzuje logikę niezbędną do wykonania porównania bez uwzględniania wielkości liter ciągu z nich:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Następnie dodamy atrybutu [Authorize] do metod akcji Edit() w ramach naszych DinnersController klasy. Daje to pewność, że użytkownicy musi być zalogowany do żądania */Dinners/Edit / [id]* adresu URL.

Firma Microsoft można dodać kod do naszych metod edycji, które używa metody pomocnika Dinner.IsHostedBy(username) do Sprawdź, czy zalogowany użytkownik zgodny host obiad. Jeśli użytkownik nie jest host, utworzymy wyświetlić "InvalidOwner" i zakończyć żądania. Kod, aby to zrobić wygląda jak poniżej:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Firma Microsoft następnie kliknij prawym przyciskiem myszy w katalogu \Views\Dinners i wybierz polecenie Add -&gt;wyświetlić polecenie menu, aby utworzyć nowy widok "InvalidOwner". Będziesz wypełnimy ją poniżej komunikat o błędzie:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

A teraz, gdy użytkownik próbuje Edytuj obiad, które nie są właścicielami, uzyska komunikat o błędzie:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Firma Microsoft Powtórz te same kroki dla metody akcji Delete(), w ramach kontrolera do blokowania uprawnienie do usuwania kolacji także i upewnij się, że hosta obiad go usunąć.

### <a name="showinghiding-edit-and-delete-links"></a>Pokazywanie i ukrywanie edycji i usuwania łącza

Firma Microsoft tworzysz łącze do metody akcji edytowania i usuwania klasy Nasze DinnersController szczegóły adresu URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Obecnie firma Microsoft są wyświetlane łącza akcji edytowania i usuwania, niezależnie od tego, czy obiekt odwiedzający do adresu URL szczegółów hosta dinner. Wybierzmy tak, aby łącza są wyświetlane tylko wtedy, gdy zaproszonych użytkownik jest właścicielem dinner.

Details() metody akcji w ramach naszych DinnersController pobiera obiekt obiad i przekazuje go jako obiekt modelu do naszych Wyświetl szablon:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Aktualizujemy nasze Wyświetl szablon warunkowo pokazywania/ukrywania edytowania i usuwania łącza za pomocą Dinner.IsHostedBy() metody pomocnika, takich jak poniżej:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Następne kroki

Teraz Przyjrzyjmy się jak można włączyć do RSVP dla kolacji za pomocą interfejsu AJAX uwierzytelnionych użytkowników.

> [!div class="step-by-step"]
> [Poprzednie](implement-efficient-data-paging.md)
> [dalej](use-ajax-to-deliver-dynamic-updates.md)
