---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Przy użyciu platformy ASP.NET MVC z różnymi wersjami usług IIS (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku Dowiedz się jak używać programu ASP.NET MVC i routingu adresów URL, z różnymi wersjami programu Internet Information Services. Dowiedz się więcej różne strategie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 73c129c1eaf85cb5b110248fe2a2c0faed0157bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874398"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Przy użyciu platformy ASP.NET MVC z różnymi wersjami usług IIS (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym samouczku Dowiedz się jak używać programu ASP.NET MVC i routingu adresów URL, z różnymi wersjami programu Internet Information Services. Dowiesz się różne strategie korzystania z usług IIS 7.0 (trybu klasycznego), usług IIS 6.0 i starszych wersjach usług IIS przy użyciu platformy ASP.NET MVC.


Platforma ASP.NET MVC jest zależna od routingu platformy ASP.NET żądań przeglądarki trasy do akcji kontrolera. Aby można było korzystać z routingu platformy ASP.NET, może być konieczne wykonanie dodatkowych kroków konfiguracji na serwerze sieci web. Zależy to od wersji programu Internet Information Services (IIS) i tryb aplikacji przetwarzania żądania.

Poniżej przedstawiono podsumowanie różnych wersji programu IIS:

- Usługi IIS 7.0 (tryb zintegrowany) — specjalnej konfiguracji koniecznych do używania routingu platformy ASP.NET.
- Usługi IIS 7.0 (trybu klasycznego) — należy przeprowadzić specjalnej konfiguracji, aby korzystać z routingu platformy ASP.NET.
- Usługi IIS w wersji 6.0 lub poniżej — należy przeprowadzić specjalnej konfiguracji, aby korzystać z routingu platformy ASP.NET.

Najnowszą wersję usług IIS jest wersja 7.5 (na Win7). Usługi IIS w usługach IIS 7 jest dołączony z systemu Windows Server 2008 i VISTA/z dodatkiem SP1 lub nowszy. Można też zainstalować usług IIS 7.0 w dowolnej wersji systemu operacyjnego Vista oprócz Home Basic (zobacz [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

Usługi IIS 7.0 obsługuje dwa tryby do przetwarzania żądań. Można użyć trybu zintegrowanego lub w trybie klasycznym. Nie trzeba wykonywać żadnych czynności konfiguracyjnych specjalne, korzystając z usług IIS 7.0 w trybie zintegrowanym. Jednak należy wykonać dodatkowe czynności konfiguracyjne, korzystając z usług IIS 7.0 w trybie klasycznym.

Microsoft Windows Server 2003 zawiera usług IIS 6.0. Nie można uaktualnić usług IIS 6.0 do usług IIS 7.0, korzystając z systemu operacyjnego Windows Server 2003. Korzystając z usług IIS 6.0, należy wykonać dodatkowe czynności konfiguracyjne.

Microsoft Windows XP Professional includes IIS 5.1. Podczas korzystania z wersji 5.1 usług IIS, należy wykonać dodatkowe czynności konfiguracyjne.

Na koniec Microsoft Windows 2000 i Microsoft Windows 2000 Professional obejmuje programem IIS 5.0. Korzystając z programem IIS 5.0, należy wykonać dodatkowe czynności konfiguracyjne.

## <a name="integrated-versus-classic-mode"></a>Zintegrowane w zależności od trybu klasycznego

Usługi IIS 7.0 może przetwarzać żądania przy użyciu dwóch trybów przetwarzania żądania różne: zintegrowanego i klasycznego. Tryb zintegrowany zapewnia lepszą wydajność i więcej funkcji. Trybu klasycznego jest dołączane do zapewnienia zgodności z wcześniejszymi wersjami usług IIS.

Tryb przetwarzania żądania jest określana przez pulę aplikacji. Można określić, który tryb przetwarzania jest używany przez aplikację sieci web określonego przez określenie puli aplikacji skojarzone z aplikacją. Wykonaj następujące kroki:

1. Uruchom Menedżera internetowych usług informacyjnych
2. W oknie połączenia wybierz aplikację
3. Kliknij w oknie akcje **podstawowe ustawienia** łącze, aby otworzyć okno dialogowe Edytowanie aplikacji (zobacz rysunek 1)
4. Zanotuj wybrany puli aplikacji.

Domyślnie usługi IIS jest skonfigurowany do obsługi dwóch pul aplikacji: **DefaultAppPool** i **Classic .NET AppPool**. Jeśli domyślna pula aplikacji jest zaznaczone, aplikacja jest uruchomiona w trybie zintegrowanym żądania przetwarzania. Jeśli wybrano Classic .NET AppPool, aplikacja działa w trybie przetwarzania żądania klasycznego.

[![Okno dialogowe nowego projektu](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Rysunek 1**: wykrywanie trybu przetwarzania żądania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Zwróć uwagę, że tryb przetwarzania żądań w oknie dialogowym Edytowanie aplikacji można modyfikować. Kliknij przycisk Wybierz, a następnie zmień pulę aplikacji skojarzone z aplikacją. Należy pamiętać, że występują problemy ze zgodnością w przypadku zmiany aplikacji ASP.NET z klasycznego trybu zintegrowanego. Aby uzyskać więcej informacji zobacz następujące artykuły:

- Uaktualnianie do usług IIS 7.0 w systemach Windows Vista i Windows Server 2008 — ASP.NET 1.1 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integracja platformy ASP.NET z usług IIS 7.0 — [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Jeśli aplikacja ASP.NET używa domyślna pula aplikacji, następnie nie trzeba wykonywać żadnych dodatkowych czynności w celu pobrania routingu platformy ASP.NET (i w związku z tym ASP.NET MVC) do pracy. Jednak jeśli aplikacja ASP.NET jest skonfigurowany do Użyj klasycznego pulę aplikacji .NET, a następnie zachować odczytu, ma więcej pracy.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Przy użyciu platformy ASP.NET MVC ze starszymi wersjami usług IIS

Jeśli konieczne jest użycie programu ASP.NET MVC przy użyciu starszej wersji programu IIS od usług IIS 7.0 lub należy użyć usług IIS 7.0 w trybie klasycznym, masz dwie opcje. Po pierwsze można zmodyfikować tabeli tras, aby korzystać z rozszerzeń pliku. Na przykład zamiast żąda adresu URL, takie jak /Store/Details, możesz poprosić adres URL podobny /Store.aspx/Details.

Drugą opcją jest utworzenie obiektu o nazwie *wieloznaczną mapę skryptu*. Wieloznaczną mapę skryptu umożliwia mapowanie każde żądanie do struktury programu ASP.NET.

Jeśli nie masz dostępu do serwera sieci web (na przykład programu ASP.NET MVC aplikacji jest obsługiwana przez usługodawcę internetowego) następnie należy użyć pierwszej opcji. Jeśli nie chcesz zmodyfikować wygląd adresami URL, a Ty masz dostęp do serwera sieci web, można użyć drugiej opcji.

Firma Microsoft Poznaj każdej opcji szczegółowo w poniższych sekcjach.

## <a name="adding-extensions-to-the-route-table"></a>Dodawanie rozszerzenia do tabeli tras

Najprostszym sposobem pobrania routingu platformy ASP.NET do pracy ze starszymi wersjami usług IIS jest do modyfikowania tabeli routingu w pliku Global.asax. Wartość domyślna i niezmodyfikowanego pliku Global.asax wyświetlania 1 konfiguruje jedną trasę o nazwie trasy domyślnej.

**Wyświetlanie listy 1 - Global.asax (bez modyfikacji)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Trasa domyślna skonfigurowana lista 1 umożliwia tras adresów URL, które wyglądają następująco:

/ Głównej/indeksu

/ Produkt/szczegóły/3

/ Produktu

Niestety starszych wersjach usług IIS nie będzie przekazywać te żądania do struktury programu ASP.NET. W związku z tym te żądania nie pobrać kierowane do kontrolera. Na przykład jeśli żądanie przeglądarki na adres URL /Home/indeksu następnie otrzymasz strony błędu na rysunku 2.

[![Okno dialogowe nowego projektu](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Rysunek 2**: odbieranie nie znaleziono błąd 404 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Starsze wersje programu IIS mapują tylko niektórych żądań do struktury programu ASP.NET. Żądanie musi być dla danego adresu URL z rozszerzeniem pliku po prawej. Na przykład żądania /SomePage.aspx pobiera mapowane na struktury programu ASP.NET. Jednak żądania /SomePage.htm nie obsługuje.

W związku z tym uzyskanie do pracy proces routingu platformy ASP.NET, możemy zmodyfikuj trasy domyślnej tak, aby zawiera rozszerzenie pliku, który jest zamapowany na struktury programu ASP.NET.

Jest to realizowane za pomocą skryptu o nazwie `registermvc.wsf`. Została włączona w wersji platformy ASP.NET MVC 1 w `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ale począwszy od programu ASP.NET 2 ten skrypt został przeniesiony do prognozy ASP.NET, dostępne pod adresem [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Wykonującego ten skrypt rejestruje nowe rozszerzenie MVC z usługami IIS. Po zarejestrowaniu rozszerzenia MVC, dzięki czemu trasy rozszerzenia MVC można zmodyfikować trasy w pliku Global.asax.

Zmodyfikowany plik Global.asax 2 listę działa ze starszymi wersjami usług IIS.

**Wyświetlanie listy 2 - Global.asax (zmodyfikowane z rozszerzeniami)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Ważne**: Pamiętaj, aby skompilować aplikację ASP.NET MVC ponownie po zmianie pliku Global.asax.

Istnieją dwie ważne zmiany w pliku Global.asax w wyświetlania 2. Istnieją teraz dwa tras określonych w pliku Global.asax. Wzorzec URL trasy domyślnej pierwszy trasy teraz wygląda następująco:

{controller}.mvc/{action}/{id}

Dodawanie rozszerzenia MVC zmienia typ plików, które przechwytuje modułu routingu platformy ASP.NET. Dzięki tej zmianie teraz aplikacji ASP.NET MVC kieruje żądania podobne do poniższych:

/Home.mvc/Index/

/Product.MVC/details/3

/Product.MVC/

Drugi trasy trasy głównego jest nowa. Ten wzorzec URL trasy głównego jest pustym ciągiem. Ta trasa jest konieczne dopasowanie żądań względem katalogu głównego aplikacji. Na przykład trasy głównego będzie odpowiadała żądania, która wygląda następująco:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Po wprowadzeniu tych zmian do tabeli tras, należy się upewnić, że wszystkie łącza do aplikacji są zgodne z tych nowych wzorców adresów URL. Innymi słowy upewnij się, że wszystkie łącza rozszerzenie MVC. Jeśli używasz Html.ActionLink() metody pomocnika do generowania łącza, należy nie należy wprowadzać żadnych zmian.

Zamiast przy użyciu skryptu registermvc.wcf, możesz dodać nowe rozszerzenie usług IIS, który jest zamapowany na struktury programu ASP.NET ręcznie. Podczas dodawania nowego rozszerzenia samodzielnie, upewnij się, że pole wyboru z etykietą **Sprawdź, czy plik istnieje** nie jest zaznaczone.

## <a name="hosted-server"></a>Hostowanego serwera

Nie zawsze miały dostęp do serwera sieci web. Na przykład przypadku hostowania aplikacji ASP.NET MVC przy użyciu dostawcy hostingu internetowych, następnie nie zawsze masz dostęp do usług IIS.

W takim przypadku należy używać jednego z istniejących rozszerzeń plików, które są mapowane do struktury programu ASP.NET. Przykładami przypisane do ASP.NET rozszerzenia plików .aspx, .axd i ashx rozszerzenia.

Na przykład zmodyfikowany plik Global.asax w 3 wyświetlania używa rozszerzenia aspx zamiast rozszerzenia MVC.

**Wyświetlanie listy 3 - Global.asax (zmodyfikowane z rozszerzeniami aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Plik Global.asax wyświetlania 3 jest dokładnie taka sama poprzedniego pliku Global.asax z wyjątkiem faktów używa rozszerzenia aspx, zamiast rozszerzenia MVC. Nie trzeba wykonywać żadnej konfiguracji na serwerze sieci web zdalnego próbę użycia rozszerzenia aspx.

## <a name="creating-a-wildcard-script-map"></a>Tworzenie wieloznaczną mapę skryptu

Jeśli nie chcesz zmodyfikować adresy URL aplikacji ASP.NET MVC, a Ty masz dostęp do serwera sieci web, należy dodatkową opcję. Można utworzyć wieloznaczną mapę skryptu, który mapuje wszystkie żądania na serwerze sieci web struktury programu ASP.NET. Dzięki temu można używać domyślnych tabeli tras platformy ASP.NET MVC z usług IIS 7.0 (w trybie klasycznym) lub usług IIS 6.0.

Należy pamiętać, że ta opcja powoduje, że usługi IIS do przechwycenia każde żądanie dotyczące serwera sieci web. W tym żądania dotyczące obrazów, strony ASP klasycznego i stron HTML. W związku z tym włączenie symbol wieloznaczny mapy skryptów platformy ASP.NET miało wpływ na wydajność.

Oto, jak włączyć wieloznaczną mapę skryptu dla usług IIS 7.0:

1. Wybierz aplikację w oknie połączenia
2. Upewnij się, że **funkcje** wybraniu widoku
3. Kliknij dwukrotnie **mapowania obsługi** przycisku
4. Kliknij przycisk **Dodaj wieloznaczną mapę skryptu** link (patrz rysunek 3)
5. Wprowadź ścieżkę do aspnet\_isapi.dll pliku (z PageHandlerFactory mapę skryptu można skopiować tej ścieżki)
6. Wprowadź nazwę MVC
7. Kliknij przycisk **OK** przycisku

[![Okno dialogowe nowego projektu](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Rysunek 3**: tworzenie wieloznaczną mapę skryptu za pomocą usług IIS 7.0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Wykonaj następujące kroki, aby utworzyć wieloznaczną mapę skryptu w usługach IIS 6.0:

1. Kliknij prawym przyciskiem myszy witrynę sieci Web i wybierz polecenie Właściwości
2. Wybierz **katalog macierzysty** kartę
3. Kliknij przycisk **konfiguracji** przycisku
4. Wybierz **mapowania** kartę
5. Kliknij przycisk **Wstaw** przycisk (patrz rysunek 4)
6. Wklej ścieżkę do aspnet\_isapi.dll pliku wykonywalnego w polu (można skopiować tej ścieżki z mapy skryptów dla plików aspx)
7. Usuń zaznaczenie pola wyboru **Sprawdź, czy plik istnieje**
8. Kliknij przycisk **OK** przycisku

[![Okno dialogowe nowego projektu](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Rysunek 4**: tworzenie wieloznaczną mapę skryptu w usługach IIS 6.0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Po włączeniu mapy skryptów symboli wieloznacznych, należy zmodyfikować tabeli tras w pliku Global.asax co zawierają one trasy głównego. W przeciwnym razie zostanie wyświetlony strony błędu na rysunku 5 podczas przesyłania żądania do strony głównej aplikacji. Można użyć zmodyfikowanego pliku Global.asax listę 4.

[![Okno dialogowe nowego projektu](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Rysunek 5**: Brak głównego trasy błędu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Wyświetlanie listy 4 - Global.asax (zmodyfikowane z trasą głównego)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Po włączeniu wieloznaczną mapę skryptu dla usług IIS 7.0 i IIS 6.0, mogą wysyłać żądania współpracujących z domyślną tabelę routingu, które wyglądają następująco:

/

/ Głównej/indeksu

/ Produkt/szczegóły/3

/ Produktu

## <a name="summary"></a>Podsumowanie

Celem tego samouczka jest wyjaśnienie, jak skorzystać z platformy ASP.NET MVC przy użyciu starszej wersji programu IIS (lub usług IIS 7.0 w trybie klasycznym). Omówiono dwie metody uzyskania routingu platformy ASP.NET do pracy ze starszymi wersjami usług IIS: Zmodyfikuj domyślną tabelę routingu lub Utwórz wieloznaczną mapę skryptu.

Pierwsza opcja wymaga zmodyfikowania adresy URL używane w aplikacji ASP.NET MVC. Jedną z zalet bardzo istotne tej pierwszej opcji jest konieczne dostęp do serwera sieci web w celu zmodyfikowania tabeli tras. Oznacza to, można użyć tej opcji pierwszy nawet wtedy, gdy obsługa aplikacji ASP.NET MVC za pomocą internetowych hostingu.

Drugą opcją jest utworzyć wieloznaczną mapę skryptu. Zaletą tej drugiej opcji jest, że nie ma potrzeby modyfikowania adresami URL. Wadą tego druga opcja to, że może wpłynąć na wydajność aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Next](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
