---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: "Przetwarzania nieobsługiwanych wyjątków (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web w środowisku produkcyjnym ważne jest, by powiadomić dewelopera i błąd logowania, dzięki czemu mogą być zdiagnozowany na a la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c7f15053ca035a1df1222f88752b8243808bef0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="processing-unhandled-exceptions-c"></a>Przetwarzania nieobsługiwanych wyjątków (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_cs.pdf)

> Gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web w środowisku produkcyjnym należy powiadomić dewelopera i Dziennik ten błąd może być zdiagnozowany w późniejszym czasie w czasie. Ten samouczek zawiera omówienie sposobu ASP.NET przetwarza błędy podczas wykonywania i analizuje jednym ze sposobów niestandardowy kod wykonania zawsze, gdy dymki nieobsługiwany wyjątek do środowiska wykonawczego programu ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET, propaguje go do środowiska uruchomieniowego ASP.NET, który wywołuje `Error` zdarzeń i zostanie wyświetlona strona błędu odpowiednie. Istnieją trzy różne typy stron błędów: środowisko uruchomieniowe błąd żółty ekranu z śmierci (YSOD); Szczegóły wyjątku YSOD; i strony błędów niestandardowych. W [poprzedniego samouczek](displaying-a-custom-error-page-cs.md) został skonfigurowany aplikacji na używanie niestandardowej strony błędu dla użytkowników zdalnych i YSOD szczegóły wyjątku dla użytkowników odwiedzających lokalnie.

Przy użyciu strony błędu niestandardowego przyjaznych dla człowieka odpowiadający wyglądu i działania witryny jest preferowana domyślne YSOD błąd środowiska uruchomieniowego, ale strona błędu niestandardowego jest tylko jedna część kompleksowe rozwiązanie do obsługi błędu. Po wystąpieniu błędu w aplikacji w środowisku produkcyjnym, jest ważne, że Deweloperzy są powiadamiani o błędu, aby mogli unearth przyczyną wyjątku i rozwiąż go. Ponadto szczegóły błędu powinny być rejestrowane, dzięki czemu błędu można zbadać i zdiagnozować w późniejszym czasie, w czasie.

Ten samouczek pokazuje, jak dostęp do szczegółów nieobsługiwany wyjątek, dzięki czemu mogą być rejestrowane i dewelopera powiadomienie. Dwa samouczki, ta po Poznaj Błąd rejestrowania bibliotek, które, po nieco konfigurację, automatycznie powiadamiać deweloperzy błędy podczas wykonywania i zaloguj się ich szczegóły.

> [!NOTE]
> Informacje przedstawione w tym samouczku jest najbardziej przydatna do przetwarzania nieobsługiwanych wyjątków w sposób unikatowy lub dostosowane. W przypadkach, gdzie wystarczy zarejestruje wyjątek i powiadom dewelopera przy użyciu błąd podczas rejestrowania biblioteki jest sposób, aby przejść. Następne dwa samouczki zawierają omówienie dwóch takich bibliotek.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Wykonywanie kodu, gdy`Error`zdarzenia

Zdarzenia mechanizm obiektu sygnalizowania, że coś interesującego wystąpił, a innym obiektem, aby wykonać kod w odpowiedzi. Deweloper ASP.NET użytkownik jest przyzwyczajony do planowania pod względem zdarzenia. Jeśli chcesz uruchomić kod po kliknięciu określonego przycisku Utwórz program obsługi zdarzeń dla tego przycisku `Click` zdarzeń i umieszczono kodu. Biorąc pod uwagę, że moduł wykonawczy platformy ASP.NET zgłasza jego [ `Error` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx) zawsze, gdy wystąpi nieobsługiwany wyjątek, wynika, że kod rejestrowania szczegóły błędu przejdzie w obsłudze zdarzeń. Jak utworzyć programu obsługi zdarzeń dla, ale `Error` zdarzenia?

`Error` Zdarzeń jest jedną z wielu zdarzeń w [ `HttpApplication` klasy](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) który pojawienia się na określonym etapie w potoku HTTP przez cały okres istnienia żądania. Na przykład `HttpApplication` klasy [ `BeginRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx) jest wywoływane na początku każdego żądania; jego [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx) jest wywoływane, gdy moduł zabezpieczeń zidentyfikował żądającego. Te `HttpApplication` zdarzenia zapewniają developer strony sposób wykonania niestandardowej logiki w różnych punktach w okresie istnienia żądanie.

Programy obsługi zdarzeń dla `HttpApplication` zdarzeń można umieścić w specjalnym pliku o nazwie `Global.asax`. Aby utworzyć ten plik w witrynie sieci Web, Dodaj nowy element do katalogu głównego witryny sieci Web przy użyciu szablonu globalnej klasy aplikacji o nazwie `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Rysunek 1**: Dodaj `Global.asax` do aplikacji sieci Web  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image3.png))

Zawartość i struktura `Global.asax` plik utworzony przez program Visual Studio różni się nieco zależności od tego, czy za pomocą projektu aplikacji sieci Web (WAP) lub projekt witryny sieci Web (WSP). Z WAP `Global.asax` jest zaimplementowany jako dwa osobne pliki - `Global.asax` i `Global.asax.cs`. `Global.asax` Zawiera plik, ale `@Application` dyrektywy, który odwołuje się do `.cs` plików; zdarzenia obsługi odsetek są zdefiniowane w `Global.asax.cs` pliku. Dla WSPs, tworzony jest tylko jeden plik, `Global.asax`, i obsługi zdarzeń są definiowane w `<script runat="server">` bloku.

`Global.asax` Plików utworzonych w WAP przez szablon globalnej klasy aplikacji Visual Studio zawiera procedury obsługi zdarzeń o nazwie `Application_BeginRequest`, `Application_AuthenticateRequest`, i `Application_Error`, które są programy obsługi zdarzeń dla `HttpApplication` zdarzenia `BeginRequest`, `AuthenticateRequest`, i `Error`odpowiednio. Dostępne są także procedury obsługi zdarzeń o nazwie `Application_Start`, `Session_Start`, `Application_End`, i `Session_End`, które są programy obsługi zdarzeń, które uruchamiają się po aplikacji sieci web rozpoczyna się po uruchomieniu nowej sesji, podczas kończenia aplikacji, a podczas kończenia sesji odpowiednio. `Global.asax` Pliku utworzonego w WSP przez program Visual Studio zawiera po prostu `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, i `Session_End` procedury obsługi zdarzeń.

> [!NOTE]
> W przypadku wdrażania aplikacji ASP.NET należy skopiować `Global.asax` pliku do środowiska produkcyjnego. `Global.asax.cs` Pliku, który jest tworzony w WAP, nie trzeba można skopiować do środowiska produkcyjnego, ponieważ ten kod jest kompilowany do zestawu projektu.


Programy obsługi zdarzeń utworzony przez szablon globalnej klasy aplikacji Visual Studio nie są wyczerpujące. Można dodać obsługi zdarzeń dla każdego `HttpApplication` zdarzeń za pomocą nazw programu obsługi zdarzeń `Application_EventName`. Można na przykład, Dodaj następujący kod, aby `Global.asax` pliku w celu utworzenia programu obsługi zdarzeń dla [ `AuthorizeRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-cs/samples/sample1.vb)]

Podobnie należy usunąć wszystkie obsługi zdarzeń utworzony przez szablon globalnej klasy aplikacji, które nie są wymagane. W tym samouczku wymagamy tylko program obsługi zdarzeń dla `Error` zdarzeń; działanie do usunięcia innych programów obsługi zdarzeń z `Global.asax` pliku.

> [!NOTE]
> *Moduły HTTP* oferują inny sposób definiowania obsługi zdarzeń `HttpApplication` zdarzenia. Moduły HTTP są tworzone jako plik klasy, które mogą być umieszczane bezpośrednio w ramach projektu aplikacji sieci web lub oddzielone do biblioteki osobnej klasy. Ponieważ one być oddzielone w bibliotece klas, moduły HTTP oferować bardziej elastyczne i wielokrotnego użytku modelu do tworzenia `HttpApplication` procedury obsługi zdarzeń. Podczas gdy `Global.asax` dotyczy pliku do aplikacji sieci web, w której znajduje się, moduły HTTP mogą być kompilowane do zestawów w takim przypadku Dodawanie moduł HTTP do witryny sieci Web jest tak proste, jak usunięcie zestawu `Bin` folderu i rejestrowanie Moduł w `Web.config`. W tym samouczku nie wygląda na tworzenie i używanie modułów HTTP, ale bibliotek dwóch rejestrowania błędów używane w następujących dwóch samouczki są zaimplementowane jako moduły HTTP. Tło więcej o zaletach moduły HTTP można znaleźć w temacie [modułów przy użyciu protokołu HTTP i dojścia do tworzenia składników ASP.NET podłączany](https://msdn.microsoft.com/en-us/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Trwa pobieranie informacji na temat nieobsługiwany wyjątek

W tym momencie mamy pliku Global.asax z `Application_Error` obsługi zdarzeń. Gdy wykonuje ten program obsługi zdarzeń musimy powiadomić dewelopera błędu i dziennika jego szczegóły. Aby wykonać te zadania, najpierw należy określić szczegóły wyjątku, który został zgłoszony. Obiekt serwera [ `GetLastError` metody](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.getlasterror.aspx) można pobrać szczegółów nieobsługiwany wyjątek, który spowodował `Error` zdarzenia.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

`GetLastError` Metoda zwraca wartość typu obiektu `Exception`, który jest typem podstawowym wszystkie wyjątki w programie .NET Framework. Jednak w powyższym kodzie I am rzutowanie obiektu wyjątku zwróconego przez `GetLastError` do `HttpException` obiektu. Jeśli `Error` zdarzenia jest uruchamiane, ponieważ wystąpił wyjątek podczas przetwarzania zasobu ASP.NET, a następnie został zgłoszony wyjątek jest ujęte w `HttpException`. Uzyskanie faktyczny wyjątek, który wytrącane Użyj zdarzenie błędu `InnerException` właściwości. Jeśli `Error` wywołano zdarzenie z powodu wyjątku oparte na protokole HTTP, np. żądań dotyczących stronę nieistniejącą `HttpException` jest zgłaszany, ale nie ma wyjątek wewnętrzny.

Poniższy kod używa `GetLastErrormessage` można pobrać informacji o wyjątku, który wywołał `Error` zdarzeń, przechowywanie `HttpException` w zmiennej o nazwie `lastErrorWrapper`. Następnie przechowuje typu, wiadomości i ślad stosu wyjątku pochodzących w trzech zmiennych ciągu w celu sprawdzania, jeśli `lastErrorWrapper` jest faktyczny wyjątek, który wywołał `Error` zdarzenie (w przypadku oparte na protokole HTTP wyjątkami) lub czy jest jedynie Otoka dla wyjątek został zgłoszony podczas przetwarzania żądania.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

W tym momencie masz wszystkie informacje potrzebne do pisania kodu, który będzie rejestrować szczegóły wyjątku do tabeli bazy danych. Dla każdej istotnej — typ, wiadomości, ślad stosu i tak dalej — wraz z innych części przydatne informacje, takie jak adres URL żądanej strony i nazwę aktualnie zalogowanego użytkownika, szczegóły błędu, można utworzyć tabeli bazy danych z kolumnami. W `Application_Error` obsługi zdarzeń, czy połączenia z bazą danych i wstawienia rekordu do tabeli. Podobnie można dodać kod do alertów dewelopera błąd za pośrednictwem poczty e-mail.

Biblioteki rejestrowania błędów w dwóch następnych samouczki Podaj takich funkcji fabrycznej, więc nie ma konieczności tworzenia tego rejestrowania błędów i powiadomień. Jednak aby zilustrować, który `Error` są zdarzenia, a `Application_Error` obsługi zdarzeń może służyć do logowania szczegóły błędu i powiadamiać dewelopera, możemy dodać kod, który powiadamia dewelopera, po wystąpieniu błędu.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Powiadamianie dewelopera, gdy wystąpi nieobsługiwany wyjątek

Gdy wystąpi nieobsługiwany wyjątek w środowisku produkcyjnym należy alertów zespół deweloperów, dzięki czemu mogą ocenić błąd i ustalić, jakie działania należy podjąć. Na przykład w przypadku wystąpił błąd podczas łączenia z bazą danych, konieczne będzie dwa razy, a następnie sprawdź ciąg połączenia i, prawdopodobnie, otwórz bilet pomocy technicznej z hostingu firmy w sieci web. Jeśli wyjątek wystąpił z powodu błędu programowania, dodatkowy kod lub logikę weryfikacji może być konieczne można dodać, aby zapobiec takie błędy w przyszłości.

Klasy programu .NET Framework w [ `System.Net.Mail` przestrzeni nazw](https://msdn.microsoft.com/en-us/library/system.net.mail.aspx) ułatwiają wysyłanie wiadomości e-mail. [ `MailMessage` Klasy](https://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx) reprezentuje wiadomości e-mail i ma właściwości, takie jak `To`, `From`, `Subject`, `Body`, i `Attachments`. `SmtpClass` Służy do wysyłania `MailMessage` przy użyciu określonego serwera SMTP; ustawienia serwera SMTP można określić programowo i deklaratywnie w [ `<system.net>` elementu](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx) w `Web.config file`. Aby uzyskać więcej informacji na wysyłanie wiadomości e-mail wiadomości w aplikacji ASP.NET, zapoznaj się z mojej artykułu [wysyłania poczty E-mail w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)i [System.Net.Mail — często zadawane pytania](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Element zawiera ustawienia serwera SMTP używany przez `SmtpClient` klasy podczas wysyłania wiadomości e-mail. Firmy prawdopodobną hostingu w sieci web ma serwer SMTP, który służy do wysyłania wiadomości e-mail z aplikacji. Informacje na temat ustawień serwera SMTP, które powinny być używane w aplikacji sieci web na ten temat można znaleźć w sekcji Obsługa hosta sieci web.


Dodaj następujący kod do `Application_Error` obsługi zdarzeń, aby wysłać wiadomość e-mail dewelopera, po wystąpieniu błędu:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Gdy powyższy kod jest bardzo długi, zbiorczego jego tworzy kod HTML, który pojawi się w wiadomości e-mail wysyłane do projektanta. Uruchamia kod za pomocą odwołań do `HttpException` zwrócony przez `GetLastError` — metoda (`lastErrorWrapper`). Faktyczny wyjątek, który został zgłoszony przez żądanie jest pobierana za pośrednictwem `lastErrorWrapper.InnerException` i jest przypisany do zmiennej `lastError`. Typ komunikatu i stosu informacje śledzenia są pobierane z `lastError` i przechowywane w trzech zmiennych ciągu.

Następnie `MailMessage` obiektu o nazwie `mm` jest tworzony. Treść wiadomości e-mail jest w formacie HTML i wyświetla adres URL żądanej strony, nazwę aktualnie zalogowanego użytkownika oraz informacje o wyjątku (typ, wiadomości i ślad stosu). Jednym z elementów chłodnych o `HttpException` jest klasa istnieje możliwość wygenerowania HTML używany do tworzenia wyjątek szczegóły żółty ekranu z śmierci (YSOD) przez wywołanie metody [GetHtmlErrorMessage — metoda](https://msdn.microsoft.com/en-us/library/system.web.httpexception.gethtmlerrormessage.aspx). Ta metoda służy tutaj, aby pobrać znaczników YSOD szczegóły wyjątku i dodaj go jako załącznik wiadomości e-mail. Ostrzeżenie o jedno słowo: Jeśli wyjątek który wyzwolone `Error` zdarzeń wyjątek oparte na protokole HTTP (np. żądań dotyczących strony nieistniejącą), a następnie `GetHtmlErrorMessage` metoda zwróci `null`.

Ostatnim krokiem jest wysłanie `MailMessage`. Odbywa się przez utworzenie nowej `SmtpClient` — metoda i wywoływanie jej `Send` metody.

> [!NOTE]
> Przed użyciem tego kodu w aplikacji sieci web można zmienić wartości w `ToAddress` i `FromAddress` stałe z support@example.com do dowolnego adresu e-mail wiadomości e-mail z powiadomieniem o mają być wysyłane do i pochodzą z. Należy również określić ustawienia serwera SMTP w `<system.net>` sekcji `Web.config`. Zapoznaj się z dostawcą hosta sieci web, aby określić ustawienia serwera SMTP do użycia.


Kod w miejscu przy każdym wystąpieniu błędu dewelopera jest wysyłana wiadomość e-mail, znajduje się podsumowanie błąd, który obejmuje YSOD. W poprzednim samouczka przedstawiono firma Microsoft błąd w czasie wykonywania odwiedzając Genre.aspx i przekazując nieprawidłowy `ID` wartości za pośrednictwem ciąg zapytania, takie jak `Genre.aspx?ID=foo`. Odwiedzania strony z `Global.asax` miejscowej pliku tworzy tego samego środowiska użytkownika, jak w poprzednim samouczek — w środowisku programistycznym nastąpi przejście do zobacz wyjątek szczegóły żółty ekranem śmierci, podczas gdy w środowisku produkcyjnym należy odwiedź stronę błędu niestandardowego. Oprócz tego zachowanie istniejącej dewelopera jest wysyłana wiadomość e-mail.

**Rysunek 2** pokazuje wiadomości e-mail podczas odwiedzania `Genre.aspx?ID=foo`. Treść wiadomości e-mail zawiera podsumowanie informacji o wyjątkach, podczas gdy `YSOD.htm` załącznika wyświetla zawartość, która jest wyświetlana w YSOD szczegóły wyjątku (zobacz **rysunek 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Rysunek 2**: dewelopera jest wysyłana wiadomość E-Mail z powiadomieniem, zawsze, gdy jest nieobsługiwany wyjątek  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Rysunek 3**: powiadomienie E-Mail zawiera szczegóły wyjątku YSOD jako załącznik  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Co zrobić za pomocą strony błędu niestandardowego?

W tym samouczku przedstawiono sposób użycia `Global.asax` i `Application_Error` obsługi zdarzeń do wykonywania kodu, gdy wystąpi nieobsługiwany wyjątek. W szczególności użyliśmy ten program obsługi zdarzeń do powiadamiania dewelopera błąd; Firma Microsoft może rozszerzyć również logowania szczegóły błędu w bazie danych. Obecność `Application_Error` program obsługi zdarzeń nie ma wpływu na środowisko użytkownika końcowego. Wciąż widzą strony błędów skonfigurowany, YSOD szczegóły błędu, YSOD błąd środowiska uruchomieniowego i stronę błędu niestandardowego.

Naturalna staje się zastanawiasz się, czy `Global.asax` pliku i `Application_Error` zdarzenie jest niezbędne, używając strony błędu niestandardowego. Jeśli wystąpi błąd, użytkownik jest wyświetlana strona błędu niestandardowego tak Dlaczego nie testujemy kod, aby powiadomić projektanta i dziennika szczegóły błędu w klasie związanej z kodem strony błędu niestandardowego? Gdy na pewno można dodać kod do klasy związane z kodem strony błędu niestandardowego nie masz dostęp do szczegółów wyjątku, który wywołał `Error` zdarzenie, gdy przy użyciu techniki możemy przedstawione w poprzednich instrukcji. Wywoływanie `GetLastError` metoda ze strony błędu niestandardowego zwraca `Nothing`.

Przyczyna to zachowanie jest ponieważ osiągnięto za pomocą przekierowania strony błędu niestandardowego. Gdy wystąpił nieobsługiwany wyjątek osiągnie środowiska uruchomieniowego ASP.NET aparatu ASP.NET zgłasza jego `Error` zdarzenia (który wykonuje `Application_Error` obsługi zdarzeń), a następnie *przekierowuje* użytkownika do strony błędu niestandardowego, wysyłając `Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Metoda wysyła odpowiedź do klienta z kodem stanu HTTP 302 poinstruowanie przeglądarkę, aby nowy adres URL, czyli strony błędu niestandardowego. Przeglądarka następnie automatycznie żąda nowej strony. Można określić, że strony błędu niestandardowego zażądano oddzielnie ze strony którego pochodzi błąd, ponieważ zmiany paska adresu w przeglądarce adres URL strony błędu niestandardowego (zobacz **4 rysunek**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Rysunek 4**: Jeśli wystąpi błąd przeglądarki jest kierowany do adresu URL strony błędu niestandardowego  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image12.png))

Net powoduje, że serwer odpowiadający przy użyciu przekierowania HTTP 302 kończy się żądanie, w którym wystąpił nieobsługiwany wyjątek. Kolejne żądania do strony błędu niestandardowego jest całkowicie nowe żądanie; w tym punkcie ASP.NET aparat odrzucił informacje o błędzie, a ponadto nie ma możliwości skojarzenia nieobsługiwany wyjątek w wcześniejsze żądanie z nowego żądania strony błędu niestandardowego. Jest to dlaczego `GetLastError` zwraca `null` wywołanego ze strony błędu niestandardowego.

Jednak jest możliwe wykonywanego w jednym żądaniu powodujący błąd strony błędu niestandardowego. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.transfer.aspx) Metody przenosi wykonanie do określonego adresu URL i przetwarza je w jednym żądaniu. Można przenieść kod w `Application_Error` obsługi zdarzeń do klasy związane z kodem strony błędu niestandardowego, zastępując je w `Global.asax` następującym kodem:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Teraz, gdy wystąpił nieobsługiwany wyjątek wystąpi `Application_Error` obsługi zdarzeń przekazuje sterowanie do odpowiednich niestandardowej strony błędu na podstawie kodu stanu HTTP. Ponieważ sterowania zostało przeniesione, stronę błędu niestandardowego ma dostęp do informacji nieobsługiwany wyjątek za pośrednictwem `Server.GetLastError` i powiadomić dewelopera błędu i dziennika można jego szczegóły. `Server.Transfer` Przestanie aparatu ASP.NET z przekierowanie użytkownika do strony błędu niestandardowego. Zamiast tego zawartość strony błędu niestandardowego jest zwracana jako odpowiedzi do strony, który wygenerował błąd.

## <a name="summary"></a>Podsumowanie

Gdy wystąpi nieobsługiwany wyjątek w aplikacji sieci web ASP.NET środowiska uruchomieniowego ASP.NET zgłasza `Error` zdarzeń i zostanie wyświetlona strona błędu skonfigurowany. Mamy powiadamiać developer błędu, jego szczegóły logowania lub przetwarzać dane w dowolny sposób, tworząc program obsługi zdarzeń dla zdarzenia błędu. Istnieją dwa sposoby tworzenia programu obsługi zdarzeń dla `HttpApplication` zdarzeń, takich jak `Error`: w `Global.asax` pliku lub modułu HTTP. W tym samouczku przedstawiono sposób tworzenia `Error` obsługi zdarzeń w `Global.asax` pliku, który powiadamia deweloperzy błąd za pomocą wiadomości e-mail.

Tworzenie `Error` procedura obsługi zdarzeń jest przydatna do przetwarzania nieobsługiwanych wyjątków w sposób unikatowy lub dostosowane. Jednak tworzenia własnego `Error` obsługi zdarzeń, który zarejestruje wyjątek lub powiadamiania Projektant nie jest najbardziej efektywne wykorzystanie czasu, ponieważ istnieją już bezpłatny i łatwy w użyciu Błąd rejestrowania bibliotek, które można skonfigurować w ciągu kilku minut. Następne dwa samouczki Sprawdź, czy dwie takie biblioteki.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie obsługi HTTP i moduły HTTP platformy ASP.NET](https://support.microsoft.com/kb/307985)
- [Bezpiecznie odpowiada na żądania nieobsługiwanych wyjątków - przetwarzania nieobsługiwanych wyjątków](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Klasa i obiekt aplikacji ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Programy obsługi HTTP i modułów HTTP w programie ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Wysyłanie wiadomości E-mail w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Opis `Global.asax` pliku](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Tworzenie składników ASP.NET podłączany przy użyciu moduły HTTP i obsługi](https://msdn.microsoft.com/en-us/library/aa479332.aspx)
- [Praca z platformy ASP.NET `Global.asax` pliku](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Praca z `HttpApplication` wystąpień](https://msdn.microsoft.com/en-us/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Poprzednie](displaying-a-custom-error-page-cs.md)
[dalej](logging-error-details-with-asp-net-health-monitoring-cs.md)
