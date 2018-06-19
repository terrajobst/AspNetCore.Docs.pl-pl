---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC — omówienie | Dokumentacja firmy Microsoft
author: microsoft
description: Więcej informacji na temat różnic między aplikacji ASP.NET MVC i formularzy sieci Web ASP.NET aplikacji. Informacje o podjęcie decyzji dotyczącej do tworzenia aplikacji platformy ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564566"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC — omówienie
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Więcej informacji na temat różnic między aplikacji ASP.NET MVC i formularzy sieci Web ASP.NET aplikacji. Informacje o podjęcie decyzji dotyczącej do tworzenia aplikacji platformy ASP.NET MVC.


Wzorzec architektury Model-widok-kontroler (MVC) dzieli aplikację na trzy główne składniki: model, widok i kontroler. Platforma ASP.NET MVC stanowi alternatywę dla wzorca formularzy sieci Web ASP.NET do tworzenia aplikacji sieci Web opartych na MVC. Platforma ASP.NET MVC to niewielka, wysoce testowalna prezentacji struktura która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejących funkcji platformy ASP.NET, takich jak strony wzorcowe i uwierzytelnianie oparte na członkostwie. Struktura MVC jest zdefiniowana w **System.Web.Mvc** przestrzeni nazw i jest częścią podstawowych, obsługiwanych **System.Web** przestrzeni nazw.   
  
MVC to standardowy wzorzec projektowania, który zna wielu deweloperów. Niektóre rodzaje aplikacji sieci Web zyskają struktura MVC. Inne będą nadal korzystać z tradycyjnego wzorca aplikacji ASP.NET, która jest oparta na formularzach sieci Web i odświeżaniu strony. Inne rodzaje aplikacji sieci Web będą łączone dwa podejścia; jedno nie wyklucza drugiego.   
  
Struktura MVC obejmuje następujące składniki:


[![Wywoływanie akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Rysunek 01**: wywoływania akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-overview/_static/image2.png))


- **Modele**. Obiekty modelu są częściami aplikacji, które implementują logikę domeny danych aplikacji s. Często obiekty modelu pobrać i przechowywanie stanu modelu w bazie danych. Na przykład obiekt produktu mogą pobierać informacje z bazy danych, wykonywać na nich operacje i następnie zapisywać zaktualizowane informacje do tabeli Produkty w programie SQL Server.

W małych aplikacjach model jest często stanowi separację koncepcyjną zamiast fizycznej. Na przykład jeśli aplikacja wyłącznie odczytuje zestaw danych i wysyła je do widoku, aplikacja nie ma warstwy modelu fizycznego i skojarzonych klas. W takim przypadku zestaw danych przyjmuje rolę obiektu modelu.

- **Widoki**. Widoki są składnikami wyświetlającymi aplikacji s interfejsu użytkownika (UI). Zazwyczaj interfejs ten jest utworzone na podstawie danych modelu. Przykładem może być widok edycji tabeli Produkty, która wyświetla pola tekstowe, listy rozwijane i pola wyboru, w oparciu o bieżący stan obiektu produktów.

- **Kontrolery**. Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania, który wyświetla interfejsu użytkownika. W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja. Na przykład kontroler obsługuje wartości ciągu zapytania i przekazuje te wartości do modelu, który z kolei wysyła zapytanie bazy danych przy użyciu wartości.

Wzorzec MVC pomaga w tworzeniu aplikacji, które różne aspekty aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając luźne powiązanie między tymi elementami. Wzorzec Określa, gdzie każdy rodzaj logiki powinien znajdować się w aplikacji. Logika interfejsu użytkownika należy w widoku. Logika danych wejściowych jest powiązana z kontrolerem. Logika biznesowa jest powiązana w modelu. Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji naraz. Na przykład można skoncentrować się na widoku bez polegania na logice biznesowej.   
  
Oprócz zarządzania złożonością wzorzec MVC ułatwia testowanie aplikacji niż do testowania aplikacji sieci Web platformy ASP.NET opartych na formularzach sieci Web. Na przykład w aplikacji sieci Web platformy ASP.NET opartych na formularzach sieci Web pojedyncza klasa jest używana zarówno wyświetlania danych wyjściowych, jak i odpowiadania na dane wejściowe użytkownika. Pisanie zautomatyzowanych testów dla aplikacji platformy ASP.NET opartych na formularzach sieci Web może być skomplikowane, ponieważ do przetestowanie jednej strony, trzeba utworzyć wystąpienie klasy strony, wszystkich jej kontrolek podrzędnych i dodatkowych klas zależnych w aplikacji. Ponieważ do uruchomienia strony wystąpienia są tworzone tylu klas, ciężko może być napisać testy skoncentrowane wyłącznie na pojedynczych częściach aplikacji. Testy dla aplikacji platformy ASP.NET opartych na formularzach sieci Web, w związku z tym może być trudniejsze do zaimplementowania niż testy aplikacji MVC. Ponadto testy w aplikacji platformy ASP.NET opartych na formularzach sieci Web wymagają serwera sieci Web. Struktura MVC rozłącza składniki i intensywnie korzysta z interfejsów, co umożliwia testowanie pojedynczych składników w izolacji od reszty struktury.   
  
Luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe. Na przykład co może pracować w widoku, drugi może pracować nad logiką kontrolera i trzeci deweloper może skupić się na logiki biznesowej w modelu.

## <a name="deciding-when-to-create-an-mvc-application"></a>Kiedy tworzyć aplikację MVC

Należy starannie rozważyć implementowania aplikacji sieci Web przy użyciu modelu formularzy sieci Web ASP.NET lub platformę ASP.NET MVC. Struktura MVC nie zastępuje modelu formularzy sieci Web; można użyć obu struktur dla aplikacji sieci Web. (Jeśli masz istniejące aplikacje oparte na formularzach sieci Web będą działać dokładnie tak, jak zawsze mają.)   
  
Przed użyć struktura MVC i modelu formularzy sieci Web dla określonej witryny sieci Web należy porównać zalety każdego z podejść.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji sieci Web opartych na MVC

Platforma ASP.NET MVC ma następujące zalety:

- Ułatwia ona zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerze. Dzięki temu struktura MVC idealna dla deweloperów, którzy mają pełną kontrolę nad działaniem aplikacji.
- Używa wzorca kontrolera Frontowego, który przetwarza żądania aplikacji sieci Web za pośrednictwem jednego kontrolera. Pozwala na projektowanie aplikacji, która obsługuje złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [kontroler Frontowy](https://go.microsoft.com/fwlink/?LinkId=106357 "kontroler Frontowy") w witrynie MSDN.
- Projektowanie oparte na (testach TDD) zapewnia lepszą obsługę.
- Sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów witryn sieci Web, którzy potrzebują wysokiego stopnia kontroli nad działaniem aplikacji.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji sieci Web opartych na formularzach sieci Web

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje ona model zdarzeń, która zachowuje stan za pośrednictwem protokołu HTTP, które korzysta z tworzenia aplikacji biznesowych z sieci Web. Aplikacji opartych na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Używa wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontrolera strony](https://go.microsoft.com/fwlink/?LinkId=106359 "kontrolera strony") w witrynie MSDN.
- Używa stanu widoku ani formularzy opartych na serwer, co ułatwia zarządzanie informacjami o stanie.
- Sprawdza się w przypadku niewielkich zespołów deweloperów sieci Web i projektantów, którzy chcą czerpać korzyści z dużej liczby składników dostępnych do szybkiego opracowywania aplikacji.
- Ogólnie rzecz biorąc, jest mniej złożona przy projektowaniu aplikacji, ponieważ składniki ( **strony** klasy, kontrolki itd.) są ściśle powiązane i zwykle potrzebna jest mniejsza ilość kodu niż w modelu MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funkcje struktury ASP.NET MVC

Platforma ASP.NET MVC ma następujące cechy:

- Rozdzielenie zadań aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), testowalność i projektowanie oparte na (testach TDD) domyślnie. Wszystkie główne kontrakty w strukturze MVC są oparte na interfejsie i mogą być testowane za pomocą zasymulować obiektów, które są symulowanymi obiektami imitującymi zachowanie rzeczywistych obiektów w aplikacji. Użytkownik może test jednostkowy aplikacji bez konieczności uruchamiania kontrolerów w procesie ASP.NET, dzięki czemu testowania szybkie i elastyczne jednostki. Można użyć dowolnego strukturę testowania jednostkowego zgodną z programu .NET Framework.
- Rozszerzalna struktura obsługująca dodatki. Składniki struktury ASP.NET MVC zostały zaprojektowane tak, aby mogą być łatwo zastępowane lub dostosować. Można dodać własny aparat widoku, zasady routingu adresów URL, serializację parametrów metody akcji i inne składniki. Platforma ASP.NET MVC obsługuje też korzystanie z modeli kontenera zależności Iniekcja oraz Inwersja kontroli (IOC). Podpisane pozwala na iniekcję obiektów do klasy zamiast polegania na klasę, aby ten obiekt utworzy sama. Inwersja kontroli określa, że w przypadku obiekt wymaga innego obiektu, pierwszy obiekt powinien pobrać drugi obiekt ze źródła zewnętrznego, takich jak plik konfiguracji. To ułatwia testowanie.
- Wydajny składnik Mapowanie adresu URL, który umożliwia tworzenie aplikacji, których adresy URL zrozumiałe i można wyszukiwać. Adresy URL nie trzeba dołączać rozszerzenia nazwy pliku i są przeznaczone do obsługi wzorców nazewnictwa adresów URL, które działa dobrze w przypadku wyszukiwania aparatu optymalizacji (SEO) i stan representational transfer (REST) adresowania.
- Obsługa przy użyciu znaczników w istniejących strony ASP.NET (plikach aspx), wzorca plikach znaczników (plikach .master) strony i kontrolki użytkownika (plikach ascx) jako szablonów widoków. Można istniejące funkcje platformy ASP.NET za pomocą struktury ASP.NET MVC, takich jak zagnieżdżone strony wzorcowe, wbudowane wyrażenia (&lt;% = %&gt;), deklaratywne kontrolki serwerowe, szablony, wiązanie danych, lokalizacja i tak dalej.
- Obsługa funkcji programu ASP.NET. ASP.NET MVC umożliwia korzystanie z funkcji, takich jak uwierzytelnianie formularzy i uwierzytelniania systemu Windows, Autoryzacja adresów URL, członkostwo i role, dane wyjściowe i buforowanie danych, zarządzania stanem sesji i profilu, monitorowanie kondycji, system konfiguracji i dostawcy Architektura.
