---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "ASP.NET Web Pages przewodnik rozwiązywania problemów (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano problemy, które mogą wystąpić podczas pracy z stron sieci Web platformy ASP.NET (Razor) oraz sugerowane rozwiązania. Wersje oprogramowania Zazn sieci Web ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 72c49ed8b76b5fb5eb15bf01f57980b382607496
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Strony sieci Web platformy ASP.NET (Razor) przewodnik rozwiązywania problemów
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano problemy, które mogą wystąpić podczas pracy z stron sieci Web platformy ASP.NET (Razor) oraz sugerowane rozwiązania.
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0.


Ten temat zawiera następujące sekcje:

- [Problemy z uruchamianiem stron](#Issues_Running_.cshtml_Pages)
- [Problemy z kodu Razor](#IssuesWithRazorCode)
- [Problemy dotyczące zabezpieczeń i członkostwa](#membership)
- [Problemy z wysyłaniem wiadomości E-mail](#email)
- [Dodatkowe zasoby](#AdditionalResources)

Pytania ogólne, zobacz [stron sieci Web platformy ASP.NET (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemy z uruchamianiem stron

Różne problemy można zapobiec *.cshtml* i *.vbhtml* stron z prawidłowo uruchomiona. Ta sekcja zawiera typowe komunikaty o błędach i prawdopodobne przyczyny.

### <a name="http-error-403---forbidden-access-is-denied"></a>Błąd HTTP 403 — Dostęp zabroniony: Odmowa dostępu

*Nie masz uprawnień do wyświetlenia tego katalogu lub strony przy użyciu podanych poświadczeń.*

Ten błąd może wystąpić, jeśli serwer nie działa poprawna wersja programu .NET Framework. Upewnij się, czy komputera, na którym uruchomiono serwer (lokalnie lub zdalnie) jest co najmniej programu .NET Framework 4 zainstalowane. Upewnij się, że sama aplikacja jest skonfigurowany do uruchamiania właściwej wersji również.

Jeśli widzisz ten problem z lokalnie podczas pracy w programie WebMatrix, kliknij przycisk **lokacji** obszaru roboczego, a następnie w widoku drzewa kliknij **ustawienia**. W **wybierz wersję systemu .NET Framework** wybierz **.NET 4 (zintegrowany)**. Jeśli ta wersja jest już ustawiona, spróbuj uruchomić program WebMatrix jako administrator.

Upewnij się, że w katalogu głównym witryny sieci Web ma co najmniej jedną *.cshtml* pliku w nim.

Jeśli zostanie wyświetlony ten błąd, gdy serwer sieci web znajduje się na serwerze zdalnym, skontaktuj się z administratorem serwera. Upewnij się, że serwer ma programu .NET Framework 4 lub nowszej. Upewnij się, że aplikacja działa w puli aplikacji jest skonfigurowana do używania tej wersji środowiska.NET Framework również.

Jeśli masz kontrolę nad serwer, upewnij się, że działa ona poprawna wersja programu .NET Framework. Możesz również spróbować naprawić instalację, uruchamiając `aspnet_regiis -iru` polecenia. (Na przykład po zainstalowaniu programu IIS po zainstalowaniu programu .NET Framework, usługi IIS będzie nie być poprawnie skonfigurowany do uruchamiania stron ASP.NET.) Aby uzyskać więcej informacji, zobacz [narzędzie rejestracji programu ASP.NET usług IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>403.14 — dostęp zabroniony. Błąd HTTP

*Serwer sieci Web skonfigurowano nie listy zawartości tego katalogu.*

Ten błąd może wystąpić, jeśli żądanie zostało wysłane z zasobem, który jest chroniony (takich jak *Web.config* pliku) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).

### <a name="http-error-40417---not-found"></a>Błąd HTTP 404.17 — nie znaleziono

*Żądana zawartość najprawdopodobniej jest skryptem i nie zostanie obsłużona przez obsługę plików statycznych.*

Ten błąd może wystąpić, jeśli serwer nie jest poprawnie skonfigurowany do użycia programu .NET Framework 4 lub później, a zatem nie rozpoznaje kod w `@{ }` bloków. Zobacz opis wcześniej *HTTP błąd 403 — Dostęp zabroniony: odmowa dostępu*.

### <a name="http-error-4047---not-found"></a>Błąd HTTP 404.7 — nie znaleziono

*W module filtrowania żądań skonfigurowano odrzucanie podanego rozszerzenia pliku*

Ten błąd może wystąpić, jeśli *.cshtml* lub *.vbhtml* jawnie zablokowano rozszerzeń na serwerze. Objawem tego problemu jest pracy adresów URL, gdy nie obejmują one rozszerzenie, ale adresów URL, które obejmują *.cshtml* lub *.vbhtml* nie działają. Możliwe rozwiązanie jest ponownie włączyć rozszerzenia w tej witrynie *Web.config* pliku. Poniższy przykład przedstawia sposób włączania *.cshtml* rozszerzenia.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Błąd HTTP 404.8 — nie znaleziono

*W module filtrowania żądań skonfigurowano odrzucanie ścieżek w adresie URL zawierających sekcję hiddenSegment.*

Ten błąd może wystąpić, jeśli żądanie zostało wysłane z zasobem, który jest chroniony (takich jak *Web.config* pliku) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Ten typ strony nie jest obsługiwany (błąd serwera w "/" aplikacja)

Zobacz opis wcześniej 404.17 błędu HTTP.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemy z kodu Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Nazwa "*klasy*" nie istnieje w bieżącym kontekście

Często przyczyną wystąpienia tego błędu jest to, że `class` odwołania pomocnika, ale pomocnika nie jest zainstalowany. Na przykład Jeśli spróbujesz użyć pomocnika, ale pakiet nie został zainstalowany z pakietu NuGet, zostanie wyświetlony ten błąd. Galerii w programie WebMatrix można użyć, aby znaleźć i zainstalować pomocnika.

Jeśli zainstalowano pomocnika, ale strony nadal nie rozpoznaje ją, spróbuj dodać dodać `using` instrukcji w kodzie. W `using` instrukcji, odwołanie do przestrzeni nazw, która zawiera pomocnika. Na przykład podstawowa wątków, które znajdują się w pakiecie pomocników sieci Web ASP.NET znajdują się w `System.Web.Helpers` przestrzeni nazw. W górnej części strony, której chcesz użyć pomocnika Dodaj następujący wiersz:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemy dotyczące zabezpieczeń i członkostwa

Jeśli korzystasz z systemu zabezpieczeń (Członkowskimi) na stronach sieci Web platformy ASP.NET (Razor), można napotkać następujące problemy.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Aby wywołać tę metodę, właściwość "Membership.Provider" musi być wystąpieniem typu "ExtendedMembershipProvider"

Ten błąd może oznaczać, że nie `AspNetSqlMembershipProvider` klasa jest skonfigurowana. (Objawem jest że lokacji działa prawidłowo lokalnie, ale zgłasza błąd podczas publikowania do dostawcy hostingu serwera). Jedną poprawkę dotyczącą tego problemu jest jawnie włączyć członkostwa prostego przez dodanie poniższego lokacji *Web.config* pliku:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemy z wysyłaniem wiadomości E-mail

Problemy z wysyłaniem wiadomości e-mail może być wyzwaniem do debugowania. Początkowa problem może być, nie można połączyć się z serwerem SMTP. Jeśli połączenie zostanie nawiązane, ASP.NET przekazuje komunikat do serwera SMTP. Jednak mogą wystąpić problemy z samej wiadomości, który uniemożliwia wysłanie ich do serwera SMTP.

Jeśli aplikacja nie pomyślnie wysłać wiadomości e-mail, należy spróbować wykonać następujące czynności:

- Nazwa serwera SMTP jest często przypominać `smtp.provider.com` lub `smtp.provider.net`. Jeśli jednak publikowania witryny dostawcy hostingu, nazwę serwera SMTP w tym momencie może być `localhost`. Ta sytuacja występuje, ponieważ po opublikowaniu, witryna jest hostowana na serwerze dostawcy, serwer SMTP może być lokalny z punktu widzenia aplikacji. Ta zmiana nazwy serwera może to oznaczać trzeba zmienić nazwę serwera SMTP w trakcie procesu publikowania.
- Numer portu jest zwykle 25. Jednak niektóre dostawców wymagają użycia portu 587 lub pewne inne porty. Skontaktuj się z właścicielem serwera SMTP, numer portu używanego oczekiwane użycia.
- Upewnij się, że używasz prawidłowych poświadczeń. Po opublikowaniu lokacji do dostawcy hostingu, Użyj poświadczeń, które dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail. Te poświadczenia mogą się różnić od poświadczeń używanych do opublikowania.
- Czasami nie potrzebujesz poświadczeń w ogóle. Przy wysyłaniu wiadomości e-mail za pomocą osobistego usługodawca Internetowy, Twój dostawca poczty e-mail może być już wiesz, poświadczenia. Po opublikowaniu, może być konieczne przy użyciu innych poświadczeń niż podczas testowania na komputerze lokalnym.
- Jeśli Twój dostawca e-mail używa szyfrowania, ustaw `WebMail.EnableSsl` do `true`.

W przypadku błąd podczas wysyłania wiadomości e-mail, można napotkać komunikat o błędzie standardowe ASP.NET, która wygląda następująco:

![Komunikat o błędzie platformy ASP.NET, gdy występuje problem z pocztą e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Można również debugowania problemów z wysyłaniem wiadomości e-mail przy użyciu `try-catch` bloku, jak w poniższym przykładzie. Jeśli używasz `try-catch` bloku, program ASP.NET nie są wyświetlane komunikaty jego błąd standardowy. Zamiast tego można przechwytywać błąd w `catch` części bloku.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Zastąp odpowiednimi wartościami dla `your-SMTP-server-name`i tak dalej. Komunikaty o błędach, które można napotkać w ten sposób między innymi następujące:

- *Błąd podczas wysyłania poczty.*

    —lub—

    *Próba połączenia nie powiodła się, ponieważ strona połączenia nie odpowiada prawidłowo po upływie określonego czasu lub utworzone połączenie nie powiodło się, ponieważ połączony host nie odpowiada*

    Ten błąd oznacza, że aplikacji nie może połączyć się z serwerem SMTP. Sprawdź nazwę serwera i numer portu.
- *Skrzynka pocztowa jest niedostępna. Odpowiedź serwera: 5.1.0 &lt; someuser@invaliddomain &gt; nadawcy odrzucone: nieprawidłowy nadawcy domeny*

    Ten komunikat można wskazać, że `From` adres jest nieprawidłowy lub Brak.
- *Określony ciąg nie ma formy wymaganej dla adresu e-mail.*

    Ten błąd może oznaczać, że wartość `To` lub `From` właściwości nie są rozpoznawane jako adresy e-mail. (Aplikacja ASP.NET nie sprawdza, czy adres e-mail jest prawidłowy tylko jego obiektu w poprawnym formacie tak samo, jak  *name@domain.com* .)

> [!NOTE]
> Usuń kod znaczników, który zawiera błąd (`@errorMessage`) przed opublikowaniem strony w witrynie na żywo. Nie jest dobrym rozwiązaniem, aby umożliwić użytkownikom wyświetlić komunikaty o błędach, które można uzyskać z serwera.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000)

[Program WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum w witrynie sieci Web ASP.NET
