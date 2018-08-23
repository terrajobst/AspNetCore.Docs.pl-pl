---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Opis wyzwalaczy UpdatePanel ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Podczas pracy w edytorze znaczników w programie Visual Studio, można zauważyć (z funkcji IntelliSense), istnieją dwa typy elementów podrzędnych kontrolki UpdatePanel. Jedną z pytania "Wh"...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 9501a2e855bdffe8c9d85c0dd0d836f50935b306
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754702"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Opis wyzwalaczy UpdatePanel ASP.NET AJAX
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Podczas pracy w edytorze znaczników w programie Visual Studio, można zauważyć (z funkcji IntelliSense), istnieją dwa typy elementów podrzędnych kontrolki UpdatePanel. Jednym z nich jest element wyzwalaczy, który określa formanty na stronie (lub kontrolki użytkownika, jeśli używana jest jedna) który spowoduje wyzwolenie częściowe renderowanie kontrolki UpdatePanel, w której znajduje się element.


## <a name="introduction"></a>Wprowadzenie

Firmy Microsoft technologii ASP.NET oferuje zorientowane obiektowo i oparte na zdarzeniach model programowania i łączy w sobie go z korzyściami skompilowanego kodu. Jednak jego przetwarzania po stronie serwera modelu ma kilka wady związane z technologii, z których wiele może zostać zlikwidowane przez nowych funkcji dostępnych w rozszerzenia AJAX programu Microsoft ASP.NET 3.5. Te rozszerzenia umożliwiają wiele nowych funkcji rozbudowanych aplikacji klienckich, w tym częściowe renderowanie stron bez konieczności odświeżenie całej strony, możliwość dostępu do usług sieci Web za pośrednictwem skrypt po stronie klienta (w tym ASP.NET, profilowania API) i rozbudowane API po stronie klienta przeznaczony do utworzenia duplikatów, wiele systemów kontroli w zestaw formantów po stronie serwera ASP.NET.

Ten oficjalny dokument sprawdza, czy funkcje wyzwalaczy XML ASP.NET AJAX `UpdatePanel` składnika. Wyzwalacze XML zapewniają szczegółową kontrolę nad składników, które mogą powodować częściowe renderowanie określonych formantów UpdatePanel.

Ten oficjalny dokument zależy od wersji Beta 2 programu .NET Framework 3.5 i Visual Studio 2008. Rozszerzenia AJAX programu ASP.NET, wcześniej zestaw dodatku, które są przeznaczone dla programu ASP.NET 2.0 są teraz wbudowane w bibliotece klas programu .NET Framework Base. Ten oficjalny dokument również założono, że będzie on pracować przy użyciu programu Visual Studio 2008, nie Visual Web Developer Express i zapewnia wskazówki, zgodnie z interfejsu użytkownika programu Visual Studio (chociaż fragmentów kodu będą całkowicie zgodne, niezależnie od tego środowisko programistyczne).

## <a name="triggers"></a>*Wyzwalacze*

Wyzwalacze dla danej kontrolki UpdatePanel domyślnie automatycznie uwzględnia formanty podrzędne, które wywołują odświeżenie strony, w tym (na przykład) pól tekstowych, które mają ich `AutoPostBack` właściwością **true**. Jednak wyzwalaczy może także stanowić sposób deklaratywny przy użyciu znaczników; odbywa się w obrębie `<triggers>` sekcji deklaracji kontrolki UpdatePanel. Mimo że jest możliwy za pośrednictwem wyzwalaczy `Triggers` właściwość kolekcji, zalecane jest, czy rejestrujesz się wszelkie wyzwalacze częściowe renderowanie w czasie wykonywania (na przykład jeśli formant nie jest dostępna w czasie projektowania) przy użyciu `RegisterAsyncPostBackControl(Control)` metody Obiekt strony, w ramach ScriptManager `Page_Load` zdarzeń. Należy pamiętać, że strony są bezstanowe, a zatem należy ponownie zarejestrować tych kontrolek za każdym razem, gdy są one tworzone.

Włączenie wyzwalacza automatycznego podrzędnych można także wyłączyć (tak, aby automatycznie formantów podrzędnych, które tworzyć ogłaszania zwrotnego nie wyzwalają częściowe renderuje), ustawiając `ChildrenAsTriggers` właściwości **false**. Dzięki temu można największą elastyczność podczas przypisywania które określonej kontrolki może wywołać renderowania strony i jest to zalecane, tak aby deweloper będzie wyrazić zgodę na odpowiadanie na zdarzenia, a nie wszelkie zdarzenia, które mogą wystąpić podczas obsługi.

Należy pamiętać, że gdy formantów UpdatePanel są zagnieżdżone, kiedy UpdateMode jest ustawiona na **warunkowego**, jeśli podrzędnych kontrolki UpdatePanel zostanie wywołany, ale element nadrzędny nie jest następnie podrzędne zostaną odświeżone kontrolki UpdatePanel. Jednak jeśli element nadrzędny kontrolki UpdatePanel są odświeżane, następnie podrzędnych kontrolki UpdatePanel będzie również odświeżane.

## <a name="the-lttriggersgt-element"></a>*&lt;Wyzwalaczy&gt; — Element*

Podczas pracy w edytorze znaczników w programie Visual Studio, można zauważyć (z funkcji IntelliSense), istnieją dwa typy elementów podrzędnych elementu `UpdatePanel` kontroli. Element najczęściej spotykanych jest `<ContentTemplate>` element, który hermetyzuje zasadniczo zawartości, która odbędzie się przez zespół aktualizacji (zawartość, dla którego umożliwiamy częściowe renderowanie). Inny element `<Triggers>` element, który określa formanty na stronie (lub kontrolki użytkownika, jeśli używana jest jedna) który spowoduje wyzwolenie częściowe renderowanie kontrolki UpdatePanel, w którym &lt;wyzwalaczy&gt; znajduje się element.

`<Triggers>` Element może zawierać dowolną liczbę każdego dwa węzły podrzędne: `<asp:AsyncPostBackTrigger>` i `<asp:PostBackTrigger>`. Przyjmują dwa atrybuty `ControlID` i `EventName`oraz można określić dowolną kontrolkę w bieżącej jednostce hermetyzacji (na przykład jeśli kontrolki UpdatePanel znajduje się w obrębie kontrolka użytkownika sieci Web, możesz nie należy próbować przywołać kontrolki w Strona, na którym będzie przechowywany kontrolki użytkownika).

`<asp:AsyncPostBackTrigger>` Element jest szczególnie przydatne w przypadku, w tym, że można wskazać, każde zdarzenie w kontrolce, która istnieje jako element podrzędny elementu *wszelkie* kontrolki UpdatePanel w jednostce hermetyzacji i nie tylko kontrolki UpdatePanel, w którym ten wyzwalacz jest elementem podrzędnym . W związku z tym każdy formant będzie możliwe do wyzwalania aktualizacji stron częściowych.

Podobnie `<asp:PostBackTrigger>` element może być użyty do renderowania strony częściowej wyzwalacza, ale taki, który wymaga pełnego przesłania danych do serwera. Ten element wyzwalacza można również wymusić renderowania pełnej strony, gdy formant, w przeciwnym razie powodowało zwykle renderowania strona częściowa (na przykład w przypadku, gdy `Button` formant istnieje w `<ContentTemplate>` element kontrolki UpdatePanel). Ponownie PostBackTrigger element można określić dowolną kontrolkę, która jest elementem podrzędnym kontrolki UpdatePanel w bieżącej jednostce hermetyzacji.

## <a name="lttriggersgt-element-reference"></a>*&lt;Wyzwalacze&gt; odwołanie do elementu*

*Elementy podrzędne znaczników:*

| **Tag** | **Opis** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Określa kontrolkę i zdarzenia, które spowoduje, że aktualizacji stron częściowych dla kontrolki UpdatePanel, który zawiera odwołanie do tego wyzwalacza. |
| &lt;asp:PostBackTrigger&gt; | Określa kontrolkę i zdarzenia, które spowoduje, że cała strona aktualizacji (odświeżenie całej strony). Ten tag może służyć do wymusić odświeżanie pełne, gdy formant, w przeciwnym razie będą wyzwalać częściowe renderowanie. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Wskazówki: Kontrolki UpdatePanel wielu wyzwalaczy*

1. Tworzenie nowej strony programu ASP.NET, za pomocą obiektu ScriptManager skonfigurować, aby włączyć częściowe renderowanie. Dodaj dwa UpdatePanels do tej strony — w pierwszym, obejmują kontrolkę typu etykieta (Label1) oraz dwie kontrolki przycisku (Button1 i Button2). Button1 powinna być widoczna nazwa kliknij, aby zaktualizować obydwa i Button2 powinna być widoczna nazwa kliknij, aby zaktualizować to lub coś, co w tym kierunku. W drugiej kontrolki UpdatePanel zawierają tylko kontrolkę typu etykieta etykiety (2), ale ustaw jego właściwość ForeColor na coś innego niż domyślny, odróżnić go.
2. Ustaw właściwość UpdateMode tagów obie kontrolki UpdatePanel **warunkowego**.

**Wyświetlanie 1: Znaczników default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. W obsłudze zdarzeń kliknięcia przycisku Button1 wartość Label1.Text i Label2.Text zależne od czasu (na przykład DateTime.Now.ToLongTimeString()). W przypadku obsługi zdarzeń kliknij Button2 należy ustawić tylko Label1.Text wartość zależne od czasu.

**Wyświetlanie 2: Plik Codebehind (spacje) default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby skompilować i uruchomić projekt. Należy pamiętać, że po kliknięciu aktualizacji zarówno paneli, zarówno etykiety zmienić tekst; Jednak po kliknięciu tego panelu aktualizacji, tylko Label1 aktualizacji.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Kulisy*

Przy użyciu przykładu, który po prostu skonstruowany, firma Microsoft zapoznaj się z działania ASP.NET AJAX i sposobie działania naszych wyzwalaczy panelu między UpdatePanel. Aby to zrobić, firma Microsoft będzie działać przy użyciu źródła wygenerowanego strony HTML, a także rozszerzenia przeglądarki Mozilla Firefox, o nazwie FireBug — dzięki niemu można łatwo omówiony ogłaszania zwrotnego AJAX. Firma Microsoft będzie także narzędzie .NET odblaskowego przez Lutz Roeder. Oba te narzędzia są dostępne bezpłatnie w trybie online i znajduje się za pomocą wyszukiwania w Internecie.

Kontrola kodu źródłowego strony pokazuje prawie nie trzeba wykonywać żadnych niestandardowych; formantów UpdatePanel są renderowane jako `<div>` kontenerów, zobaczysz zasobów skryptów zawiera podany przez `<asp:ScriptManager>`. Dostępne są także niektóre nowe specyficzne dla AJAX wywołania PageRequestManager wewnętrzne biblioteki skryptów klienta AJAX. Na koniec widzimy dwa kontenery UpdatePanel — jeden z renderowanych `<input>` przyciski z dwóch `<asp:Label>` renderowaniu kontrolek `<span>` kontenerów. (Jeśli drzewo DOM w FireBug możesz sprawdzić, zauważysz, że etykiety są wygaszone, aby wskazać, że nie eksportują widocznej zawartości).

Kliknij przycisk aktualizacji tego panelu i zwróć uwagę, że najważniejsze UpdatePanel zostanie zaktualizowane o bieżący czas serwera. W FireBug wybierz kartę konsoli, dzięki czemu można sprawdzić żądanie. Najpierw sprawdź parametry żądania POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Należy pamiętać, że kontrolki UpdatePanel wskazuje kodu AJAX po stronie serwera dokładnie drzewo kontroli, które zostało wywołane za pomocą parametru ScriptManager1: `Button1` z `UpdatePanel1` kontroli. Teraz kliknij przycisk aktualizacji zarówno paneli. Następnie badanie odpowiedzi, widzimy rozdzielonych potoku serii zestaw zmiennych w ciąg. w szczególności widzimy najważniejsze UpdatePanel `UpdatePanel1`, ma materiałami jego HTML wysyłany do przeglądarki. Skrypt biblioteki klienckiej AJAX zastępuje UpdatePanel oryginalna zawartość HTML z nowej zawartości za pośrednictwem `.innerHTML` właściwości, a zatem serwer wysyła zmieniona zawartość z serwera w formacie HTML.

Teraz kliknij przycisk panele zarówno aktualizacji i przejrzeć wyniki z serwera. Wyniki są bardzo podobne — zarówno UpdatePanels odbierania nowego kodu HTML z serwera. Podobnie jak w przypadku poprzedniego wywołania zwrotnego, dodatkowa strona stanie są wysyłane.

Jak widać, ponieważ żaden specjalny kod służy do wykonywania ogłaszania zwrotnego AJAX, biblioteki skryptów AJAX klienta jest w stanie przechwycenia ogłaszania zwrotnego formularza, bez konieczności wprowadzania dodatkowego kodu. Formanty serwera automatycznie wykorzystywać JavaScript, aby nie automatycznie przesłaniem formularza — ASP.NET automatycznie wprowadza kod weryfikacji formularza i stan, przede wszystkim osiągnięte przez automatyczny skrypt zasobu dołączania, klasa PostBackOptions , a klasa ClientScriptManager.

Na przykład należy wziąć pod uwagę formantu CheckBox; Sprawdź dezasemblacji klasy w odblaskowego .NET. Aby to zrobić, upewnij się, że zestawu System.Web został otwarty, a następnie przejdź do `System.Web.UI.WebControls.CheckBox` klasy, otwierając `RenderInputTag` metody. Wyszukaj warunkowych, która sprawdza `AutoPostBack` właściwości:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Po włączeniu automatycznego ogłaszania zwrotnego `CheckBox` Sterowanie (za pośrednictwem właściwości AutoPostBack o wartości true), wynikowe `<input>` tagu w związku z tym jest renderowany przy użyciu zdarzenia platformy ASP.NET, obsługa skryptów w jego `onclick` atrybutu. Przejmowanie przesyłania formularza, następnie umożliwia ASP.NET AJAX ich wstrzyknięcie do strony nonintrusively, pomaga uniknąć wszelkich potencjalnych przełomowe zmiany, które mogą wystąpić przy użyciu zastąpienia prawdopodobnie nieprecyzyjną ciągu. Ponadto, dzięki temu *wszelkie* niestandardowy formant ASP.NET korzystanie z możliwości technologii ASP.NET AJAX bez konieczności wprowadzania dodatkowego kodu do obsługi jej użycie w kontenerze kontrolki UpdatePanel.

`<triggers>` Funkcji odnosi się do wartości zainicjowane w wywołaniu PageRequestManager \_updateControls (należy zauważyć, że biblioteki skryptów klienta ASP.NET AJAX korzysta z Konwencji tej metody, zdarzenia i nazwy pól, które zaczynają się znakiem podkreślenia są oznaczone jako wewnętrzne, a nie są przeznaczone do użytku poza sama biblioteka). Dzięki niemu możemy obserwować, formanty, które mają na celu spowodować ogłaszania zwrotnego AJAX.

Na przykład możemy dodać dwa dodatkowe formanty do strony, pozostawiając jeden formant poza UpdatePanels całkowicie i pozostawienie niezmienionej w UpdatePanel. Firma Microsoft będzie dodać kontrolkę pola wyboru w prawym górnym UpdatePanel i upuszczania kontrolki DropDownList przy liczbie kolorów zdefiniowanych w obrębie listy. Oto nowy kod znaczników:

**Wyświetlanie 3: Nowych znaczników**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

A Oto nowe kodem:

**Wyświetlanie listy 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Idei ta strona jest listy rozwijanej wybiera jeden z trzech kolorów, aby pokazać drugiej etykiety, że pole wyboru określa, czy jest pogrubiony i wyświetlanie etykiet na datę, a także czas. Pole wyboru nie powinno powodować aktualizacji interfejsu AJAX, ale powinny listy rozwijanej, nawet jeśli nie mieści się w obrębie UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Jest widoczny na powyższym zrzucie ekranu, można kliknąć przycisk najnowszych był prawy przycisk aktualizacji tego panelu, aktualizowane niezależnie od czasu najważniejsze czasu dolnej. Data została również przełączyć je między kliknięciami, ponieważ data jest widoczna w dolną etykietę. Na koniec zainteresowań jest kolor dolną etykietę: Zaktualizowano niedawno tekst etykiety, który pokazuje, że stan formantu jest ważne, a użytkownicy mogą być zachowywane ogłaszania zwrotnego AJAX. *Jednak*, czas nie został zaktualizowany. Czas został automatycznie ponownie umieszczone za pośrednictwem trwałości \_ \_pole stanu WIDOKU strony interpretowane przez środowisko uruchomieniowe programu ASP.NET, gdy kontrolka została ponownie renderowany na serwerze. Kod serwera ASP.NET AJAX nie rozpoznaje, w której metody formanty zmieniają stan; ją po prostu repopulates ze stanu widoku, a następnie uruchamia zdarzenia, które są odpowiednie.

Należy wskazać, jednak miał I zainicjowanej czasu na stronie\_załadowane zdarzenie czas będzie została zwiększona poprawnie. W związku z tym, deweloperzy powinien być ostrożnym, że odpowiedni kod jest uruchamiany podczas obsługi zdarzeń właściwe i Unikaj stosowania strony\_obciążenia, gdy procedurę obsługi zdarzeń kontrolki będzie odpowiedni.

## <a name="summary"></a>Podsumowanie

Formant kontrolki UpdatePanel ASP.NET AJAX rozszerzenia jest uniwersalny i mogą korzystać z szeregu metod identyfikacji zdarzeń kontrolki, które powinno spowodować, że można zaktualizować. Obsługuje jest automatycznie aktualizowana przez jego formantów podrzędnych, ale również odpowiadać na zdarzenia obiektu Controls w innym miejscu na stronie.

Aby zmniejszyć ryzyko obciążenie serwera, zalecane jest, `ChildrenAsTriggers` można ustawić właściwości UpdatePanel `false`, i aby zdarzenia były wyrażeniu zgody na uczestnictwo w zamiast domyślnie włączone. To zapobiega także wszelkie niepotrzebne zdarzenia powodować potencjalnie niepożądane skutki takich jak sprawdzanie poprawności i zmian do pól wejściowych. Tego rodzaju błędów może być trudne do izolowania, ponieważ strona zostanie zaktualizowana w sposób niewidoczny dla użytkownika i przyczyny w związku z tym nie można od razu widoczne.

Sprawdzając przebiega w postaci kodu ASP.NET AJAX po przejęciu modelu, byliśmy w stanie ustalić, korzysta z framework już dostarczanego przez platformę ASP.NET. W ten sposób jej zachowuje maksymalną zgodność z formantami, zaprojektowana z wykorzystaniem tej samej struktury i narusza co najmniej na wszelkie dodatkowe JavaScript przeznaczony dla strony.

## <a name="bio"></a>Biografia

Rob Paveza jest starszy Deweloper aplikacji .NET w Terralever ([www.terralever.com](http://www.terralever.com)), wiodący interaktywne przedsiębiorstwo marketingu Tempe, AZ. ADAM można z Tobą skontaktować w [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), a jego blog znajduje się w folderze [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-partial-page-updates-with-asp-net-ajax.md)
> [dalej](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
