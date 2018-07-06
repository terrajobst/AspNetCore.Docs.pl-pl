---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Używanie wzorca ASP.NET MVC z różnymi wersjami usług IIS (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku dowiesz się, jak używać platformy ASP.NET MVC i routingu adresów URL, z różnymi wersjami programu Internet Information Services. Dowiedz się więcej różne strategie...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d0c625552c87a8e63e48d145eb14266998cc5e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814879"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Używanie wzorca ASP.NET MVC z różnymi wersjami usług IIS (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym samouczku dowiesz się, jak używać platformy ASP.NET MVC i routingu adresów URL, z różnymi wersjami programu Internet Information Services. Można dowiedzieć się, jak różne strategie Używanie wzorca ASP.NET MVC za pomocą usług IIS 7.0 (tryb klasyczny), usług IIS 6.0 i starszych wersjach usług IIS.


Platforma ASP.NET MVC jest zależna od routingu platformy ASP.NET na żądania przeglądarki trasy do akcji kontrolera. Aby można było korzystać z routingu platformy ASP.NET, trzeba będzie wykonać dodatkowe czynności konfiguracyjne na serwerze sieci web. Wszystko zależy od wersji programu Internet Information Services (IIS) i tryb aplikacji przetwarzania żądania.

Poniżej przedstawiono podsumowanie z różnymi wersjami usług IIS:

- Usługi IIS 7.0 (tryb zintegrowany) — specjalnej konfiguracji niezbędne do korzystania z routingu platformy ASP.NET.
- Usługi IIS 7.0 (tryb klasyczny) — należy wykonać specjalnej konfiguracji do użycia routingu platformy ASP.NET.
- Usługi IIS w wersji 6.0 lub poniżej - musisz wykonać specjalnej konfiguracji do użycia routingu platformy ASP.NET.

Najnowszą wersję usług IIS jest w wersji 7.5 (w systemie Win7). Usługi IIS w usługach IIS 7 jest uwzględniane przy użyciu systemu Windows Server 2008 i VISTA/z dodatkiem SP1 lub nowszy. Można też zainstalować usługi IIS 7.0 w dowolnej wersji systemu operacyjnego Vista oprócz Home Basic (zobacz [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

Usługi IIS 7.0 obsługuje dwa tryby do przetwarzania żądań. Można użyć w trybie zintegrowanym lub w trybie klasycznym. Nie trzeba wykonywać żadnych czynności konfiguracyjnych specjalne, korzystając z usług IIS 7.0 w trybie zintegrowanym. Jednakże należy wykonać dodatkowe czynności konfiguracyjne, korzystając z usług IIS 7.0 w trybie klasycznym.

Microsoft Windows Server 2003 obejmuje usługi IIS 6.0. Nie można uaktualnić usług IIS 6.0 do usług IIS 7.0, korzystając z systemu operacyjnego Windows Server 2003. Korzystając z usług IIS 6.0, należy wykonać dodatkowe czynności konfiguracyjne.

Microsoft Windows XP Professional includes IIS 5.1. Należy wykonać dodatkowe czynności konfiguracyjne, korzystając z usługi IIS 5.1.

Na koniec Microsoft Windows 2000 i Microsoft Windows 2000 Professional obejmuje usługi IIS 5.0. Należy wykonać dodatkowe czynności konfiguracyjne, korzystając z programem IIS 5.0.

## <a name="integrated-versus-classic-mode"></a>Zintegrowane w porównaniu z trybu klasycznego

Usługi IIS 7.0 może przetwarzać żądania przy użyciu dwóch trybów przetwarzania żądania innego: klasycznym i zintegrowane usługi. W trybie zintegrowanym zapewnia lepszą wydajność i więcej funkcji. Trybu klasycznego jest dołączony do tyłu zgodność z wcześniejszymi wersjami usług IIS.

Tryb przetwarzania żądania jest określana przez pulę aplikacji. Można określić, który tryb przetwarzania jest on używany przez aplikacji sieci web określonej przez określenie puli aplikacji skojarzonych z aplikacją. Wykonaj następujące kroki:

1. Uruchom Menedżera internetowych usług informacyjnych
2. W oknie połączenia wybierz aplikację
3. Kliknij w oknie akcje **podstawowych ustawień** łącze, aby otworzyć okno dialogowe Edytowanie aplikacji (zobacz rysunek 1)
4. Zwróć uwagę na pulę aplikacji wybrane.

Domyślnie program IIS jest skonfigurowany do obsługi dwóch pul aplikacji: **DefaultAppPool** i **klasyczne .NET puli AppPool**. Jeśli domyślna pula aplikacji jest zaznaczone, aplikacja jest uruchomiona w trybie zintegrowanym żądania przetwarzania. Jeśli wybrano klasyczne .NET pulę aplikacji, Twoja aplikacja jest uruchomiona w trybie klasycznym żądania przetwarzania.

[![Okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Rysunek 1**: wykrywanie trybu przetwarzania żądania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Należy zauważyć, że tryb przetwarzania żądań w oknie dialogowym Edytowanie aplikacji można modyfikować. Kliknij przycisk Wybierz, a następnie zmień pulę aplikacji skojarzonych z aplikacją. Należy pamiętać, że występują problemy ze zgodnością, zmieniając aplikacji ASP.NET z wersji klasycznej do działania w trybie zintegrowanym. Aby uzyskać więcej informacji zobacz następujące artykuły:

- Uaktualnianie platformy ASP.NET 1.1 w usługach IIS 7.0 w systemach Windows Vista i Windows Server 2008 — [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integracja platformy ASP.NET w usługach IIS 7.0 — [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Jeśli aplikacja ASP.NET używa domyślna pula aplikacji, nie musisz wykonywać żadnych dodatkowych czynności w celu routingu platformy ASP.NET (i w związku z tym platformy ASP.NET MVC) do pracy. Jednak jeśli aplikacja ASP.NET jest skonfigurowany do Użyj klasycznego pulę aplikacji .NET, a następnie Zachowaj odczytu, ma więcej pracy do wykonania.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Używanie wzorca ASP.NET MVC z poprzednimi wersjami usług IIS

Jeśli musisz użyć programu ASP.NET MVC za pomocą starszej wersji programu IIS od usług IIS 7.0 lub należy użyć usług IIS 7.0 w trybie klasycznym, masz dwie opcje. Po pierwsze można zmodyfikować tabeli tras, aby korzystać z rozszerzeń plików. Na przykład zamiast żądanie adresu URL typu /Store/Details, możesz zażądać adresu URL typu /Store.aspx/Details.

Drugą opcją jest utworzenie obiektu o nazwie *wieloznaczną mapę skryptu*. Wieloznaczną mapę skryptu umożliwia mapowanie każdego żądania do struktury ASP.NET.

Jeśli nie masz dostępu do serwera sieci web (na przykład usługi ASP.NET MVC aplikacja jest hostowana przez usługodawcę internetowego) następnie należy użyć pierwszej opcji. Jeśli nie chcesz zmienić wygląd adresami URL, a Ty masz dostęp do serwera sieci web, można użyć drugiej opcji.

Omówimy każdą opcję szczegółowo w poniższych sekcjach.

## <a name="adding-extensions-to-the-route-table"></a>Dodawanie rozszerzeń do tabeli tras

Najprostszym sposobem routingu platformy ASP.NET do pracy ze starszymi wersjami usług IIS jest modyfikowanie tabeli tras w pliku Global.asax. Wartość domyślna i niezmodyfikowanego pliku Global.asax w ofercie 1 konfiguruje jedną trasę o nazwie trasy domyślnej.

**Wyświetlanie listy 1 - Global.asax (bez modyfikacji)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Trasa domyślna skonfigurowane w ofercie 1 umożliwia tras adresów URL, które wyglądają następująco:

/ Home/Index

/ Produkt/szczegóły/3

/ Produktu

Niestety starsze wersje usług IIS nie będzie przekazywać te żądania do struktury ASP.NET. W związku z tym te żądania nie uzyskać kierowane do kontrolera. Na przykład jeśli wykonasz żądanie typu przeglądarki dla adresu URL /Home/indeksu następnie otrzymasz strony błędu na rysunku 2.

[![Okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Rysunek 2**: odbieranie błąd 404 Nie znaleziono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Starsze wersje usług IIS mapują tylko niektórych żądań do struktury ASP.NET. Żądanie musi być dla danego adresu URL z rozszerzeniem pliku po prawej. Na przykład żądanie /SomePage.aspx pobiera mapowany do struktury ASP.NET. Jednak żądanie /SomePage.htm — nie.

W związku z tym Aby uzyskać routingu platformy ASP.NET do pracy, firma Microsoft zmodyfikować trasy domyślnej tak, aby zawiera rozszerzenie pliku, który jest mapowany do struktury ASP.NET.

Odbywa się przy użyciu skryptu o nazwie `registermvc.wsf`. Została włączona w wersji platformy ASP.NET MVC 1 w `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ale począwszy od platformy ASP.NET 2 ten skrypt został przeniesiony do prognoz ASP.NET dostępne pod adresem [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Wykonującego ten skrypt rejestruje nowe rozszerzenie MVC za pomocą programu IIS. Po zarejestrowaniu rozszerzenia MVC, tak aby tras za pomocą rozszerzenia MVC można zmodyfikować trasy w pliku Global.asax.

Zmodyfikowany plik Global.asax w ofercie 2 działa ze starszymi wersjami usług IIS.

**Wyświetlanie listy 2 - Global.asax (zmodyfikowany za pomocą rozszerzenia)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Ważne**: Pamiętaj, aby tworzenie aplikacji ASP.NET MVC ponownie po zmianie pliku Global.asax.

Istnieją dwie ważne zmiany w pliku Global.asax w ofercie 2. Są teraz dwie trasy zdefiniowane w pliku Global.asax. Wzorzec URL trasy domyślnej pierwsza trasa wygląda teraz następująco:

{controller}.mvc/{action}/{id}

Dodawanie rozszerzenia MVC zmieni typ plików, które przechwytuje modułu routingu platformy ASP.NET. Dzięki tej zmianie aplikacji platformy ASP.NET MVC teraz kieruje żądania, jak pokazano poniżej:

/Home.mvc/Index/

/Product.MVC/details/3

/Product.MVC/

Druga trasa główny trasy, jest nowa. Ten wzorzec URL trasy głównego jest pustym ciągiem. Ta trasa jest niezbędne dla dopasowania żądania skierowanego do katalogu głównego aplikacji. Na przykład główny trasy będą zgodne żądanie, które wyglądają następująco:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Po wprowadzeniu tych zmian do tabeli tras, należy się upewnić, że wszystkie linki w aplikacji, są zgodne z tych nowych wzorce adresów URL. Innymi słowy upewnij się, że wszystkie łącza rozszerzenie MVC. Jeśli używasz Html.ActionLink() metody pomocnika do generowania łącza, następnie nie należy wprowadzać żadnych zmian.

Zamiast przy użyciu skryptu registermvc.wcf, możesz dodać nowe rozszerzenie usług IIS, który jest mapowany do środowiska ASP.NET framework ręcznie. Podczas dodawania nowego rozszerzenia samodzielnie, upewnij się, że pole wyboru etykietą **Sprawdź, czy plik istnieje** nie jest zaznaczone.

## <a name="hosted-server"></a>Hostowanego serwera

Nie zawsze mieć dostęp do serwera sieci web. Na przykład jeśli hostujesz aplikację ASP.NET MVC przy użyciu dostawcy hostingu internetowego, następnie nie będzie zawsze masz dostęp do usług IIS.

W takiej sytuacji należy używać jednej z istniejących rozszerzeń plików, które są mapowane do struktury ASP.NET. Przykłady rozszerzeń plików przypisane do ASP.NET .aspx, .axd i ashx rozszerzenia.

Na przykład zmodyfikowany plik Global.asax w ofercie 3 wykorzystuje rozszerzenie .aspx, zamiast rozszerzenia MVC.

**Wyświetlanie listy 3 - w pliku Global.asax (zmodyfikowany z rozszerzeniami .aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Plik Global.asax w ofercie 3 jest dokładnie taka sama, jak poprzedni plik Global.asax z wyjątkiem faktów używa rozszerzenia .aspx, zamiast rozszerzenia MVC. Nie trzeba wykonywać żadnej konfiguracji na serwerze sieci web do zdalnego próbę użycia rozszerzenia .aspx.

## <a name="creating-a-wildcard-script-map"></a>Tworzenie wieloznaczną mapę skryptu

Jeśli nie chcesz zmodyfikować adresy URL dla aplikacji ASP.NET MVC, a Ty masz dostęp do serwera sieci web, masz dodatkową opcję. Można utworzyć wieloznaczną mapę skryptu, który mapuje wszystkie żądania do serwera sieci web do struktury ASP.NET. Dzięki temu użytkownik korzysta z domyślnych tabeli trasy ASP.NET MVC za pomocą usług IIS 7.0 (w trybie klasycznym) lub usług IIS 6.0.

Należy pamiętać, że ta opcja powoduje, że usługi IIS, aby przechwycić każdego żądania skierowanego do serwera sieci web. Dotyczy to żądań dla obrazów, klasycznych stron ASP i strony HTML. Włączanie symboli wieloznacznych mapę skryptu do programu ASP.NET więc wpływ na wydajność.

Poniżej przedstawiono, jak włączyć wieloznaczną mapę skryptu dla usług IIS 7.0:

1. Wybierz swoją aplikację w oknie połączenia
2. Upewnij się, że **funkcji** wybrany widok
3. Kliknij dwukrotnie **mapowania obsługi** przycisku
4. Kliknij przycisk **Dodaj wieloznaczną mapę skryptu** link (zobacz rysunek 3)
5. Wprowadź ścieżkę do aspnet\_pliku isapi.dll (można skopiować tę ścieżkę z mapę skryptu PageHandlerFactory)
6. Wprowadź nazwę MVC
7. Kliknij przycisk **OK** przycisku

[![Okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Rysunek 3**: tworzenie wieloznaczną mapę skryptu za pomocą usług IIS 7.0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Wykonaj następujące kroki, aby utworzyć wieloznaczną mapę skryptu za pomocą usług IIS 6.0:

1. Kliknij prawym przyciskiem myszy witrynę sieci Web i wybierz polecenie Właściwości
2. Wybierz **katalog macierzysty** kartę
3. Kliknij przycisk **konfiguracji** przycisku
4. Wybierz **mapowania** kartę
5. Kliknij przycisk **Wstaw** przycisku (zobacz rysunek 4)
6. Wklej ścieżkę do aspnet\_isapi.dll do pola pliku wykonywalnego (można skopiować tę ścieżkę z mapy skryptów dla plików aspx)
7. Usuń zaznaczenie pola wyboru **Sprawdź, czy plik istnieje**
8. Kliknij przycisk **OK** przycisku

[![Okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Rysunek 4**: tworzenie wieloznaczną mapę skryptu za pomocą usług IIS 6.0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Po włączeniu mapowania skryptów symboli wieloznacznych, należy zmodyfikować tabeli tras w pliku Global.asax, aby obejmowała główny trasy. W przeciwnym razie otrzymasz strony błędu na rysunku 5 podczas przesyłania żądania do strony głównej aplikacji. Można użyć zmodyfikowany plik Global.asax w ofercie 4.

[![Okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Rysunek 5**: Brak główny trasy błędu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Wyświetlanie listy 4 - Global.asax (zmodyfikowany przy użyciu główny trasy)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Po włączeniu wieloznaczną mapę skryptu dla usług IIS 7.0 i IIS 6.0, mogą wysyłać żądania współpracujących z tabela routingu domyślnego, które wyglądają następująco:

/

/ Home/Index

/ Produkt/szczegóły/3

/ Produktu

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wyjaśniają, jak używać platformy ASP.NET MVC przy użyciu starszej wersji usług IIS (lub w usługach IIS 7.0 w trybie klasycznym). Omówiliśmy dwie metody uzyskiwania routingu platformy ASP.NET do pracy ze starszymi wersjami usług IIS: modyfikowanie tabela routingu domyślnego lub tworzenie wieloznaczną mapę skryptu.

Pierwsza opcja wymaga zmodyfikowania adresy URL używane w aplikacji ASP.NET MVC. Jedną z bardzo istotne korzyści to pierwsza opcja to, czy nie potrzebują dostępu do serwera sieci web Aby zmodyfikować tabelę tras. Oznacza to, można użyć tej opcji pierwszy nawet wtedy, gdy hosting aplikacji ASP.NET MVC z Internetu firma zapewniająca hosting.

Drugą opcją jest utworzyć wieloznaczną mapę skryptu. Zaletą tej drugiej opcji jest, że nie ma potrzeby modyfikowania adresami URL. Wadą tego druga opcja to, że może mieć wpływ na wydajność aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Next](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
