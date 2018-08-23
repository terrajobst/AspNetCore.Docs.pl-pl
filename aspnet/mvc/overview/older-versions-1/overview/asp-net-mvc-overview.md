---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC — omówienie | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się więcej o różnicach między aplikacją platformy ASP.NET MVC i aplikacji formularzy sieci Web programu ASP.NET. Dowiedz się, jak i decyzji dotyczącej tworzenia aplikacji ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 61a7841ee238ec365b7d1909221bbe3d834faf84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756556"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC — omówienie
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się więcej o różnicach między aplikacją platformy ASP.NET MVC i aplikacji formularzy sieci Web programu ASP.NET. Dowiedz się, jak i decyzji dotyczącej tworzenia aplikacji ASP.NET MVC.


Wzorzec architektury Model-View-Controller (MVC) dzieli aplikację na trzy główne składniki: model, widok i kontroler. Platforma ASP.NET MVC stanowi alternatywę dla wzorca ASP.NET Web Forms do tworzenia aplikacji sieci Web opartej na MVC. Platforma ASP.NET MVC to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejących funkcji programu ASP.NET, takich jak strony wzorcowe i uwierzytelnianie oparte na członkostwie. Struktura MVC jest zdefiniowana w **System.Web.Mvc** przestrzeni nazw i jest częścią podstawowych, obsługiwanych **System.Web** przestrzeni nazw.   
  
MVC to standardowy wzorzec projektowania, wielu programistów, którzy znają. Niektóre rodzaje aplikacji sieci Web będą mogli korzystać z struktura MVC. Inne osoby będą nadal korzystać z tradycyjnego wzorca aplikacji ASP.NET, która opiera się na formularzach sieci Web i odświeżaniu strony. Innych typów aplikacji sieci Web będą łączone dwa podejścia; nie wyklucza drugiego.   
  
Struktura MVC obejmuje następujące składniki:


[![Wywołanie akcji kontrolera, który oczekuje, że wartość parametru](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Rysunek 01**: wywołanie akcji kontrolera, który oczekuje, że wartość parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-overview/_static/image2.png))


- **Modele**. Obiekty modelu są częściami aplikacji, które implementują logikę domeny danych aplikacji s. Często obiekty modelu pobierania i przechowywania stanu modelu w bazie danych. Na przykład obiekt produktu mogą pobierać informacje z bazy danych, wykonywać na nich operacje i następnie zapisywać zaktualizowane informacje do tabeli Produkty w programie SQL Server.

W małych aplikacjach model jest często stanowi separację koncepcyjną zamiast fizycznej. Na przykład jeśli aplikacja wyłącznie odczytuje zestaw danych i wysyła je do widoku, aplikacja nie ma warstwy modelu fizycznego i skojarzonych klas. W takim przypadku zestaw danych przyjmuje rolę obiektu modelu.

- **Widoki**. Widoki są składnikami interfejsie użytkownika (UI) s aplikacji. Zazwyczaj interfejs ten jest tworzony w modelu danych. Przykładem może być widok edycji tabeli Produkty, który wyświetla pola tekstowe, listy rozwijane i pola wyboru bazujące na bieżącym stanie obiektu produktów.

- **Kontrolery**. Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika. W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji. Przykładowo kontroler obsługuje wartości ciągu zapytania i przekazuje te wartości do modelu, który z kolei wysyła zapytanie bazy danych przy użyciu wartości.

Wzorzec MVC pomaga w tworzeniu aplikacji, które rozdzielają różnych aspektów aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając tym luźne powiązanie tych elementów. Wzorzec Określa, gdzie każdy rodzaj logiki powinien znajdować się w aplikacji. Logika interfejsu użytkownika, należy w widoku. Logika danych wejściowych jest powiązana z kontrolerem. Logika biznesowa jest powiązana w modelu. Ta separacja ułatwia zarządzanie złożonością podczas tworzenia aplikacji, ponieważ pozwala skoncentrować się na jednym aspekcie implementacji w danym momencie. Na przykład można skoncentrować się na widoku bez polegania na logice biznesowej.   
  
Oprócz zarządzania złożonością wzorzec MVC ułatwia testowanie aplikacji niż można przetestować aplikację sieci Web platformy ASP.NET opartej na formularzach sieci Web. Na przykład w aplikacji sieci Web platformy ASP.NET opartej na formularzach sieci Web pojedyncza klasa jest używana zarówno do wyświetlania danych wyjściowych, jak i odpowiedzieć na dane wejściowe użytkownika. Pisanie zautomatyzowanych testów dla aplikacji platformy ASP.NET opartych na formularzach sieci Web może być skomplikowane, ponieważ na przetestowanie jednej strony, trzeba utworzyć wystąpienie klasy strony, wszystkich jej kontrolek podrzędnych i dodatkowych klas zależnych w aplikacji. Ponieważ tak wiele klas są tworzone, aby uruchomić stronę, może być trudno napisać testy skoncentrowane wyłącznie na pojedynczych częściach aplikacji. Testy dla aplikacji platformy ASP.NET opartych na formularzach sieci Web, w związku z tym może być trudniejsze do zaimplementowania niż testy aplikacji MVC. Ponadto testy w aplikacji platformy ASP.NET opartych na formularzach sieci Web wymagają serwera sieci Web. Struktura MVC rozłącza składniki i intensywnie korzysta z interfejsów, co umożliwia testowanie pojedynczych składników w izolacji od reszty struktury.   
  
Luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe. Na przykład jeden deweloper może pracować w widoku, drugi może pracować nad logiką kontrolera, a trzeci skupić się na logice biznesowej w modelu.

## <a name="deciding-when-to-create-an-mvc-application"></a>Podejmowanie decyzji o utworzeniu aplikacji MVC

Należy starannie rozważyć czy wdrożyć aplikację sieci Web za pomocą struktury ASP.NET MVC lub modelu formularzy sieci Web platformy ASP.NET. Struktura MVC nie zastępuje modelu formularzy sieci Web; dla aplikacji sieci Web, można użyć obu struktur. (Jeśli masz istniejące aplikacje oparte na formularzach sieci Web, będą działać dokładnie tak jak dotychczas.)   
  
Zanim zdecydujesz się używać struktura MVC, czy modelu formularzy sieci Web dla określonej witryny sieci Web, należy rozważyć zalety każde podejście.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji sieci Web opartej na MVC

Platforma ASP.NET MVC ma następujące zalety:

- Ułatwia ona zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerze. To sprawia, że struktura MVC idealne rozwiązanie dla deweloperów, którzy mają pełną kontrolę nad działaniem aplikacji.
- Używa wzorca kontrolera Frontowego, który przetwarza żądania aplikacji sieci Web za pośrednictwem jednego kontrolera. Dzięki temu można zaprojektować aplikację, która obsługuje złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [kontroler Frontowy](https://go.microsoft.com/fwlink/?LinkId=106357 "kontroler Frontowy") w witrynie MSDN w sieci Web.
- Zapewnia lepszą obsługę projektowania opartego na testach (TDD).
- Sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów witryn sieci Web, którzy potrzebują wysokiego stopnia kontroli nad działaniem aplikacji.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji sieci Web opartych na formularzach sieci Web

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje model zdarzeń zachowujący stan przez HTTP, co opracowywanie aplikacji sieci Web line-of-business. Aplikacja oparta na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Używa wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontrolera strony](https://go.microsoft.com/fwlink/?LinkId=106359 "kontrolera strony") w witrynie MSDN w sieci Web.
- Używa stanu widoku ani formularzy opartych na serwer, które ułatwia zarządzanie informacjami o stanie.
- Sprawdza się w przypadku małych zespołów deweloperów i projektantów, którzy chcą korzystać z zalet dużej liczby składników dostępnych do szybkiego opracowywania aplikacji internetowych.
- Ogólnie rzecz biorąc, jest mniej złożona przy projektowaniu aplikacji, ponieważ składniki ( **strony** klasy, formantów i tak dalej) są ściśle powiązane i zwykle wymagają mniejszej ilości kodu niż w modelu MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funkcje struktury ASP.NET MVC

Platforma ASP.NET MVC zawiera następujące funkcje:

- Rozdzielenie zadań aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), testowania i programowania sterowanego testami (TDD) domyślnie. Wszystkie główne kontrakty w strukturze MVC są oparte na interfejsie i może być przetestowany przy użyciu makiety obiektów, które są symulowanymi obiektami imitującymi zachowanie rzeczywistych obiektów w aplikacji. Użytkownik może testu jednostkowego aplikacji bez konieczności uruchamiania kontrolerów w procesie ASP.NET, co sprawia, że testowanie szybkich i elastycznych jednostek. Możesz użyć dowolnej platformy testów jednostkowych, która jest zgodna z .NET Framework.
- Rozszerzalna struktura obsługująca dodatki. Składniki struktury ASP.NET MVC zaprojektowano tak, aby można je łatwo zastępowane lub dostosować. Można dodać własny aparat widoku, zasady routingu adresów URL, serializację parametrów metod akcji i inne składniki. Platforma ASP.NET MVC obsługuje też korzystanie z modeli kontenera iniekcja zależności (DI) oraz Inwersja kontroli (IOC). DI pozwala na iniekcję obiektów do klasy zamiast polegania na klasę, aby obiekt utworzy sama. Inwersja kontroli określa, że w przypadku obiekt wymaga innego obiektu, pierwszy obiekt powinien pobrać drugi obiekt z zewnętrznego źródła, takiego jak plik konfiguracji. To ułatwia testowanie.
- Zaawansowane składnik Mapowanie adresu URL, który pozwala tworzyć aplikacje, które mają adresy URL zrozumiałe i którą można przeszukiwać. Adresy URL nie trzeba dołączać rozszerzenia nazwy pliku i są przeznaczone do obsługi wzorców nazewnictwa adresów URL, które działają dobrze w przypadku wyszukiwania aparat optymalizacji (SEO) i REST representational state transfer () adresowania.
- Obsługa przy użyciu znaczników w istniejącej strony ASP.NET (pliki aspx), kontrolki użytkownika (plikach ascx) i pliki znaczników (pliki .master) w usłudze master strony jako szablonów widoków. Możesz używać istniejące funkcje platformy ASP.NET za pomocą struktury ASP.NET MVC, takich jak zagnieżdżone strony wzorcowe, wbudowane wyrażenia (&lt;% = %&gt;), deklaratywne kontrolki serwerowe, szablony, wiązanie danych, lokalizacja i tak dalej.
- Obsługa funkcji programu ASP.NET. ASP.NET MVC umożliwia korzystanie z funkcji, takich jak uwierzytelnianie formularzy i uwierzytelniania Windows, Autoryzacja adresów URL, członkostwo i role, dane wyjściowe i buforowanie danych, zarządzania stanem sesji i profilu, monitorowanie kondycji, system konfiguracji i dostawcy Architektura.
