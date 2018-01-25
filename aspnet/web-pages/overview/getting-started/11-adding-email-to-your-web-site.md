---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "Wysyłanie wiadomości E-mail z sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym rozdziale opisano sposób wysyłania wiadomości e-mail automatycznych z witryny sieci Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: c5878c3bc468daef050dcebee99f64441066409a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Wysyłanie wiadomości E-mail z witryny sieci Web platformy ASP.NET (Razor) stron
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób używania stron sieci Web platformy ASP.NET (Razor), Wyślij wiadomość e-mail z witryny sieci Web.
> 
> Zawartość:
> 
> - Jak wysłać wiadomość e-mail z witryny sieci Web.
> - Jak dołączyć plik do wiadomości e-mail.
> 
> Jest to funkcja ASP.NET, wprowadzona w artykule:
> 
> - `WebMail` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Wysyłanie wiadomości E-mail z witryny sieci Web

Istnieją różne rodzaje powodów dlaczego może być konieczne do wysyłania wiadomości e-mail z witryny sieci Web. Potwierdzenie wiadomości mogą wysyłać do użytkowników lub może wysyłać powiadomienia do siebie (na przykład, który został zarejestrowany przez nowego użytkownika.) `WebMail` Pomocnika umożliwia łatwe wysyłanie wiadomości e-mail.

Aby użyć `WebMail` pomocnika, musisz mieć dostęp do serwera SMTP. (Oznacza SMTP *Simple Mail Transfer Protocol*.) Serwer SMTP to serwer poczty e-mail, który tylko przesyła dalej wiadomości do serwera adresata &#8212; jest wychodzący po stronie wiadomości e-mail. Jeśli używasz dostawcy usług hosta dla witryny sieci Web, ich prawdopodobnie skonfiguruj można za pomocą poczty e-mail i ich pomagają stwierdzić, co to jest nazwa serwera SMTP. Podczas pracy w sieci firmowej, administratorem lub działem IT można zwykle można uzyskać informacje dotyczące serwera SMTP, który można użyć. Jeśli pracujesz w domu, nawet można przetestować go przy użyciu dostawcy zwykłej poczty e-mail, który można podać nazwę serwera SMTP. Zazwyczaj potrzebne są:

- Nazwa serwera SMTP.
- Numer portu. To jest prawie zawsze 25. Usługodawcy mogą jednak wymagają użycia portu 587. Jeśli korzystasz z bezpiecznego secure sockets layer (SSL) do obsługi poczty e-mail, może być konieczne innego portu. Skontaktuj się z dostawcą poczty e-mail.
- Poświadczenia (nazwa użytkownika, hasło).

W tej procedurze utworzysz dwie strony. Pierwsza strona ma formularz, który umożliwia użytkownikom, podaj opis, tak, jakby były wypełnieniu formularza obsługi technicznej. Pierwsza strona prześle informacje do drugiej strony. Na drugiej stronie kod wyodrębnia dane użytkownika i wysyła wiadomość e-mail. Wyświetla komunikat potwierdzający Odebrano raport o problemie.

![[Obraz]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Aby zachować w tym przykładzie jest proste, inicjuje kod `WebMail` pomocnika prawej strony, której używasz. Jednak dla rzeczywistych witryn sieci Web, jest zorientować się wprowadzić kod inicjujący następująco w pliku globalnego, dzięki czemu można zainicjować `WebMail` pomocnika dla wszystkich plików w witrynie sieci Web. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowanie całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *EmailRequest.cshtml* i Dodaj następujący kod: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Zwróć uwagę, że `action` ustawiono atrybut elementu form *ProcessRequest.cshtml*. Oznacza to, że formularz zostanie przesłany do tej strony, a nie do bieżącej strony.
3. Dodaj nową stronę o nazwie *ProcessRequest.cshtml* witryny sieci Web i Dodaj następujący kod i kod znaczników:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    W kodzie można pobrać wartości pola formularza, które zostały przesłane do strony. Następnie wywołaj `WebMail` przez pomocnika `Send` metody do tworzenia i wysyłania wiadomości e-mail. W tym przypadku wartości do użycia składają się z tekstu, który można łączyć z wartościami, które zostały przesłane z formularza.

    Kod na tej stronie znajduje się wewnątrz `try/catch` bloku. Jeśli z jakiegokolwiek powodu, próba wysłania wiadomości e-mail nie działa (na przykład, te ustawienia są prawo), kod w `catch` blok uruchamia i ustawia `errorMessage` zmiennej, aby wskazywał błąd, który wystąpił. (Aby uzyskać więcej informacji na temat `try/catch` bloków lub `<text>` tagów, zobacz [wprowadzenie do platformy ASP.NET Web Pages programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    W treści strony Jeśli `errorMessage` zmiennej jest pusty (wartość domyślna), użytkownik zobaczy następujący komunikat, który wiadomość e-mail została wysłana. Jeśli `errorMessage` zmienna jest ustawiona na wartość true, użytkownik widzi na komunikat, że wystąpił problem podczas wysyłania wiadomości.

    Należy zauważyć, że w części strony, która wyświetla komunikat o błędzie, jest dodatkowy test: `if(debuggingFlag)`. Jest to zmienna ustawioną na true, jeśli wystąpiły problemy podczas wysyłania wiadomości e-mail. Gdy `debuggingFlag` ma wartość true, a w przypadku problem podczas wysyłania wiadomości e-mail, wyświetlony komunikat o błędzie dodatkowe pokazujący, niezależnie od platformy ASP.NET został zgłoszony podczas próby wysłania wiadomości e-mail. Ostrzeżenie odpowiedniego, choć: komunikaty o błędach, które program ASP.NET zgłasza, jeśli nie można wysłać wiadomości e-mail mogą być ogólne. Na przykład jeśli program ASP.NET nie może skontaktować się z serwerem SMTP (na przykład, ponieważ wprowadzone wystąpił błąd w nazwie serwera), błąd `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Ważne** Jeśli wyświetlony komunikat o błędzie z obiektu wyjątku (`ex` w kodzie), czy *nie* rutynowo przekazywać wiadomości za pośrednictwem użytkownikom. Obiekty wyjątków często zawierają informacje, czy użytkownicy nie powinien być widoczny i które może nawet być luki w zabezpieczeniach. Dlatego ten kod zawiera zmienną `debuggingFlag` który jest używany jako przełącznik, aby wyświetlić komunikat o błędzie i dlaczego zmiennej domyślnie jest ustawiona na wartość false. Należy ustawić tej zmiennej na true (i w związku z tym wyświetlony komunikat o błędzie) *tylko* Jeśli występuje problem z wysyłaniem wiadomości e-mail należy do debugowania. Po rozwiązaniu problemów, ustaw `debuggingFlag` ponownie na wartość false.

    Zmodyfikuj następujące e-mail powiązane ustawienia w kodzie:

    - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
    - Ustaw `your-user-name-here` do nazwy użytkownika dla konta serwera SMTP.
    - Ustaw `your-account-password` hasło dla konta serwera SMTP.
    - Ustaw `your-email-address-here` na adres e-mail. Jest to komunikat jest wysyłany z adres e-mail. (Niektóre dostawców poczty e-mail nie umożliwiają określenie innej `From` adresów, a następnie użyje swoją nazwę użytkownika jako `From` adresu.)

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>Konfigurowanie ustawień poczty E-mail
    > 
    > Może stanowić wyzwanie czasami, aby upewnić się, że masz prawa ustawienia serwera SMTP, numer portu i tak dalej. Poniżej przedstawiono kilka wskazówek:
    > 
    > - Nazwa serwera SMTP jest często przypominać `smtp.provider.com` lub `smtp.provider.net`. Jeśli jednak publikowania witryny dostawcy hostingu, nazwę serwera SMTP w tym momencie może być `localhost`. Jest to spowodowane po opublikowaniu, witryna jest hostowana na serwerze dostawcy, z serwerem poczty e-mail może być lokalny z punktu widzenia aplikacji. Ta zmiana nazwy serwera może oznaczać, że trzeba zmienić nazwę serwera SMTP w trakcie procesu publikowania.
    > - Numer portu jest zwykle 25. Jednak niektóre dostawców wymagają użycia portu 587 lub pewne inne porty.
    > - Upewnij się, że używasz prawidłowych poświadczeń. Po opublikowaniu lokacji do dostawcy hostingu, Użyj poświadczeń, które dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail. Te mogą się różnić od poświadczeń używanych do opublikowania.
    > - Czasami nie potrzebujesz poświadczeń w ogóle. Przy wysyłaniu wiadomości e-mail za pomocą osobistego usługodawca Internetowy dostawcę poczty e-mail może być już wiesz, swoje poświadczenia. Po opublikowaniu, może być konieczne przy użyciu innych poświadczeń niż podczas testowania na komputerze lokalnym.
    > - Jeśli Twój dostawca e-mail używa szyfrowania, należy ustawić `WebMail.EnableSsl` do `true`.
4. Uruchom *EmailRequest.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)
5. Wprowadź nazwę i opis problemu, a następnie kliknij przycisk **przesyłania** przycisku. Są przekierowywane do *ProcessRequest.cshtml* strony, który potwierdza wiadomości i które wysyła do Ciebie wiadomość e-mail. 

    ![[Obraz]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Wysłanie pliku za pomocą poczty E-mail

Można również wysłać pliki, które są dołączone do wiadomości e-mail. W tej procedurze Utwórz plik tekstowy i dwie strony HTML. Użyjesz plik jako załącznik wiadomości e-mail.

1. W witrynie sieci Web, Dodaj nowy plik tekstowy i nadaj mu nazwę *mojplik.txt*.
2. Skopiuj poniższy tekst i wklej go w pliku: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Utwórz stronę o nazwie *SendFile.cshtml* i Dodaj następujący kod: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Utwórz stronę o nazwie *ProcessFile.cshtml* i Dodaj następujący kod: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modyfikowanie powiązanych ustawień w kodzie w przykładzie poczty e-mail następujące:

    - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
    - Ustaw `your-user-name-here` do nazwy użytkownika dla konta serwera SMTP.
    - Ustaw `your-email-address-here` na adres e-mail. Jest to komunikat jest wysyłany z adres e-mail.
    - Ustaw `your-account-password` hasło dla konta serwera SMTP.
    - Ustaw `target-email-address-here` na adres e-mail. (Jak przed, czy zwykle wysłać wiadomość e-mail do innej osoby, ale do testowania, możesz go wysłać do siebie).
6. Uruchom *SendFile.cshtml* strony w przeglądarce.
7. Wprowadź nazwę, wiersz tematu i nazwę pliku tekstowego, aby dołączyć (*mojplik.txt*).
8. Kliknij przycisk `Submit` przycisku. Jak wcześniej, są przekierowywane do *ProcessFile.cshtml* strony, który potwierdza wiadomości i które wysyła do Ciebie wiadomość e-mail z załączony plik.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protokół Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
