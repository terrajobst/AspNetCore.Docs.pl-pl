---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: "Rozwiązywanie problemów z HTTP 405 błędy po opublikowaniu interfejs API sieci Web 2 aplikacji | Dokumentacja firmy Microsoft"
author: rmcmurray
description: "Ten przewodnik opisuje sposób rozwiązywania problemów z błędami HTTP 405 po opublikowaniu aplikacji interfejsu API sieci Web, na serwerze sieci web w środowisku produkcyjnym."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 2455bbed891179466de8fb6ade3b0a8e66eadee6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Rozwiązywanie problemów z HTTP 405 błędy po opublikowaniu interfejs API sieci Web 2 aplikacji
====================
przez [Roberta Mcmurraya](https://github.com/rmcmurray)

> Ten przewodnik opisuje sposób rozwiązywania problemów z błędami HTTP 405 po opublikowaniu aplikacji interfejsu API sieci Web, na serwerze sieci web w środowisku produkcyjnym.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (w wersji 7 lub nowszy)
> - [Interfejs API sieci Web](../../index.md) (wersja 1 lub 2)


Aplikacje interfejsu API sieci Web zwykle używać kilka typowych zleceń HTTP: GET, POST, PUT, DELETE, a czasami poprawki. Trwa powiedział, deweloperzy mogą z systemem w sytuacji, w którym te zlecenia są implementowane przez inny moduł usług IIS na serwerze produkcyjnym, co prowadzi do sytuacji, gdy kontroler Web API, który działa prawidłowo w programie Visual Studio lub na serwerze projektowym zwróci HTTP 405 błąd, gdy jest wdrożony na serwerze produkcyjnym. Na szczęście jest łatwo rozwiązać ten problem, ale rozwiązanie gwarantuje wyjaśnienie przyczynę wystąpienia problemu.

## <a name="what-causes-http-405-errors"></a>Jakie HTTP 405 przyczyny błędów

Pierwszym krokiem procesu poznanie problemów błędów HTTP 405 jest zrozumienie, jakie błąd HTTP 405 oznacza w rzeczywistości. Podstawowy dotyczących dokumentu HTTP jest [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), definiujący kod stanu HTTP 405 jako ***metoda niedozwolona***i dokładniej opisuje ten kod stanu jako sytuację, gdzie &quot;— metoda określona w wierszu żądania nie jest dozwolony dla zasobu określonego przez identyfikator URI żądania.&quot; Innymi słowy zlecenie HTTP jest niedozwolona dla określonego adresu URL żądany przez klienta HTTP.

Jako krótki przegląd w tym miejscu jest kilka metod HTTP najczęściej używane zgodnie z definicją w dokumencie RFC 2616 RFC 4918 i RFC 5789:

| Metoda HTTP | Opis |
| --- | --- |
| **POBIERZ** | Ta metoda służy do pobierania danych z identyfikatorem URI który prawdopodobnie metoda HTTP najczęściej używane. |
| **HEAD** | Ta metoda jest podobne jak w przypadku metody GET, z wyjątkiem tego, że faktycznie nie pobierać dane z identyfikatora URI żądania — po prostu pobiera stan HTTP. |
| **POST** | Ta metoda jest zwykle używana do wysyłania nowe dane do identyfikatora URI; POST jest często używany do przesyłania danych formularza. |
| **UMIEŚĆ** | Ta metoda jest zwykle używana do danych pierwotnych do identyfikatora URI; PUT jest często używany do przesyłania danych JSON i XML do aplikacji interfejsu API sieci Web. |
| **USUŃ** | Ta metoda jest używana do usunięcia danych z identyfikatora URI. |
| **OPCJE** | Ta metoda jest zwykle używany można pobrać listy metod HTTP, które są obsługiwane dla identyfikatora URI. |
| **KOPIOWANIE PRZENOSZENIE** | Te dwie metody są używane z WebDAV, a ich celem jest oczywiste. |
| **MKCOL** | Ta metoda jest używana z WebDAV i jest używany do tworzenia kolekcji (np. katalog) na określony identyfikator URI. |
| **PROPFIND, PROPPATCH** | Te dwie metody są używane z WebDAV i są one używane do zapytań, lub ustawić właściwości identyfikatora URI. |
| **ODBLOKOWANIA BLOKADY** | Te dwie metody są używane z WebDAV i są one używane do Zablokuj/Odblokuj zasobu określonego przez identyfikator URI żądania, podczas tworzenia. |
| **POPRAWKI** | Ta metoda służy do modyfikowania istniejącego zasobu HTTP. |

Jedną z tych metod HTTP jest skonfigurowana do użycia na serwerze, serwer odpowie ze stanem HTTP i innych danych, który jest odpowiedni dla żądania. (Na przykład metoda GET może odbierać 200 protokołu HTTP ***OK*** odpowiedzi i metody PUT może odbierać 201 protokołu HTTP ***utworzony*** odpowiedzi.)

Jeśli metoda HTTP nie jest skonfigurowana do użycia na serwerze, z 501 HTTP będzie odpowiadać serwera ***nie zaimplementowano*** błędu.

Jednak gdy metoda HTTP jest skonfigurowana do użycia na serwerze, ale zostało wyłączone dla danego identyfikatora URI, serwer wysyła w odpowiedzi HTTP 405 ***metoda niedozwolona*** błędu.

## <a name="example-http-405-error"></a>Przykład HTTP 405 błąd

Następujący przykład HTTP żądania i odpowiedzi ilustrują sytuacji, w której klient HTTP próbuje umieścić wartość do aplikacji interfejsu API sieci Web na serwerze sieci web, a serwer zwraca błąd HTTP, które stany, które metody PUT nie jest dozwolone:


Żądania HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Odpowiedź HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


W tym przykładzie klient HTTP wysyłane prawidłowe żądania JSON do adresu URL dla aplikacji interfejsu API sieci Web na serwerze sieci web, ale serwer zwrócił komunikat o błędzie HTTP 405, co oznacza, że metody PUT nie jest dozwolone dla adresu URL. Natomiast jeśli identyfikator URI żądania jest niezgodny trasy dla aplikacji interfejsu API sieci Web, serwer zwróci HTTP 404 ***nie można odnaleźć*** błędu.

## <a name="resolving-http-405-errors"></a>Rozpoznawanie HTTP 405 błędów

Istnieje kilka przyczyn, dlaczego określonych zlecenie HTTP nie może być dozwolone, ale ma jednego podstawowego scenariusza, który jest wiodącym przyczyna tego błędu w usługach IIS: wielu obsług są zdefiniowane dla tego samego zlecenia/metody i jeden z programów obsługi blokuje programu obsługi oczekiwanego przetwarzanie żądania. I wyjaśnienie usługi IIS przetwarzają obsługi od pierwszego do ostatniego na podstawie kolejności obsługi wpisów w plikach applicationHost.config i web.config, gdzie pierwszy pasującego kombinacji ścieżki, zlecenie, zasobów itp., będzie służyć do obsługi żądania.

Poniższy przykład zawiera fragment pliku applicationHost.config dla serwera usług IIS, który został zwróciła błąd HTTP 405 podczas przesyłania danych do aplikacji interfejsu API sieci Web przy użyciu metody PUT. W tym fragmencie zdefiniowano kilka programów obsługi HTTP, a każdy program obsługi ma inny zestaw metod HTTP, dla których skonfigurowano — ostatni wpis na liście jest statyczny obsługi zawartości, który jest używany po innych programów obsługi miały chanc domyślny program obsługi e, aby sprawdzić żądanie:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

W powyższym przykładzie obsługi WebDAV i obsługi adresów URL bez rozszerzeń dla platformy ASP.NET (która jest używana dla interfejsu API sieci Web) są jasno określone dla oddzielne listy metod HTTP. Należy pamiętać, że programu obsługi ISAPI DLL jest skonfigurowany dla wszystkich metod HTTP, mimo że taka konfiguracja nie spowoduje niekoniecznie wystąpił błąd. Jednak ustawienia konfiguracji, takich jak ta należy wziąć pod uwagę podczas rozwiązywania problemów z błędami HTTP 405.

W powyższym przykładzie programu obsługi ISAPI DLL nie był problem; w rzeczywistości problem nie został zdefiniowany w pliku applicationHost.config serwera usług IIS — problem został spowodowany przez wpis, która została wprowadzona w pliku web.config, gdy aplikacja interfejsu API sieci Web została utworzona w programie Visual Studio. Poniższy fragment plik web.config zawiera lokalizację problemu:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

W tym fragmencie obsługi adresów URL bez rozszerzeń dla platformy ASP.NET jest ponownie zdefiniować uwzględnienie dodatkowych metod HTTP, które będą używane z aplikacji interfejsu API sieci Web. Jednak ponieważ podobny zestaw metod HTTP jest zdefiniowany dla obsługi WebDAV, występuje konflikt. W tym przypadku określone obsługi WebDAV jest zdefiniowana i załadowane przez usługi IIS, nawet jeśli WebDAV są wyłączone dla witryny sieci Web, która zawiera aplikację interfejsu API sieci Web. Podczas przetwarzania żądania HTTP PUT, usługi IIS wywołują moduł WebDAV, ponieważ jest ona zdefiniowana dla zlecenie PUT. Wywołanego modułu WebDAV sprawdza konfigurację i widzi, że jest ono wyłączone, dlatego zostanie zwrócona HTTP 405 ***metoda niedozwolona*** błąd każde żądanie podobny żądania WebDAV. Aby rozwiązać ten problem, należy usunąć WebDAV z listy modułów HTTP witryny sieci Web, w którym zdefiniowana jest aplikacja interfejsu API sieci Web. W poniższym przykładzie pokazano, co czy może wyglądać:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Ten scenariusz jest często napotkał po opublikowaniu aplikacji ze środowiska programowania do środowiska produkcyjnego i dzieje się tak dlatego listy programów obsługi/modułów różni się między środowiska projektowania i produkcji. Na przykład jeśli używasz programu Visual Studio 2012 lub 2013 umożliwiające tworzenie aplikacji interfejsu API sieci Web usług IIS Express 8 jest domyślnego serwera sieci web do testowania. Tego serwera wdrożeniowego sieci web jest wersją skalowany w dół pełną funkcjonalność usług IIS, który jest dostarczany w produkcie serwera, a ten serwer sieci web development zawiera kilka zmian, które zostały dodane do scenariuszy programowania. Na przykład moduł WebDAV często jest zainstalowany na serwerze sieci web produkcji, pełną wersją usług IIS, mimo że nie może być w praktyce. Wersji programu IIS (usługi IIS Express), instaluje moduł WebDAV, ale wpisy dla modułu WebDAV celowo są ujęte w komentarz, aby moduł WebDAV nigdy nie jest załadowane na serwer IIS Express, chyba że w szczególności zmiany konfiguracji usług IIS Express ustawienia, aby dodać funkcję WebDAV do instalacji usług IIS Express. W związku z tym aplikacji sieci web może działać poprawnie na komputerze deweloperskim, ale mogą wystąpić błędy HTTP 405 podczas publikowania aplikacji interfejsu API sieci Web do serwera sieci web w środowisku produkcyjnym.

## <a name="summary"></a>Podsumowanie

HTTP 405 błędy są spowodowane, gdy metoda HTTP nie jest dozwolone przez serwer sieci web dla żądanego adresu URL. Ten stan często występuje, podczas obsługi określonego został zdefiniowany dla określonego czasownika i zastępuje program obsługi, który może przetworzyć żądania programu obsługi.

W razie wystąpienia sytuacji, gdy pojawi się komunikat o błędzie HTTP 501, co oznacza, że na serwerze nie została zaimplementowana określonych funkcji, często oznacza, że istnieje bez obsługi zdefiniowany w ustawieniach usług IIS, które odpowiada na żądania HTTP, które Prawdopodobnie wskazuje, że element nie został poprawnie zainstalowany w systemie lub coś zmodyfikował ustawienia usług IIS, aby nie zostały nie programy obsługi zdefiniowane tej metody HTTP określonej pomocy technicznej. Aby rozwiązać ten problem, należy ponownie zainstalować dowolnej aplikacji, która próbuje użyć metody HTTP, dla którego go nie ma odpowiedniego modułu ani definicje programu obsługi.
