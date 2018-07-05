---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: Przetwarzanie nieobsługiwanych wyjątków (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web w środowisku produkcyjnym ważne jest, by powiadomić dewelopera i się błąd, dzięki czemu mogą być zdiagnozować w a la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 784290f1390668518ad88df9d38b60c0179a5923
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371859"
---
<a name="processing-unhandled-exceptions-c"></a>Przetwarzanie nieobsługiwanych wyjątków (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([sposobu pobierania](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web w środowisku produkcyjnym należy do powiadamiania dla deweloperów i się błąd, dzięki czemu mogą być zdiagnozować w dowolnym momencie w czasie. Ten samouczek zawiera omówienie sposobu ASP.NET przetwarza błędy w czasie wykonywania i patrzy na jednym ze sposobów niestandardowy kod wykonywanie zawsze wtedy, gdy bąbelki nieobsługiwanego wyjątku do środowiska uruchomieniowego programu ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Po wystąpieniu nieobsługiwanego wyjątku w aplikacji ASP.NET, będzie przechodzić do środowiska uruchomieniowego programu ASP.NET, który wywołuje `Error` zdarzeń i wyświetli stronę odpowiedni komunikat o błędzie. Istnieją trzy różne typy stron błędów: środowisko uruchomieniowe błąd żółty ekranu z śmierci (YSOD); Szczegóły wyjątku YSOD; i strony błędów niestandardowych. W [poprzedni Samouczek](displaying-a-custom-error-page-cs.md) skonfigurowaliśmy aplikacji do korzystania z niestandardowej strony błędu dla użytkowników zdalnych i YSOD szczegółów wyjątku dla użytkowników odwiedzających lokalnie.

Za pomocą przyjaznego dla człowieka niestandardowej strony błędu odpowiadający wygląd i działanie witryny jest preferowana domyślne YSOD błąd środowiska uruchomieniowego, ale Wyświetlanie niestandardowej strony błędu jest tylko jednej części kompleksową obsługę rozwiązania błędów. Po wystąpieniu błędu w aplikacji w środowisku produkcyjnym, ważne jest, że Deweloperzy są powiadamiani błędu, czemu mogą odkryć przyczynę wyjątku i rozwiązać problem. Ponadto szczegóły błędu powinny być rejestrowane, tak aby ten błąd można zbadać i zdiagnozować w dowolnym momencie w czasie.

W tym samouczku pokazano, jak uzyskiwać dostęp szczegóły nieobsługiwany wyjątek, dzięki czemu mogą one być rejestrowane i deweloperem powiadomienia. Błąd rejestrowania bibliotek, które, po znacznej liczby konfiguracji, spowoduje to automatyczne powiadomienia deweloperów błędy czasu wykonywania i rejestrować ich szczegóły zapoznaj się z dwóch samouczki, zgodnie z tego jednego.

> [!NOTE]
> Informacje w tym samouczku jest najbardziej użyteczna, jeśli potrzebujesz do przetwarzania nieobsługiwanych wyjątków w sposób unikatowy lub dostosowanych. W przypadkach, w których konieczne jest zarejestruje wyjątek i powiadamia dewelopera za pomocą biblioteki rejestrowanie błędów jest Zdajemy sobie. W dwóch następnych samouczków omówiono dwie biblioteki.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Wykonywanie kodu, gdy`Error`zdarzenie jest zgłaszane w

Zdarzenia mechanizm obiekt do sygnalizowania, że coś interesującego wystąpił oraz do innego obiektu wykonać kod w odpowiedzi. Jako deweloper platformy ASP.NET jest wyrażenie pod kątem zdarzeń. Do uruchomienia kodu po kliknięciu określonego przycisku, można utworzyć program obsługi zdarzeń dla tego przycisku `Click` zdarzeń i umieść kod istnieje. Biorąc pod uwagę, że środowisko uruchomieniowe ASP.NET zgłasza jego [ `Error` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) przy każdym wystąpieniu nieobsługiwanego wyjątku, wynika, że kod rejestrowanie szczegółów błędów przejdzie w obsłudze zdarzeń. Jak utworzyć moduł obsługi zdarzenia, ale `Error` zdarzeń?

`Error` Zdarzenia jest jednym z wielu zdarzeń w [ `HttpApplication` klasy](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , są inicjowane na określonym etapie w potoku HTTP w okresie istnienia żądania. Na przykład `HttpApplication` klasy [ `BeginRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) jest wywoływane na początku każdego żądania; jego [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) jest wywoływane, gdy moduł security zidentyfikowała osoby zgłaszającej żądanie. Te `HttpApplication` zdarzeń dawać developer strony oznacza, że do wykonania niestandardowej logiki w różnych punktach w okresie istnienia żądania.

Programy obsługi zdarzeń dla `HttpApplication` zdarzenia można umieścić w specjalnym pliku o nazwie `Global.asax`. Aby utworzyć ten plik w swojej witryny sieci Web, należy dodać nowy element do katalogu głównego witryny sieci Web przy użyciu szablonu globalna klasa aplikacji o nazwie `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Rysunek 1**: Dodaj `Global.asax` do aplikacji sieci Web  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image3.png))

Zawartość i struktura `Global.asax` plik utworzony przez program Visual Studio różni się nieco zależnie od tego, czy przy użyciu projektu aplikacji sieci Web (WAP) lub projektu witryny sieci Web (WSP). Z WAP `Global.asax` jest implementowany jako dwa osobne pliki - `Global.asax` i `Global.asax.cs`. `Global.asax` Zawierają plik, ale `@Application` dyrektywę, który odwołuje się do `.cs` plików; zdarzenie obsługi zainteresowania są zdefiniowane w `Global.asax.cs` pliku. Dla WSPs, jest tworzony tylko jeden plik, `Global.asax`, programy obsługi zdarzeń są one definiowane w `<script runat="server">` bloku.

`Global.asax` Pliku utworzonego w WAP przez szablon globalnej klasy aplikacji programu Visual Studio obejmuje obsługę zdarzeń o nazwie `Application_BeginRequest`, `Application_AuthenticateRequest`, i `Application_Error`, które są programy obsługi zdarzeń dla `HttpApplication` zdarzenia `BeginRequest`, `AuthenticateRequest`, i `Error`, odpowiednio. Dostępne są także procedury obsługi zdarzeń o nazwie `Application_Start`, `Session_Start`, `Application_End`, i `Session_End`, które są procedury obsługi zdarzeń wyzwalane, gdy aplikacja sieci web rozpoczyna się po uruchomieniu nowej sesji podczas kończenia aplikacji, a kiedy kończy się sesja odpowiednio. `Global.asax` Pliku utworzonego w WSP przez program Visual Studio zawiera po prostu `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, i `Session_End` procedury obsługi zdarzeń.

> [!NOTE]
> W przypadku wdrażania aplikacji ASP.NET należy skopiować `Global.asax` pliku do środowiska produkcyjnego. `Global.asax.cs` Pliku, który zostanie utworzony w WAP, nie trzeba można skopiować do środowiska produkcyjnego, ponieważ ten kod jest kompilowany do zestawu projektu.


Programy obsługi zdarzeń utworzonych przez szablon globalnej klasy aplikacji programu Visual Studio nie są wyczerpujące. Można dodać program obsługi zdarzeń dla każdego `HttpApplication` zdarzeń za pomocą nazw programu obsługi zdarzeń `Application_EventName`. Na przykład można dodać następujący kod, aby `Global.asax` pliku w celu utworzenia programu obsługi zdarzeń dla [ `AuthorizeRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Podobnie należy usunąć wszystkie procedury obsługi zdarzeń utworzonych przez szablon globalnej klasy aplikacji, które nie są wymagane. W tym samouczku tylko wymagamy, aby program obsługi zdarzeń dla `Error` zdarzeń; możesz usunąć inne procedury obsługi zdarzeń z `Global.asax` pliku.

> [!NOTE]
> *Moduły HTTP* oferują innym sposobem zdefiniowania procedury obsługi zdarzeń dla `HttpApplication` zdarzenia. Moduły HTTP są tworzone jako plik klasy, które mogą być umieszczane bezpośrednio w ramach projektu aplikacji sieci web lub oddzielone w bibliotece osobnej klasy. Ponieważ one być oddzielone w bibliotece klasy, moduły HTTP oferują bardziej elastyczne i wielokrotnego użytku modelu tworzenia `HttpApplication` procedury obsługi zdarzeń. Natomiast `Global.asax` zależy od plików do aplikacji sieci web, w którym znajduje się, można kompilować moduły HTTP do zestawów, w tym momencie Dodawanie modułu HTTP do witryny sieci Web jest tak proste, jak usunięcie zestawu `Bin` folder i rejestrowanie Moduł w `Web.config`. W tym samouczku nie znajduje się w tworzenie i używanie moduły HTTP, ale bibliotek dwóch rejestrowania błędów używanych w ramach następujących samouczków dwa są implementowane jako moduły HTTP. Aby uzyskać więcej ogólnych informacji o zaletach moduły HTTP dotyczą [modułów przy użyciu protokołu HTTP i procedury obsługi do tworzenia składników ASP.NET podłączanych](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Trwa pobieranie informacji na temat nieobsługiwany wyjątek

W tym momencie mamy plik Global.asax z `Application_Error` programu obsługi zdarzeń. Gdy ta procedura obsługi zdarzeń musimy powiadamia dewelopera błędu i zaloguj się jego szczegóły. Aby wykonać te zadania, najpierw należy określić szczegóły wyjątku, który został zgłoszony. Użyj obiektu serwera [ `GetLastError` metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) można pobrać szczegółów nieobsługiwany wyjątek, który spowodował `Error` wyzwolenie zdarzenia.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

`GetLastError` Metoda zwraca obiekt typu `Exception`, który jest typem podstawowym dla wszystkich wyjątków w programie .NET Framework. Jednak w powyższym kodzie I jestem rzutowanie zwrócona przez obiekt wyjątku `GetLastError` do `HttpException` obiektu. Jeśli `Error` jest jest wyzwalane zdarzenie, ponieważ wystąpił wyjątek podczas przetwarzania zasobu ASP.NET, a następnie opakowaniu wyjątek, który został zgłoszony w obrębie `HttpException`. Można pobrać faktyczny wyjątek, który wytrącane Użyj zdarzenie błędu `InnerException` właściwości. Jeśli `Error` zdarzenia zostało podniesione z powodu wyjątku oparty na protokole HTTP, takich jak żądanie strony nieistniejącej `HttpException` jest generowany, ale nie ma wyjątek wewnętrzny.

Poniższy kod używa `GetLastErrormessage` można pobrać informacji o wyjątku, który wyzwolił `Error` zdarzenia przechowywania `HttpException` w zmiennej o nazwie `lastErrorWrapper`. Następnie przechowuje typ, wiadomości i ślad stosu wyjątku źródłowy w trzech zmiennych ciągu w celu sprawdzania, jeśli `lastErrorWrapper` jest faktyczny wyjątek, który wyzwolił `Error` zdarzenie (w przypadku wyjątków opartych na protokole HTTP) lub jeśli jest to jedynie Otoka dla wyjątku zgłoszonego podczas przetwarzania żądania.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

W tym momencie masz wszystkie informacje potrzebne do pisania kodu, który będzie rejestrować szczegóły wyjątku do tabeli bazy danych. Można utworzyć z kolumnami tabeli bazy danych dla każdego szczegóły błędu zainteresowania — typ, wiadomości, ślad stosu i tak dalej — wraz z części przydatne informacje, takie jak adres URL żądanej strony i nazwę aktualnie zalogowanego użytkownika. W `Application_Error` programu obsługi zdarzeń, czy połączenia z bazą danych i wstawienia rekordu do tabeli. Podobnie można dodać kod do wyzwalania alertu, deweloper błąd za pośrednictwem poczty e-mail.

Biblioteki rejestrowania błędów w dwóch następnych samouczków oferuje takie gotowych, więc nie ma konieczności tworzenia tego rejestrowania błędów i powiadomień. Jednak aby zilustrować, że `Error` jest wywoływane zdarzenie i `Application_Error` program obsługi zdarzeń można rejestrować szczegóły błędu i powiadamia dewelopera, możemy dodać kod, który powiadamia dewelopera po wystąpieniu błędu.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Powiadomienie dla deweloperów, po wystąpieniu nieobsługiwanego wyjątku

Po wystąpieniu nieobsługiwanego wyjątku w środowisku produkcyjnym należy do wyzwalania alertu zespół programistyczny, tak aby ocenić błędu i określenia, jakie działania należy podjąć. Na przykład w przypadku wystąpił błąd podczas łączenia z bazą danych, a następnie konieczne będzie double Sprawdź parametry połączenia i, otwórz bilet pomocy technicznej przy użyciu hostingu firmy w sieci web. Jeśli wyjątek wystąpił z powodu błędu programowania, dodatkowy kod lub logikę weryfikacji może być konieczne do dodania do uniknąć takich błędów w przyszłości.

.NET Framework klas w [ `System.Net.Mail` przestrzeni nazw](https://msdn.microsoft.com/library/system.net.mail.aspx) ułatwiają Wyślij wiadomość e-mail. [ `MailMessage` Klasy](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) reprezentuje wiadomości e-mail i ma właściwości, takie jak `To`, `From`, `Subject`, `Body`, i `Attachments`. `SmtpClass` Służy do wysyłania `MailMessage` przy użyciu określonego serwera SMTP; ustawienia serwera SMTP można określić programowo i deklaratywnie w [ `<system.net>` elementu](https://msdn.microsoft.com/library/6484zdc1.aspx) w `Web.config file`. Aby uzyskać więcej informacji na temat wysyłania wiadomości e-mail wiadomości w aplikacji ASP.NET zapoznaj się z moich artykułu [wysyłania wiadomości E-mail w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)i [System.Net.Mail — często zadawane pytania](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Element zawiera ustawienia serwera SMTP, które są używane przez `SmtpClient` klasy podczas wysyłania wiadomości e-mail. Firmy mogą hostingu w sieci web ma serwera SMTP, który służy do wysyłania wiadomości e-mail z aplikacji. Informacje na temat ustawień serwera SMTP, którego należy używać w aplikacji sieci web na ten temat można znaleźć w sekcji Obsługa hosta sieci web.


Dodaj następujący kod do `Application_Error` program obsługi zdarzeń, aby wysłać wiadomość e-mail dla deweloperów, gdy wystąpi błąd:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Powyższy kod jest dość długich, duża część jej tworzy kod HTML, który pojawia się w wiadomości e-mail wysyłanej do deweloperów. Zaczyna się kod, odwołując się do `HttpException` zwrócone przez `GetLastError` — metoda (`lastErrorWrapper`). Faktyczny wyjątek, który został zgłoszony przez żądanie jest pobierany za pomocą `lastErrorWrapper.InnerException` i jest przypisany do zmiennej `lastError`. Typ, wiadomości i stos informacje o śledzeniu są pobierane z `lastError` i przechowywane w trzech zmiennych ciągu.

Następnie `MailMessage` obiektu o nazwie `mm` zostanie utworzony. Treść wiadomości e-mail jest w formacie HTML i wyświetla adres URL żądanej strony, nazwa aktualnie zalogowanego użytkownika, a informacje o wyjątku (typ, wiadomości i ślad stosu). Jedną z najfajniejszych rzeczy podczas `HttpException` klasa jest generowanie kodu HTML, użyty do utworzenia wyjątków szczegóły żółty ekranu z śmierci (YSOD) przez wywołanie metody [metoda GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Ta metoda jest używana w tym miejscu można pobrać znaczników YSOD szczegółów wyjątku i dodaj go do wiadomości e-mail jako załącznik. Jeden wyraz Uwaga: Jeśli wyjątek, wyzwolone `Error` zdarzeń został wyjątek oparty na protokole HTTP (na przykład żądanie dla strony nieistniejącej), a następnie `GetHtmlErrorMessage` metoda zwróci `null`.

Ostatnim krokiem jest wysyłanie `MailMessage`. Jest to realizowane przez utworzenie nowego `SmtpClient` metody i wywoływania jego `Send` metody.

> [!NOTE]
> Przed rozpoczęciem korzystania z tego kodu w aplikacji sieci web można zmienić wartości na `ToAddress` i `FromAddress` stałe od support@example.com do dowolnych wiadomości e-mail adres wiadomość e-mail z powiadomieniem błędu powinny być przesyłane do i pochodzą z. Należy także określić ustawienia serwera SMTP w `<system.net>` sekcji `Web.config`. Zapoznaj się z dostawcą hosta sieci web, aby określić ustawienia serwera SMTP do użycia.


Przy użyciu tego kodu w miejscu przy każdym wystąpieniu błędu dewelopera jest wysyłana wiadomość e-mail zawiera podsumowanie błędu i zawiera YSOD. W poprzednim samouczku przedstawiono firma Microsoft błąd w czasie wykonywania, odwiedzając Genre.aspx i przekazując nieprawidłowy `ID` wartości za pomocą ciągu kwerendy, takie jak `Genre.aspx?ID=foo`. Odwiedź stronę z `Global.asax` miejscowej plików tworzy się tego samego środowiska użytkownika, jak w poprzednim samouczku — w środowisku programistycznym nadal będzie się wyjątek szczegóły żółty ekranem śmierci, podczas gdy w środowisku produkcyjnym należy zobacz stronę błędu niestandardowego. Oprócz tego istniejącego zachowania deweloper jest wysyłana wiadomość e-mail.

**Rysunek 2** pokazuje wiadomości e-mail odbieranych, kiedy odwiedzający `Genre.aspx?ID=foo`. Treść wiadomości e-mail zawiera podsumowanie informacji o wyjątku, gdy `YSOD.htm` załącznika wyświetla zawartość, która jest wyświetlana w YSOD szczegóły wyjątku (zobacz **rysunek 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Rysunek 2**: dewelopera jest wysyłana wiadomość E-mail z powiadomieniem, zawsze wtedy, gdy wystąpił nieobsługiwany wyjątek  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Rysunek 3**: powiadomienie E-mail zawiera szczegóły wyjątku YSOD jako załącznik  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Co przy użyciu strony błędu niestandardowego?

W tym samouczku pokazano, jak używać `Global.asax` i `Application_Error` programu obsługi zdarzeń w celu wykonania kodu, po wystąpieniu nieobsługiwanego wyjątku. W szczególności użyliśmy ta procedura obsługi zdarzeń, aby powiadomić deweloperem błąd; Firma Microsoft może rozszerzyć, można również rejestrować szczegóły błędu w bazie danych. Obecność `Application_Error` program obsługi zdarzeń nie ma wpływu na środowisko użytkownika końcowego. Nadal widzą strony błędów skonfigurowany, YSOD szczegóły błędu, YSOD błąd środowiska uruchomieniowego lub stronę błędu niestandardowego.

Naturalna, aby dowiedzieć się, czy `Global.asax` pliku i `Application_Error` zdarzeń jest konieczne w przypadku, gdy za pomocą niestandardowej strony błędu. Gdy wystąpi błąd, użytkownik jest wyświetlana strona błędu niestandardowego więc Dlaczego nie umieściliśmy kod, aby powiadomić projektanta i rejestrować szczegóły błędów w klasie CodeBehind strony błędu niestandardowego? Gdy bez obaw można dodać kod do klasy CodeBehind stronę błędu niestandardowego nie masz dostęp do szczegółów wyjątku, który wyzwolił `Error` zdarzenie, kiedy korzystające z techniki rozważyliśmy w poprzednim samouczku. Wywoływanie `GetLastError` metoda ze strony błędu niestandardowego zwraca `Nothing`.

Przyczyna to zachowanie jest, ponieważ osiągnięto za pośrednictwem przekierowanie strony błędu niestandardowego. Gdy nieobsługiwany wyjątek osiągnie środowiska uruchomieniowego ASP.NET aparatu ASP.NET zgłasza jego `Error` zdarzeń (które wykonuje `Application_Error` programu obsługi zdarzeń) i następnie *przekierowuje* użytkownika do strony błędu niestandardowego, wysyłając `Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Metoda wysyła odpowiedź do klienta z kodem stanu HTTP 302, poinstruowanie przeglądarkę w celu zażądania nowego adresu URL, czyli stronę błędu niestandardowego. Przeglądarka następnie automatycznie żąda tej nowej strony. Stwierdzić, że strona błędu niestandardowego zażądano oddzielnie ze strony skąd pochodzi ten błąd, ponieważ zmiany paska adresu w przeglądarce adres URL strony błędu niestandardowego (zobacz **rysunek 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Rysunek 4**: Jeśli wystąpi błąd, przeglądarka jest przekierowywany do adresu URL strony błędu niestandardowego  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](processing-unhandled-exceptions-cs/_static/image12.png))

Net powoduje, że żądanie, w którym wystąpił nieobsługiwany wyjątek kończy się, gdy serwer odpowiada przy użyciu przekierowania HTTP 302. Kolejne żądanie strony błędu niestandardowego jest całkiem nowe wezwanie; na tym etapie ASP.NET aparatu odrzucił informacje o błędzie, a ponadto nie ma możliwości skojarzenia nieobsługiwany wyjątek w poprzednie żądanie z nowe żądanie dla strony błędu niestandardowego. Dlatego `GetLastError` zwraca `null` wywołanego ze strony błędu niestandardowego.

Jednak jest możliwe stronę błędu niestandardowego wykonanych podczas tego samego żądania, które spowodowały błąd. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) Metoda przenosi wykonanie do określonego adresu URL i przetwarza je w ramach tego samego żądania. Można przenieść kod w `Application_Error` programu obsługi zdarzeń do klasy związane z kodem stronę błędu niestandardowego, zastępując je w `Global.asax` następującym kodem:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Teraz, gdy wystąpił nieobsługiwany wyjątek wystąpi `Application_Error` program obsługi zdarzeń przekazuje sterowanie do odpowiednich niestandardowej strony błędu w oparciu o kod stanu HTTP. Ponieważ sterowania zostało przeniesione, stronę błędu niestandardowego ma dostęp do informacji o nieobsługiwany wyjątek za pomocą `Server.GetLastError` i powiadamia dewelopera błędu i rejestrować jego szczegóły. `Server.Transfer` Przestanie aparatu ASP.NET z przekierowanie użytkownika do strony błędu niestandardowego. Zamiast tego należy zawartość strony błędu niestandardowego jest zwracany jako odpowiedzi do strony, który wygenerował błąd.

## <a name="summary"></a>Podsumowanie

Po wystąpieniu nieobsługiwanego wyjątku, w aplikacji sieci web ASP.NET środowiska uruchomieniowego ASP.NET zgłasza `Error` zdarzeń i zostanie wyświetlona strona błędu skonfigurowany. Powiadomimy Deweloper błędu, zaloguj się jego szczegóły lub przetwarzać dane w dowolny sposób, tworząc program obsługi zdarzeń dla zdarzenia błędów. Istnieją dwa sposoby, aby utworzyć program obsługi zdarzeń dla `HttpApplication` zdarzenia, takie jak `Error`: w `Global.asax` pliku lub moduł protokołu HTTP. W tym samouczku pokazano, jak utworzyć `Error` programu obsługi zdarzeń w `Global.asax` pliku, który informuje deweloperów o błąd za pomocą wiadomości e-mail.

Tworzenie `Error` procedura obsługi zdarzeń jest przydatne, jeśli potrzebujesz do przetwarzania nieobsługiwanych wyjątków w sposób unikatowy lub dostosowane. Jednak tworzyć własne `Error` program obsługi zdarzeń, który zarejestruje wyjątek lub powiadamiania deweloper nie jest najbardziej efektywny sposób wykorzystać czas, ponieważ istnieją już bezpłatny i łatwy w użyciu Błąd rejestrowania bibliotek, które można skonfigurować w ciągu kilku minut. W dwóch następnych samouczków zbadać dwie biblioteki.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Moduły HTTP platformy ASP.NET oraz omówienie obsługi protokołu HTTP](https://support.microsoft.com/kb/307985)
- [Bez problemu zmieniała odpowiadanie na nieobsługiwane wyjątki — przetwarzanie nieobsługiwanych wyjątków](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Klasa i obiekt aplikacji ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Funkcje obsługi protokołu HTTP i moduły HTTP w programie ASP.NET:](http://www.15seconds.com/Issue/020417.htm)
- [Wysyłanie wiadomości E-mail w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Opis `Global.asax` pliku](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Tworzenie składników podłączanych ASP.NET przy użyciu moduły HTTP do programów obsługi](https://msdn.microsoft.com/library/aa479332.aspx)
- [Praca z ASP.NET `Global.asax` pliku](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Praca z `HttpApplication` wystąpień](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Poprzednie](displaying-a-custom-error-page-cs.md)
> [dalej](logging-error-details-with-asp-net-health-monitoring-cs.md)
