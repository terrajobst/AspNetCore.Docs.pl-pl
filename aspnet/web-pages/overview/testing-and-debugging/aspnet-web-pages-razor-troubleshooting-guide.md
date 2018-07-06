---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) przewodnik rozwiązywania problemów | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) oraz sugerowane rozwiązania. Wersje oprogramowania strona początkowa sieci Web ASP.NET...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 0d0e47ce1b52f0e9ac23a44bbd37ac1a38290dbd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825166"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) przewodnik rozwiązywania problemów
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) oraz sugerowane rozwiązania.
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 i stron sieci Web platformy ASP.NET w wersji 1.0.


Ten temat zawiera następujące sekcje:

- [Problemy z uruchamianiem stron](#Issues_Running_.cshtml_Pages)
- [Problemy z kodem Razor](#IssuesWithRazorCode)
- [Problemy dotyczące zabezpieczeń i członkostwa](#membership)
- [Problemy z wysyłaniem wiadomości E-mail](#email)
- [Dodatkowe zasoby](#AdditionalResources)

Pytania ogólne, można zobaczyć [stron ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemy z uruchamianiem stron

Różne problemy mogą uniemożliwić *.cshtml* i *.vbhtml* stron z prawidłowo uruchomiona. Ta sekcja zawiera typowe komunikaty o błędach i prawdopodobne przyczyny.

### <a name="http-error-403---forbidden-access-is-denied"></a>Błąd HTTP 403 — Dostęp zabroniony: Odmowa dostępu

*Nie masz uprawnień do wyświetlenia tego katalogu lub strony przy użyciu podanych poświadczeń.*

Ten błąd może wystąpić, jeśli serwer nie działa poprawną wersję programu .NET Framework. Upewnij się, że komputera, na którym uruchomiono serwer (lokalnie lub zdalnie) co najmniej ma zainstalowany program .NET Framework 4. Upewnij się, że sama aplikacja jest skonfigurowany do uruchamiania właściwą wersję również.

Jeśli widzisz ten problem z lokalnie podczas pracy w programie WebMatrix, kliknij pozycję **witryny** obszaru roboczego, a następnie w widoku drzewa kliknij **ustawienia**. W **wybierz wersję systemu .NET Framework** , wybierz na liście **.NET 4 (Integrated)**. Jeśli ta wersja jest już ustawiony, spróbuj uruchomić program WebMatrix jako administrator.

Upewnij się, że katalog główny witryny sieci Web ma co najmniej jeden *.cshtml* plik.

Jeśli zostanie wyświetlony ten błąd, gdy serwer sieci web znajduje się na serwerze zdalnym, skontaktuj się z administratorem serwera. Upewnij się, że serwer ma programu .NET Framework 4 lub nowszy zainstalowany. Upewnij się, że aplikacja jest uruchomiona w puli aplikacji, który jest skonfigurowany do korzystania z tej wersji systemu.NET Framework również.

Jeśli masz kontrolę nad serwer, upewnij się, że działa on poprawną wersję programu .NET Framework. Możesz również spróbować naprawić instalację, uruchamiając `aspnet_regiis -iru` polecenia. (Na przykład po zainstalowaniu usług IIS po zainstalowaniu programu .NET Framework, usługi IIS nie zostanie poprawnie skonfigurowany do uruchomienia strony ASP.NET.) Aby uzyskać więcej informacji, zobacz [narzędzie rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Błąd HTTP 403.14 — dostęp zabroniony

*Serwer sieci Web jest skonfigurowany, aby nie wyświetlać listę zawartości tego katalogu.*

Ten błąd może wystąpić, jeśli żądanie jest zasobem, który jest chroniony (takich jak *Web.config* plików) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).

### <a name="http-error-40417---not-found"></a>Błąd HTTP 404.17 — nie znaleziono

*Żądana zawartość wydaje się być skryptu i będzie nie być obsługiwana przez program obsługi plików statycznych.*

Ten błąd może wystąpić, jeśli serwer nie jest prawidłowo skonfigurowany do użycia programu .NET Framework 4 lub nowszym i w związku z tym nie może rozpoznać kodu w `@{ }` bloków. Zobacz opis wcześniej *HTTP błąd 403 — Dostęp zabroniony: odmowa dostępu*.

### <a name="http-error-4047---not-found"></a>Błąd HTTP 404.7 — nie znaleziono

*Moduł filtrowania żądań skonfigurowano odmowę rozszerzenie pliku*

Ten błąd może wystąpić, jeśli *.cshtml* lub *.vbhtml* rozszerzenia zostały jawnie zablokowane na serwerze. Objawem tego problemu jest działające adresy URL, gdy nie obejmują one rozszerzenie, ale adresy URL, które obejmują *.cshtml* lub *.vbhtml* nie działają. Możliwe rozwiązanie to aby ponownie włączyć rozszerzenia w tej witrynie *Web.config* pliku. Poniższy przykład pokazuje, jak włączyć *.cshtml* rozszerzenia.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Błąd HTTP 404.8 — nie znaleziono

*Moduł filtrowania żądań skonfigurowano odmowę ścieżki w adresie URL, który zawiera sekcja hiddenSegment.*

Ten błąd może wystąpić, jeśli żądanie jest zasobem, który jest chroniony (takich jak *Web.config* plików) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Ten typ strony nie jest obsługiwany (błąd serwera w aplikacji» /»)

Zobacz opis wcześniej 404.17 Błąd HTTP.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemy z kodem Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Nazwa "*klasy*" nie istnieje w bieżącym kontekście

Często przyczyny wystąpienia tego błędu jest to, że `class` odwołania pomocnika, ale pomocnika nie jest zainstalowany. Na przykład Jeśli spróbujesz użyć pomocnika, ale nie został jeszcze zainstalowany pakiet z pakietów NuGet, zostanie wyświetlony ten błąd. Galerii w programie WebMatrix można użyć, aby znaleźć i zainstalować pomocnika.

Jeśli zainstalowano pomocnika, ale strona nadal nie rozpoznaje ich, spróbuj dodać dodać `using` instrukcji w kodzie. W `using` instrukcji, odwołanie do przestrzeni nazw, który zawiera pomocnika. Na przykład podstawowa pomocników, znajdujących się w pakiecie ASP.NET pomocnicy znajdują się w `System.Web.Helpers` przestrzeni nazw. W górnej części strony, której chcesz użyć pomocnika Dodaj następujący wiersz:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemy dotyczące zabezpieczeń i członkostwa

Jeśli używasz systemu wbudowanych rozwiązań zabezpieczeń (Członkowskimi) w składniku ASP.NET Web Pages (Razor), można napotkać następujące problemy.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Aby wywołać tę metodę, właściwość "Membership.Provider" musi być wystąpieniem elementu "ExtendedMembershipProvider"

Ten błąd może wskazywać, że nie `AspNetSqlMembershipProvider` klasa jest skonfigurowana. (Symptomem jest czy lokacji działa prawidłowo lokalnie, ale zgłasza błąd podczas publikowania do dostawcy hostingu serwera). Jeden rozwiązać ten problem jest jawnie włączyć członkostwo proste przez dodanie poniższego w witrynie *Web.config* pliku:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemy z wysyłaniem wiadomości E-mail

Problemy z wysyłaniem wiadomości e-mail może być wyzwaniem do debugowania. Początkowego problemu może być, że nie można nawiązać połączenia z serwerem SMTP. Jeśli połączenie zostanie nawiązane, ASP.NET przekazywało wiadomość do serwera SMTP. Jednakże mogą wystąpić problemy z komunikatem sam, który uniemożliwia wysłanie go do serwera SMTP.

Jeśli aplikacja nie pomyślnie wysłać wiadomości e-mail, spróbuj wykonać następujące czynności:

- Nazwa serwera SMTP jest często podobny `smtp.provider.com` lub `smtp.provider.net`. Jeśli jednak publikowania witryny dostawcy usług hostingowych, nazwę serwera SMTP w tym momencie może być `localhost`. Ta sytuacja występuje, ponieważ po opublikowaniu i witryna jest hostowana na serwerze dostawcy, serwer SMTP, może być lokalne z perspektywy aplikacji. Ta zmiana nazwy serwera może oznaczać, że trzeba zmienić nazwę serwera SMTP w ramach procesu publikowania.
- Numer portu jest zwykle 25. Jednakże niektórzy dostawcy wymagają użycia portu 587 lub niektórych innych portów. Skontaktuj się z właścicielem serwera SMTP, w jakiego numeru portu spełniają oczekiwane do użycia.
- Upewnij się, że używasz właściwych poświadczeń dostępu. Po opublikowaniu witryny dostawcy usług hostingowych, Użyj poświadczeń z dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail. Te poświadczenia mogą się różnić od poświadczenia, których używasz do publikowania.
- Czasami nie potrzebujesz poświadczeń w ogóle. Przy wysyłaniu wiadomości e-mail przy użyciu osobistych usługodawcę internetowego, dostawcy poczty e-mail może już znasz swoje poświadczenia. Po opublikowaniu, może być konieczne użycie innych poświadczeń niż podczas testowania na komputerze lokalnym.
- Jeśli dostawcy poczty e-mail używa szyfrowania, ustaw `WebMail.EnableSsl` do `true`.

Występuje błąd podczas wysyłania wiadomości e-mail, może zostać wyświetlony komunikat o błędzie standardowa ASP.NET, która wygląda następująco:

![Komunikat o błędzie programu ASP.NET, gdy występuje problem z adresem e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Można również debugować problemy z wysyłaniem wiadomości e-mail przy użyciu `try-catch` bloku, jak w poniższym przykładzie. Kiedy używasz `try-catch` bloku, program ASP.NET nie wyświetla jego standardowy błąd wiadomości. Zamiast tego można przechwycić błąd w `catch` części bloku.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Zastąp odpowiednie wartości dla `your-SMTP-server-name`i tak dalej. Komunikaty o błędach, które można napotkać w ten sposób między innymi następujące:

- *Błąd podczas wysyłania wiadomości e-mail.*

    —lub—

    *Próba połączenia nie powiodło się, ponieważ strona połączona nie odpowiedziała poprawnie po określonym czasie albo ustanowione połączenie nie powiodło się, ponieważ połączony host nie odpowiada*

    Ten błąd zazwyczaj oznacza, że aplikacja nie może połączyć się z serwerem SMTP. Sprawdź nazwę serwera i numer portu.
- <em>Skrzynka pocztowa jest niedostępna. Odpowiedź serwera: 5.1.0 &lt; someuser@invaliddomain &gt; nadawcy odrzucone: nieprawidłowy nadawca domeny</em>

    Ten komunikat można wskazać, że `From` adres jest nieprawidłowy lub Brak.
- *Określony ciąg nie jest w formularzu wymaganych dla adresu e-mail.*

    Ten błąd może wskazywać, że wartość `To` lub `From` właściwości nie są rozpoznawane jako adresy e-mail. (ASP.NET nie może sprawdzić, czy adres e-mail jest prawidłowy, tylko jej użytkownika w prawidłowym formacie jak *name@domain.com*.)

> [!NOTE]
> Usuń kod znaczników, który powoduje wyświetlenie błędu (`@errorMessage`), przed opublikowaniem strony w działającej witrynie. Nie jest dobry pomysł, aby umożliwić użytkownikom wyświetlić komunikaty o błędach, które można uzyskać z serwera.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000)

[Program WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum w witrynie sieci Web platformy ASP.NET
