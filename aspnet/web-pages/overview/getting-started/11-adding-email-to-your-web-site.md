---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Wysyłanie wiadomości E-mail z sieci Web platformy ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale wyjaśniono, jak wysyłać wiadomości e-mail automatycznych w witrynie sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 8a65a152dc72725b1ff65eefd47890b42cb7ea90
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394051"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Wysyłanie wiadomości E-mail z witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób wysyłania wiadomości e-mail z witryny sieci Web, korzystając z ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak wysłać wiadomość e-mail z witryny sieci Web.
> - Jak dołączyć plik do wiadomości e-mail.
> 
> Jest to ASP.NET wprowadzonej w artykule:
> 
> - `WebMail` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Wysyłanie wiadomości E-mail z witryny sieci Web

Istnieje wiele powodów dlaczego może być konieczne wysyłanie wiadomości e-mail z witryny sieci Web. Potwierdzenie wiadomości może wysłać do użytkowników lub może wysyłać powiadomienia do siebie (na przykład, że nowy użytkownik został zarejestrowany.) `WebMail` Pomocnika ułatwia Ci na wysyłanie wiadomości e-mail.

Aby użyć `WebMail` pomocnika, musisz mieć dostęp do serwera SMTP. (Oznacza SMTP *Simple Mail Transfer Protocol*.) Serwer SMTP to serwer poczty e-mail, który tylko przekazuje dalej wiadomości do serwera adresata &#8212; jest stronie wychodzących wiadomości e-mail. Jeśli używasz dostawcy usług hosta dla witryny sieci Web, prawdopodobnie skonfigurowane możesz za pomocą adresu e-mail i ich może powiadomić co to jest nazwa serwera SMTP. Jeśli pracujesz w sieci firmowej, administratorem lub działem IT można zwykle zapewniają informacje dotyczące serwera SMTP, który można użyć. Jeśli pracujesz w domu, nawet można przetestować za pomocą dostawcy zwykłej poczty e-mail, który można podać nazwę serwera SMTP. Zazwyczaj są potrzebne:

- Nazwa serwera SMTP.
- Numer portu. Prawie zawsze wynosi 25. Usługodawcy mogą jednak wymagać używają portu 587. Jeśli używasz protokołu secure sockets layer (SSL) do obsługi poczty e-mail, możesz potrzebować innego portu. Skontaktuj się z dostawcy poczty e-mail.
- Poświadczenia (nazwę użytkownika, hasło).

W tej procedurze utworzysz dwie strony. Pierwsza strona zawiera formularz, który umożliwia użytkownikom, podaj opis, tak, jakby były wypełnianie formularza wsparcia technicznego. Pierwsza strona przesyła jego informacje do drugiej strony. Na drugiej stronie kod wyodrębnia informacje o użytkowniku i wysyła wiadomość e-mail. Wyświetla komunikat potwierdzający, że otrzymał raporcie dotyczącym problemu.

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> W celu uproszczenia w tym przykładzie inicjuje kod `WebMail` pomocnika po prawej stronie na stronie, których używasz. Jednak rzeczywiste witryn sieci Web, jest lepiej zrozumieć, należy umieścić kod inicjowania w pliku globalnego, dzięki czemu można zainicjować `WebMail` pomocnika dla wszystkich plików w witrynie sieci Web. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowania dla całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *EmailRequest.cshtml* i Dodaj następujący kod: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Należy zauważyć, że `action` ustawiono atrybut elementu form *ProcessRequest.cshtml*. Oznacza to, że formularz zostanie przesłany do tej strony, zamiast wstecz do bieżącej strony.
3. Dodaj nową stronę o nazwie *ProcessRequest.cshtml* do witryny sieci Web i Dodaj następujący kod i znaczników:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    W kodzie uzyskasz wartości pól formularza, które zostały przesłane do strony. Następnie wywołaj `WebMail` Pomocnika `Send` metody do tworzenia i wysyłania wiadomości e-mail. W tym przypadku wartości, które składają się z tekstu, który łączenie z wartościami, które zostały przesłane z formularza.

    Kod na tej stronie znajduje się wewnątrz `try/catch` bloku. Jeśli z jakiegokolwiek powodu próby wysłania wiadomości e-mail nie działa (na przykład ustawienia nie są odpowiednie), kod w `catch` bloku uruchamia i ustawia `errorMessage` zmienną błędu, który wystąpił. (Aby uzyskać więcej informacji na temat `try/catch` bloków lub `<text>` tagów, zobacz [wprowadzenie do platformy ASP.NET Web Pages programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    W treści strony Jeśli `errorMessage` zmiennej jest pusty (ustawienie domyślne), użytkownik zobaczy komunikat, który została wysłana wiadomość e-mail. Jeśli `errorMessage` zmienna jest ustawiona na wartość true, użytkownik widzi na komunikat, że wystąpił problem podczas wysyłania wiadomości.

    Zwróć uwagę, że w części strony, która wyświetla komunikat o błędzie, jest dodatkowy test: `if(debuggingFlag)`. Jest to zmienna, którego można ustawić wartość true, jeśli wystąpiły problemy podczas wysyłania wiadomości e-mail. Gdy `debuggingFlag` ma wartość true, a jeśli występuje problem podczas wysyłania wiadomości e-mail, komunikat o błędzie dodatkowe są wyświetlane pokazujący, niezależnie od platformy ASP.NET został zgłoszony podczas próby wysłania wiadomości e-mail. Ostrzeżenie podejście, jednak: komunikaty o błędach, które ASP.NET raportów, gdy nie jest w stanie wysyłać wiadomości e-mail może być ogólna. Na przykład jeśli ASP.NET nie może skontaktować się z serwerem SMTP, (na przykład dlatego, że wprowadzone błąd w nazwie serwera), błąd jest `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Ważne** gdy otrzymasz komunikat o błędzie z obiektu wyjątku (`ex` w kodzie), czy *nie* rutynowo przekazać ten komunikat za pośrednictwem dla użytkowników. Obiekty wyjątków często zawierają informacje, nie powinni widzieć użytkownicy i które może być nawet luki w zabezpieczeniach. Dlatego ten kod zawiera zmienną `debuggingFlag` który jest używany jako przełącznik, aby wyświetlić komunikat o błędzie i dlaczego zmiennej, domyślnie jest ustawiona na wartość false. Należy ustawić tej zmiennej na wartość true (i w związku z tym wyświetlony komunikat o błędzie) *tylko* potrzebne do debugowania, jeśli występuje problem z wysyłaniem wiadomości e-mail. Po rozwiązaniu problemów, ustaw `debuggingFlag` ponownie na wartość false.

    Zmodyfikuj następujące e-mail powiązane ustawienia w kodzie:

   - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
   - Ustaw `your-user-name-here` nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `your-account-password` hasło dla konta serwera SMTP.
   - Ustaw `your-email-address-here` na adres e-mail. To jest adres e-mail, który komunikat jest wysyłany z. (Niektórzy dostawcy poczty e-mail nie pozwalają określić inną `From` adresem, a następnie użyje nazwy użytkownika jako `From` adresu.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Konfigurowanie ustawień poczty E-mail
     > 
     > Może być wyzwaniem wymagającym czasami upewnij się, że masz odpowiednie ustawienia serwera SMTP, numer portu i tak dalej. Poniżej przedstawiono kilka wskazówek:
     > 
     > - Nazwa serwera SMTP jest często podobny `smtp.provider.com` lub `smtp.provider.net`. Jeśli jednak publikowania witryny dostawcy usług hostingowych, nazwę serwera SMTP w tym momencie może być `localhost`. Jest to spowodowane po opublikowaniu i witryna jest hostowana na serwerze dostawcy, serwer poczty e-mail może być lokalne z perspektywy aplikacji. Ta zmiana nazwy serwera może oznaczać, że trzeba zmienić nazwę serwera SMTP w ramach procesu publikowania.
     > - Numer portu jest zwykle 25. Jednakże niektórzy dostawcy wymagają użycia portu 587 lub niektórych innych portów.
     > - Upewnij się, że używasz właściwych poświadczeń dostępu. Po opublikowaniu witryny dostawcy usług hostingowych, Użyj poświadczeń z dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail. Mogą być inne niż poświadczenia, których używasz do publikowania.
     > - Czasami nie potrzebujesz poświadczeń w ogóle. Przy wysyłaniu wiadomości e-mail za pomocą osobistego usługodawcę internetowego dostawcy poczty e-mail może już znasz swoje poświadczenia. Po opublikowaniu, może być konieczne użycie innych poświadczeń niż podczas testowania na komputerze lokalnym.
     > - Jeśli dostawcy poczty e-mail używa szyfrowania, należy ustawić `WebMail.EnableSsl` do `true`.
4. Uruchom *EmailRequest.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.)
5. Wprowadź nazwę i opis problemu, a następnie kliknij przycisk **przesyłania** przycisku. Są przekierowywane do *ProcessRequest.cshtml* strony, który potwierdza wiadomości i który wyśle do Ciebie wiadomość e-mail. 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Wysłanie pliku za pomocą poczty E-mail

Można również wysłać pliki, które są dołączone do wiadomości e-mail. W tej procedurze utworzysz plik tekstowy i dwie strony HTML. Użyjesz pliku tekstowego jako załącznik wiadomości e-mail.

1. W witrynie sieci Web, Dodaj nowy plik tekstowy i nadaj mu nazwę *mójplik.txt*.
2. Skopiuj poniższy tekst i wklej go w pliku: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Utwórz stronę o nazwie *SendFile.cshtml* i Dodaj następujący kod: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Utwórz stronę o nazwie *ProcessFile.cshtml* i Dodaj następujący kod: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Zmodyfikuj następujące e-mail powiązanych ustawień w kod z przykładu:

    - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
    - Ustaw `your-user-name-here` nazwę użytkownika dla konta serwera SMTP.
    - Ustaw `your-email-address-here` na adres e-mail. To jest adres e-mail, który komunikat jest wysyłany z.
    - Ustaw `your-account-password` hasło dla konta serwera SMTP.
    - Ustaw `target-email-address-here` na adres e-mail. (Jak wcześniej, jak zwykle wysyłając wiadomość e-mail do innej osoby, ale do testowania, możesz go wysłać do siebie).
6. Uruchom *SendFile.cshtml* strony w przeglądarce.
7. Wprowadź nazwę, wiersz tematu i nazwę pliku tekstowego, aby dołączyć (*mójplik.txt*).
8. Kliknij przycisk `Submit` przycisku. Jak poprzednio, są przekierowywane do *ProcessFile.cshtml* stronę, potwierdzenie wiadomości i który wyśle do Ciebie wiadomość e-mail za pomocą dołączonego pliku.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Dostosowywanie zachowania dla całej witryny dla stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
