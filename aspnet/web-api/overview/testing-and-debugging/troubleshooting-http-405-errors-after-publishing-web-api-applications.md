---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Rozwiązywanie problemów z HTTP 405 błędy po opublikowaniu interfejsu Web API 2 aplikacji | Dokumentacja firmy Microsoft
author: rmcmurray
description: W tym samouczku opisano, jak rozwiązywać problemy z błędami HTTP 405 po opublikowaniu aplikacji interfejsu API sieci Web, na serwerze sieci web w środowisku produkcyjnym.
ms.author: riande
ms.date: 05/01/2014
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 735b8ceeafa63e0546529ef17f103070dc760794
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754584"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Rozwiązywanie problemów z HTTP 405 błędy po opublikowaniu interfejsu Web API 2 aplikacji
====================
przez [Robert McMurray](https://github.com/rmcmurray)

> W tym samouczku opisano, jak rozwiązywać problemy z błędami HTTP 405 po opublikowaniu aplikacji interfejsu API sieci Web, na serwerze sieci web w środowisku produkcyjnym.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (w wersji 7 lub nowszy)
> - [Interfejs API sieci Web](../../index.md) (wersja 1 lub 2)


Aplikacje interfejsu API sieci Web zazwyczaj używają kilka typowych zleceń HTTP: GET, POST, PUT, DELETE, a czasami PATCH. Po uwzględnieniu deweloperzy mogą napotkać sytuacje, gdzie te polecenia są implementowane przez inny moduł usług IIS na serwerze produkcyjnym, co prowadzi do sytuacji, w których będzie zwracać kontroler internetowego interfejsu API, który działa prawidłowo w programie Visual Studio lub na serwerze rozwoju HTTP 405 błąd, gdy aplikacja jest wdrożona na serwerze produkcyjnym. Na szczęście jest łatwo rozwiązać ten problem, ale rozwiązanie gwarantuje wyjaśnienie, dlaczego występuje błąd.

## <a name="what-causes-http-405-errors"></a>Jakie HTTP 405 przyczyny błędów

Pierwszym krokiem procesu nauki problemów z błędami HTTP 405 jest zrozumienie, jakie błąd HTTP 405 oznacza w rzeczywistości. Podstawowy regulujące dokumentów dla protokołu HTTP jest [dokumencie RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), definiujący kod stanu HTTP 405 jako ***niedozwolona metoda***i dalej w tym artykule opisano ten kod stanu jako sytuację, której &quot;metody określone w wierszu żądania nie jest dozwolona dla zasobu określonego przez identyfikator URI żądania.&quot; Innymi słowy czasownik HTTP nie jest dozwolona dla określonych adresów URL, który zgłosił żądanie klienta HTTP.

Jako krótki przegląd poniżej przedstawiono niektóre z najczęściej używanych metod HTTP zgodnie z definicją w dokumencie RFC 2616 RFC 4918 i RFC 5789:

| Metoda HTTP | Opis |
| --- | --- |
| **POBIERZ** | Ta metoda jest używana do pobierania danych z identyfikatora URI który prawdopodobnie metoda HTTP najczęściej używanych. |
| **GŁÓWNY** | Ta metoda jest podobne do metody GET, z tą różnicą, że faktycznie nie pobierać dane z identyfikatora URI żądania — po prostu pobiera stan HTTP. |
| **WPIS** | Ta metoda jest zwykle używana do wysyłania nowych danych do identyfikatora URI; WPIS jest często używane do wysyłania danych formularza. |
| **PUT** | Ta metoda jest zwykle używana do wysyłania danych pierwotnych do identyfikatora URI; PUT jest często używane do przesyłania danych JSON lub XML do aplikacji interfejsu API sieci Web. |
| **USUŃ** | Ta metoda jest używana w celu usunięcia danych z identyfikatora URI. |
| **OPCJE** | Ta metoda jest zwykle używana do pobrania listy metod HTTP, które są obsługiwane dla identyfikatora URI. |
| **KOPIOWANIE PRZENOSZENIE** | Te dwie metody są używane z WebDAV, a ich celem jest oczywista. |
| **MKCOL** | Ta metoda jest używana z WebDAV i jest używany do tworzenia kolekcji (np. katalogu) na określony identyfikator URI. |
| **PROPFIND PROPPATCH** | Te dwie metody są używane z WebDAV i są one używane do zapytań lub ustawić właściwości dla identyfikatora URI. |
| **ODBLOKOWANIE BLOKADY** | Te dwie metody są używane z WebDAV i są one używane do Zablokowanie/odblokowanie zasobu określonego przez identyfikator URI żądania, podczas tworzenia. |
| **POPRAWKI** | Ta metoda jest używana do modyfikowania istniejącego zasobu HTTP. |

Gdy jeden z tych metod HTTP jest skonfigurowany do użycia na serwerze, serwer będzie odpowiadać ze stanem HTTP i innych danych, które jest odpowiednie dla żądania. (Na przykład metoda GET może zostać wyświetlony protokołu HTTP 200 ***OK*** odpowiedzi i metodę PUT może pojawić się 201 protokołu HTTP ***utworzono*** odpowiedzi.)

Jeśli metoda HTTP nie jest skonfigurowany do użycia na serwerze, serwer odpowiada przy użyciu protokołu HTTP 501 ***nie zaimplementowano*** błędu.

Jednak gdy metoda HTTP jest skonfigurowana do użycia na serwerze, ale została ona wyłączona dla danego identyfikatora URI, serwer wysyła w odpowiedzi HTTP 405 ***niedozwolona metoda*** błędu.

## <a name="example-http-405-error"></a>Przykład HTTP 405 błąd

Następujące przykładowe żądanie HTTP i odpowiedzi przedstawiają sytuacji, w którym klient HTTP próbuje umieścić wartości do aplikacji interfejsu API sieci Web na serwerze sieci web, a serwer zwraca błąd HTTP, która stanów, które metody PUT nie jest dozwolona:


Żądania HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Odpowiedź HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


W tym przykładzie klienta HTTP wysyłane prawidłowemu żądaniu JSON do adresu URL dla aplikacji interfejsu API sieci Web na serwerze sieci web, ale serwer zwrócił komunikat o błędzie HTTP 405, co oznacza, że metody PUT nie mógł dla adresu URL. Natomiast jeśli identyfikator URI żądania nie był zgodny trasę dla aplikacji interfejsu API sieci Web, serwer zwraca błąd HTTP 404 ***nie można odnaleźć*** błędu.

## <a name="resolving-http-405-errors"></a>Rozpoznawanie HTTP 405 błędy

Istnieje kilka powodów dlaczego określone zlecenie HTTP może nie być dozwolone, ale istnieje jeden podstawowy scenariusz, który jest wiodącym przyczynę tego błędu, w usługach IIS: wielu obsług są zdefiniowane dla tej samej zlecenie/metody i jeden z elementów obsługi blokuje oczekiwanego programu obsługi na podstawie przetwarzanie żądania. Za pomocą wyjaśnienie usługi IIS przetwarzają obsługi od pierwszego do ostatniego na podstawie kolejności obsługi wpisów w plikach applicationHost.config i pliku web.config, użycia pierwszego dopasowania kombinacji ścieżki, zlecenie, zasobów itp., aby obsłużyć żądanie.

Poniższy przykład jest fragment pliku applicationHost.config w zakresie serwera usług IIS, który został zwróci błąd HTTP 405, korzystając z metody PUT przesyłania danych do aplikacji interfejsu API sieci Web. W tym fragmencie zdefiniowano kilka programów obsługi HTTP, a każdy program obsługi ma inny zestaw metod HTTP, dla których skonfigurowano — ostatni wpis na liście jest statyczny obsługi zawartości, który jest domyślny program obsługi, który jest używany po innych programów obsługi mieli chanc e, aby sprawdzić żądanie:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

W powyższym przykładzie obsługi WebDAV i obsługi adresu URL bez rozszerzeń dla programu ASP.NET (która jest używana dla internetowego interfejsu API) są jasno określone dla osobne listy metod HTTP. Należy pamiętać, że programu obsługi ISAPI DLL jest skonfigurowany dla wszystkich metod HTTP, mimo że ta konfiguracja będzie powoduje błąd. Jednak ustawienia konfiguracji, takich jak należy uwzględnić podczas rozwiązywania problemów z błędami HTTP 405.

W powyższym przykładzie programu obsługi ISAPI DLL nie był problem; w rzeczywistości problem nie został zdefiniowany w pliku applicationHost.config dla serwera IIS — problem został spowodowany przez wpis, który został wykonany w pliku web.config, podczas tworzenia aplikacji interfejsu API sieci Web w programie Visual Studio. Poniższy fragment z pliku web.config aplikacji przedstawia lokalizację problemu:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

W tym fragmencie obsługi adresu URL bez rozszerzeń dla programu ASP.NET zostało przedefiniowane uwzględnienie dodatkowych metod HTTP, które będą używane w aplikacji interfejsu API sieci Web. Jednak ponieważ podobny zestaw metod HTTP jest zdefiniowany dla programu obsługi WebDAV, występuje konflikt. W tym konkretnym przypadku obsługi funkcji WebDAV jest zdefiniowana i załadowany przez usługi IIS, nawet jeśli funkcja WebDAV jest wyłączona dla witryny sieci Web, które obejmuje aplikację interfejsu API sieci Web. Podczas przetwarzania żądania HTTP PUT, usługi IIS wywołują moduł WebDAV, ponieważ jest on zdefiniowany dla czasownika PUT. Gdy moduł WebDAV jest wywoływana, sprawdza, czy jego konfiguracja i widzi, że jest ono wyłączone, dlatego zostanie zwrócona, HTTP 405 ***niedozwolona metoda*** błąd dla każdego żądania, podobny do żądań WebDAV. Aby rozwiązać ten problem, należy usunąć WebDAV z listy moduły HTTP witryny sieci Web, w którym definiowany jest aplikacja interfejsu API sieci Web. Poniższy przykład pokazuje, jakie, może wyglądać:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

W tym scenariuszu występuje często, po opublikowaniu aplikacji z poziomu środowiska projektowego w środowisku produkcyjnym i jest to spowodowane listy programów obsługi/modułów różni się między środowisk deweloperskich i produkcyjnych. Na przykład jeśli używasz programu Visual Studio 2012 lub 2013 do tworzenia aplikacji interfejsu API sieci Web usług IIS Express 8 jest domyślnego serwera sieci web do testowania. Tego serwera wdrożeniowego sieci web jest skalowane w dół wersję pełną funkcjonalność usług IIS, który jest dostarczany w produkcie serwera, a ten serwera wdrożeniowego sieci web zawiera kilka zmian, które zostały dodane do scenariuszy programowania. Na przykład moduł WebDAV często jest zainstalowany na serwerze sieci web w środowisku produkcyjnym jest pełną wersją programu IIS, mimo że nie może być rzeczywistego użycia. Wersji deweloperskiej internetowych usług informacyjnych (IIS Express), zainstaluje moduł WebDAV, ale wpisy dla modułu WebDAV są celowo komentarzami, dlatego moduł WebDAV nigdy nie jest załadowany w usługach IIS Express, chyba że specjalnie zmienić konfigurację usług IIS Express ustawienia, aby dodać funkcję WebDAV do instalacji usług IIS Express. W rezultacie aplikację sieci web mogą działać poprawnie na komputerze deweloperskim, ale mogą wystąpić błędy HTTP 405 po opublikowaniu aplikacji interfejsu API sieci Web na serwerze sieci web w środowisku produkcyjnym.

## <a name="summary"></a>Podsumowanie

HTTP 405 błędy powstają, gdy metoda HTTP nie jest dozwolona przez serwer sieci web dla żądanego adresu URL. Ten warunek występuje często, określonej procedury obsługi został zdefiniowany dla określonego czasownika, gdy ten program obsługi zastępują program obsługi, który może przetworzyć żądania.

Jeśli napotkasz sytuację, w którym pojawi się komunikat o błędzie HTTP 501, co oznacza, że nie została zaimplementowana określonych funkcji na serwerze, często oznacza, że istnieje żadna procedura obsługi zdefiniowane w ustawieniach usług IIS, która odpowiada na żądania HTTP, który Prawdopodobnie wskazuje, że coś nie został poprawnie zainstalowany na komputerze lub coś zmodyfikował ustawienia usług IIS, aby nie programy obsługi zdefiniowane tej metody obsługi określonych HTTP. Aby rozwiązać ten problem, należy ponownie zainstalować każdą aplikację, która próbuje użyć metodę HTTP, dla którego go nie ma odpowiedniego modułu ani definicje programu obsługi.
