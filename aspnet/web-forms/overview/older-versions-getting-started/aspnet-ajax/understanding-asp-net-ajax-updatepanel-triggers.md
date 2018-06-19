---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Opis ASP.NET AJAX UpdatePanel wyzwalaczy | Dokumentacja firmy Microsoft
author: scottcate
description: Podczas pracy w edytorze kodu znaczników w programie Visual Studio, (z IntelliSense) można zauważyć, że istnieją dwa elementy podrzędne formantu UpdatePanel. Jeden z ki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890752"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Opis wyzwalaczy UpdatePanel AJAX ASP.NET
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Podczas pracy w edytorze kodu znaczników w programie Visual Studio, (z IntelliSense) można zauważyć, że istnieją dwa elementy podrzędne formantu UpdatePanel. Z których jeden jest element wyzwalaczy, który określa formantów na stronie (lub kontrolki użytkownika, jeśli używasz) która wyzwoli częściowe renderowanie formantu UpdatePanel, w której znajduje się element.


## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowane obiektowo i sterowane zdarzeniami i unites go z zalet skompilowanego kodu. Jednak jego model przetwarzania po stronie serwera ma kilka wady związane z technologii, z których wiele może zostać zlikwidowane przez nowe funkcje uwzględnione w rozszerzeniach AJAX Microsoft ASP.NET 3.5. Te rozszerzenia włączyć wiele nowych funkcji wzbogaconego klienta, w tym częściowe renderowanie stron bez konieczności odświeżania strony pełne, możliwość dostępu za pośrednictwem skrypt po stronie klienta (w tym profilowania API ASP.NET) i szeroką gamę po stronie klienta interfejsu API usług sieci Web przeznaczony do duplikatów wiele systemów kontroli w zestawie formantu po stronie serwera ASP.NET.

Ten dokument sprawdza funkcji wyzwalaczy XML programu ASP.NET AJAX `UpdatePanel` składnika. Wyzwalacze XML zapewniają kontrolę nad składników, które mogą spowodować częściowe renderowanie kontrolek elementu UpdatePanel.

Ten dokument jest oparta na wersji Beta 2 programu .NET Framework 3.5 i Visual Studio 2008. Rozszerzenia ASP.NET AJAX, wcześniej zestawu dodatku celem składnika ASP.NET 2.0, teraz są zintegrowane w bibliotece klas programu .NET Framework bazy. Ten dokument również założono, że będzie działać z programu Visual Studio 2008, nie Visual Web Developer Express i zapewnia wskazówki zgodnie z interfejsu użytkownika programu Visual Studio (chociaż fragmentów kodu są całkowicie zgodne, niezależnie od tego środowisko projektowe).

## <a name="triggers"></a>*Wyzwalacze*

Wyzwalacze dla danego elementu UpdatePanel, domyślnie automatycznie obejmują wszystkie formanty podrzędne, które wywołują ogłaszania zwrotnego, w tym (na przykład) pól tekstowych, które mają ich `AutoPostBack` ustawioną właściwość **true**. Jednak usługa wyzwalaczy może także stanowić deklaratywnie przy użyciu znaczników; jest to realizowane w ramach `<triggers>` sekcji deklaracji formantu UpdatePanel. Mimo że wyzwalaczy jest możliwy za pośrednictwem `Triggers` właściwości kolekcji, zaleca się zarejestrować wszelkie wyzwalacze częściowe renderowanie w czasie wykonywania, (na przykład, jeśli formant nie jest dostępny w czasie projektowania) przy użyciu `RegisterAsyncPostBackControl(Control)` metody ScriptManager obiekt ze strony poziomu `Page_Load` zdarzeń. Należy pamiętać, że strony są bezstanowych i dlatego należy ponownie zarejestrować tych kontrolek za każdym razem, gdy są one tworzone.

Automatyczne podrzędnych wyzwalacza włączenia można także wyłączyć (tak, aby formanty podrzędne, które utworzyć ogłaszania zwrotnego nie wyzwalają automatycznie renderuje częściowe) przez ustawienie `ChildrenAsTriggers` właściwości **false**. Zapewnia większą elastyczność podczas przypisywania które określonej kontrolki może wywoływać renderowania strony i jest zalecane, dzięki czemu deweloper będzie wyrazić zgodę na odpowiadanie na zdarzenia, a nie wszystkie zdarzenia, które mogą wystąpić podczas obsługi.

Należy pamiętać, że gdy UpdatePanel formanty są zagnieżdżone, gdy właściwość UpdateMode ma wartość **warunkowego**, jeśli podrzędnych elementu UpdatePanel zostanie wywołany, ale element nadrzędny nie jest następnie podrzędnych elementu UpdatePanel zostanie odświeżony. Jednak jeśli element nadrzędny elementu UpdatePanel zostanie odświeżony, następnie podrzędnych elementu UpdatePanel będzie również odświeżyć.

## <a name="the-lttriggersgt-element"></a>*&lt;Wyzwalaczy&gt; — Element*

Podczas pracy w edytorze kodu znaczników w programie Visual Studio, można zauważyć (z IntelliSense) czy dwa elementy podrzędne elementu `UpdatePanel` formantu. Element najczęściej spotykanych to `<ContentTemplate>` element, który hermetyzuje zasadniczo zawartość, która będzie on wstrzymany przez zespół aktualizacji (zawartość, dla którego możemy włączenie częściowe renderowanie). Inny element `<Triggers>` element, który określa formantów na stronie (lub kontrolki użytkownika, jeśli używasz) która wyzwoli częściowe renderowanie formantu UpdatePanel, w którym &lt;wyzwalaczy&gt; znajduje się elementu.

`<Triggers>` Element może zawierać dowolną liczbę każdego dwa węzły podrzędne: `<asp:AsyncPostBackTrigger>` i `<asp:PostBackTrigger>`. Akceptacji dwa atrybuty `ControlID` i `EventName`i określić żadnego formantu w bieżącej jednostce encapsulation (na przykład, jeśli formantu UpdatePanel znajduje się w obrębie kontrolka użytkownika sieci Web, możesz nie powinny podejmować próby odwołania formantu na Strona, na którym będzie przechowywany kontrolki użytkownika).

`<asp:AsyncPostBackTrigger>` Element jest szczególnie przydatne w tym można kierować wszystkie zdarzenia z formantu, czy istnieje jako element podrzędny *żadnych* formantu UpdatePanel w jednostce hermetyzacji, nie tylko element UpdatePanel, w którym tego wyzwalacza jest elementem podrzędnym . W związku z tym żadnego formantu, możliwe do wyzwolenia aktualizacji strony częściowe.

Podobnie `<asp:PostBackTrigger>` element może służyć do renderowania strony częściowe wyzwalacza, ale wymagający pełnego przesyłania danych do serwera. Ten element wyzwalacza można również wymusić renderowania strony pełnego, gdy formant normalnie w przeciwnym razie wywołane renderowania strony częściowe (na przykład w przypadku, gdy `Button` formant istnieje w `<ContentTemplate>` element formantu UpdatePanel). Ponownie PostBackTrigger element można określić żadnego formantu, który jest elementem podrzędnym żadnego formantu UpdatePanel w bieżącej jednostce hermetyzacji.

## <a name="lttriggersgt-element-reference"></a>*&lt;Wyzwalacze&gt; odwołanie do elementu*

*Elementy podrzędne znaczników:*

| **Tag** | **Opis** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Określa formant i zdarzenia, który spowoduje, że aktualizacja strona częściowa dla elementu UpdatePanel, który zawiera odwołanie do tego wyzwalacza. |
| &lt;asp:PostBackTrigger&gt; | Określa formant i zdarzenia, który spowoduje, że całą stronę aktualizacji (odświeżania strony pełną). Ten tag można wymusić pełne odświeżenie, gdy formant w przeciwnym razie spowoduje wywołanie częściowe renderowanie. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Wskazówki: Element UpdatePanel między wyzwalacze*

1. Utwórz nową stronę ASP.NET z obiektem ScriptManager, aby włączyć częściowe renderowanie. Dodaj dwa UpdatePanels do tej strony — w pierwszym, obejmują formantu etykiety (Label1) i dwóch kontrolek przycisku (Button1 i Button2). Button1 powinny przekazać komunikat Kliknij, aby zaktualizować zarówno i Button2 powinny przekazać komunikat Kliknij, aby zaktualizować to lub czegoś w tym kierunku. W drugim UpdatePanel obejmować tylko formantu etykiety (Label2), ale ustaw dla właściwości ForeColor inną niż domyślna do odróżnienia go.
2. Ustaw właściwość UpdateMode zarówno tagów UpdatePanel **warunkowego**.

**Wyświetlanie 1: Znaczników default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. W obsługi zdarzeń kliknięcia Button1 Ustaw Label1.Text i Label2.Text coś zależnych od czasu (na przykład DateTime.Now.ToLongTimeString()). Dla obsługi zdarzeń kliknięcia Button2 ustawić Label1.Text tylko wartości zależnych od czasu.

**Wyświetlanie 2: Plik Codebehind (obcięte) default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby skompilować i uruchomić projekt. Należy pamiętać, że po kliknięciu aktualizacji zarówno panele zarówno etykiety zmienić tekst; Jednak po kliknięciu tego panelu aktualizacji tylko Label1 aktualizacji.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Kulisy*

Wykorzystanie w przykładzie, który właśnie utworzone, firma Microsoft może Przyjrzyjmy się czynności ASP.NET AJAX i sposobu działania naszych wyzwalaczy między panel elementu UpdatePanel. Aby to zrobić, zajmiemy się źródła wygenerowanego strony HTML, a także rozszerzenia Mozilla Firefox o nazwie FireBug — dzięki temu można łatwo omówione ogłaszania zwrotnego AJAX. Firma Microsoft będzie także narzędzie reflektora .NET przez Lutz Roeder. Oba te narzędzia są dostępne bezpłatnie w trybie online i znajduje się z wyszukiwaniem w Internecie.

Analiza kodu źródłowego strony nie zawiera prawie żadnych wartości niezwykłe; Formanty UpdatePanel są renderowane jako `<div>` kontenery i firma Microsoft można wyświetlić zasobu skryptu zawiera pod warunkiem przez `<asp:ScriptManager>`. Dostępne są także niektóre nowe specyficzne dla interfejsu AJAX wywołań elementu PageRequestManager będących wewnętrzne skryptu biblioteki klienckiej AJAX. Ponadto przedstawiono dwa kontenery UpdatePanel — jeden z renderowanych `<input>` przycisków z dwóch `<asp:Label>` formanty renderowane jako `<span>` kontenerów. (Jeśli sprawdzenie drzewie DOM w FireBug można zauważyć, że etykiety są nieaktywne, aby wskazać, że nie prowadzą widocznej zawartości).

Kliknij przycisk panelu tej aktualizacji, a należy zauważyć, że górny element UpdatePanel zostanie zaktualizowany przy użyciu bieżącego czasu serwera. W FireBug wybierz kartę konsoli, dzięki czemu można sprawdzić żądanie. Najpierw sprawdź parametry żądania POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Należy pamiętać, że element UpdatePanel wskazany kod AJAX po stronie serwera dokładnie drzewa formantów, które zostało zainicjowane przez parametr ScriptManager1: `Button1` z `UpdatePanel1` formantu. Teraz kliknij przycisk panele zarówno aktualizacji. Następnie badanie odpowiedzi, widzimy szereg zmienne ustawione w ciągu; rozdzielone potoku w szczególności widzimy górny element UpdatePanel, `UpdatePanel1`, ma całości jego HTML wysyłany do przeglądarki. Skryptu biblioteki klienckiej AJAX zastępuje elementu UpdatePanel oryginalna zawartość HTML z nowej zawartości za pośrednictwem `.innerHTML` właściwości, a więc serwer wysyła zmieniona zawartość z serwera w formacie HTML.

Teraz kliknij przycisk panele zarówno aktualizacji i przejrzeć wyniki z serwera. Wyniki są bardzo podobne — zarówno UpdatePanels uzyskiwać nowy HTML od serwera. Podobnie jak w przypadku poprzedniego wywołania zwrotnego, są wysyłane dodatkowe strony stanu.

Jak widać, ponieważ żaden specjalny kod jest wykorzystywany do wykonania ogłaszania zwrotnego AJAX, biblioteki skryptów AJAX klienta jest w stanie przechwycenia ogłaszania zwrotnego formularza bez jakiegokolwiek dodatkowego kodu. Formanty serwera automatycznie wykorzystywać JavaScript, aby ich nie automatycznie przesłać formularza — program ASP.NET jest automatycznie injects kodu walidacji formularza i stan już, przede wszystkim osiągnięte przez automatyczny skrypt zasobu dołączania, klasa PostBackOptions , a klasa ClientScriptManager.

Na przykład należy wziąć pod uwagę kontrolkę pola wyboru; Sprawdź, czy klasa dezasemblacji w reflektora .NET. Aby to zrobić, upewnij się, że używanemu zestawowi System.Web jest otwarty, a następnie przejdź do `System.Web.UI.WebControls.CheckBox` klasy otwierania `RenderInputTag` metody. Szukaj w warunku, który sprawdza zgodność `AutoPostBack` właściwości:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Po włączeniu na automatyczne odświeżenie strony `CheckBox` Sterowanie (za pośrednictwem właściwości AutoPostBack o wartości true), wynikowe `<input>` tag w związku z tym jest odwzorowywany z obsługi skryptów dla zdarzeń ASP.NET jego `onclick` atrybutu. Zatrzymania przesyłania formularza, następnie umożliwia ASP.NET AJAX do można wprowadzić do strony nonintrusively, co pomaga uniknąć potencjalnego, wszelkie istotne zmiany, które mogą wystąpić przy użyciu prawdopodobnie nieprecyzyjne ciąg zastępczy. Ponadto pozwala to *żadnych* kontrolki niestandardowej ASP.NET mogą korzystać z możliwości środowiska ASP.NET AJAX bez jakiegokolwiek dodatkowego kodu do obsługi użytkowania w kontenerze UpdatePanel.

`<triggers>` Funkcji odpowiada wartości zainicjowany w wywołaniu elementu PageRequestManager \_updateControls (Uwaga biblioteki skryptu klienta ASP.NET AJAX używa konwencji, tej metody, zdarzeń i nazw pól, które rozpoczynają się od znaku podkreślenia są oznaczone jako wewnętrzne i nie są przeznaczone do użytku poza samej biblioteki). Firma Microsoft można obserwować, które kontrolki powinny powodować ogłaszania zwrotnego AJAX.

Na przykład Dodajmy dwóch dodatkowych funkcji kontroli do strony, pozostawiając jeden formant poza UpdatePanels całkowicie i pozostawienie w elemencie UpdatePanel. Firma Microsoft będzie dodać kontrolkę pola wyboru w górnej UpdatePanel i upuść DropDownList o liczbie kolorów zdefiniowanych w obrębie listy. Oto nowy kod znaczników:

**Wyświetlanie 3: Nowego znacznika**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

A Oto nowy kodem:

**Wyświetlanie listy 4: plik Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

W założeniu ta strona jest listy rozwijanej wybiera jeden z trzech kolorów do wyświetlenia drugą etykietę, że pole wyboru określa zarówno Określa, czy jest pogrubiony, oraz wyświetlanie etykiet daty, a także czas. Pole wyboru nie powinno powodować aktualizacji interfejsu AJAX, ale powinien listy rozwijanej, nawet jeśli nie są przechowywane w elemencie UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Jest widoczna w zrzucie ekranu powyżej, można kliknąć przycisk najnowsze został prawy przycisk ten panelu aktualizacji, która aktualizowana top niezależnie od czasu dolnej. Data została również wyłączyć między kliknięciami, ponieważ data jest widoczny w etykiecie dolnej. Na koniec odsetek jest kolor etykiety dolnej: zostało zaktualizowane niedawno niż tekst etykiety, który pokazuje, czy formant jest ważne, a użytkownicy powinien być zachowane poprzez ogłaszania zwrotnego AJAX. *Jednak*, czas nie został zaktualizowany. Czas został automatycznie zapełnienia za pośrednictwem trwałość \_ \_pole stanu WIDOKU strony interpretowane przez moduł wykonawczy platformy ASP.NET, gdy formant został ponownie renderowany na serwerze. Kodu serwera ASP.NET AJAX nie rozpoznaje, w którym metody kontrolki są zmiany stanu; po prostu repopulates ze stanu widoku, a następnie uruchamia zdarzenia, które są odpowiednie.

Należy wskazać, jednak miał I zainicjowania czas na stronie\_obciążenia zdarzenia, czas będzie została zwiększona poprawnie. W związku z tym, deweloperzy powinien Uważaj, że odpowiedni kod jest uruchamiana podczas obsługi zdarzeń odpowiednie i Unikaj stosowania strony\_obciążenia podczas obsługi zdarzeń formantu będzie odpowiednia.

## <a name="summary"></a>Podsumowanie

Formantu UpdatePanel rozszerzenia AJAX ASP.NET jest elastyczne i mogą korzystać z metod identyfikacji formantu zdarzenia, które powinno spowodować, że plik aktualizacji. Obsługuje są automatycznie aktualizowane przez jej kontrolkach podrzędnych, ale można również odpowiadanie na zdarzenia formantów w innym miejscu na stronie.

Aby zmniejszyć ryzyko obciążenia serwera przetwarzania, zalecane jest `ChildrenAsTriggers` mieć ustawioną właściwość elementu UpdatePanel `false`, a zdarzenia można korzystać w zamiast domyślnie włączone. Zapobiega to również niepotrzebnych zdarzeń z powodować potencjalnie niepożądane skutki tym sprawdzania poprawności i zmiany do pól wejściowych. Tego rodzaju błędów, może być trudne do izolacji, ponieważ strony aktualizacje sposób niewidoczny dla użytkownika, a przyczynę w związku z tym nie może być od razu widoczne.

Sprawdzając przebiega w postaci kodu ASP.NET AJAX po przejęciu modelu, można określić korzysta z framework już dostarczane przez platformę ASP.NET. W ten sposób ją zachowuje maksymalną zgodność z formantami zaprojektowany przy użyciu tej samej struktury i narusza co najmniej na wszelkie dodatkowe JavaScript zapisywane dla strony.

## <a name="bio"></a>Mnie

Tomasz Paveza jest starszy Deweloper aplikacji .NET na Terralever ([www.terralever.com](http://www.terralever.com)), wiodące interakcyjne przedsiębiorstwo marketing Tempe, AZ. Piotr można uzyskać pod adresem [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), i znajduje się w jego blog [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-partial-page-updates-with-asp-net-ajax.md)
> [dalej](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
