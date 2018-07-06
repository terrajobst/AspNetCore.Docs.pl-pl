---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autoryzacja oparta na użytkownikach (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przyjrzymy się ograniczenie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 2fe1dbc7e5fd609870975fbf41849522bbeed534
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823994"
---
<a name="user-based-authorization-c"></a>Autoryzacja oparta na użytkownikach (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> W tym samouczku przyjrzymy się ograniczenie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik.


## <a name="introduction"></a>Wprowadzenie

Większość aplikacji sieci web, które oferują kont użytkowników to zrobić w części w celu ograniczenia pewnych odwiedzających dostęp do niektórych stron w witrynie. W większości online messageboard witryn na przykład wszyscy użytkownicy — anonimowy i uwierzytelnionego — będą mogli wyświetlić wpisy messageboard jednak tylko uwierzytelnieni użytkownicy mogą odwiedzić stronę sieci web, aby utworzyć nowy wpis. I może być stron administracyjnych dostępnych tylko dla określonego użytkownika (lub w określonej grupie użytkowników). Ponadto funkcje na poziomie strony mogą różnić się na podstawie użytkownika przez użytkownika. Podczas przeglądania listy wpisów, uwierzytelnionych użytkowników są wyświetlane interfejs dla każdego wpisu, klasyfikacja, ten interfejs nie jest dostępna anonimowych gościom.

Program ASP.NET ułatwia do zdefiniowania reguł autoryzacji opartej na użytkownika. Za pomocą tylko znacznej liczby znaczników w `Web.config`, określonych stron sieci web lub katalogów całego można zablokować tak, aby tylko są one dostępne do określonej podgrupy użytkowników. Funkcje na poziomie strony można włączyć lub wyłączyć zależnie od aktualnie zalogowanego użytkownika w sposób programowy i deklaratywnego.

W tym samouczku przyjrzymy się ograniczenie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik. Zaczynajmy!

## <a name="a-look-at-the-url-authorization-workflow"></a>Przyjrzeć się przepływu pracy autoryzacji adresów URL

Zgodnie z opisem w [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, gdy środowisko uruchomieniowe ASP.NET przetwarza żądanie dla zasobu żądania ASP.NET zgłasza liczbę zdarzeń podczas ich cyklu życia. *Moduły HTTP* są klasami zarządzanymi, którego kod jest wykonywany w odpowiedzi na konkretne zdarzenie w cyklu życia żądania. ASP.NET jest dostarczany z liczbą moduły HTTP, który wykonywania podstawowych zadań w tle.

Jest jeden taki moduł HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Zgodnie z opisem w poprzednich samouczkach podstawową funkcją `FormsAuthenticationModule` ma na celu określenie tożsamość bieżącego żądania. Jest to realizowane przez sprawdzenie biletu uwierzytelniania formularzy, który znajduje się w pliku cookie albo osadzony w adresie URL. Ten identyfikator ma miejsce podczas [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Będzie wprowadzenie innego modułu HTTP ważne [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), który jest zgłaszany w odpowiedzi na [ `AuthorizeRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (który stanie się po `AuthenticateRequest` zdarzeń). `UrlAuthorizationModule` Sprawdza, czy konfiguracja znaczników w `Web.config` ustalenie, czy bieżąca tożsamość ma uprawnienia do określoną stronę. Ten proces jest nazywany *Autoryzacja adresów URL*.

Zajmiemy się składnia dla reguły autoryzacji adresów URL w kroku 1, ale najpierw możemy przyjrzeć się co `UrlAuthorizationModule` jest w zależności od tego, czy żądanie jest autoryzowane. Jeśli `UrlAuthorizationModule` Określa, czy żądanie jest autoryzowane, a następnie nic nie robi i żądanie jest kontynuowane przy użyciu jej cyklu projektowania. Jednak jeśli żądanie jest *nie* autoryzowany, a następnie `UrlAuthorizationModule` przerywa cyklu życia i powoduje, że `Response` obiekt do zwrotu [HTTP 401 nieautoryzowane](http://www.checkupdown.com/status/E401.html) stanu. Podczas korzystania z uwierzytelniania formularzy ten stan HTTP 401 nigdy nie są zwracane do klienta, ponieważ jeśli `FormsAuthenticationModule` wykrywa HTTP 401, jest w stanie modyfikuje się [przekierowania 302 HTTP](http://www.checkupdown.com/status/E302.html) do strony logowania.

Rysunek 1 przedstawia przepływ pracy potoku platformy ASP.NET `FormsAuthenticationModule`i `UrlAuthorizationModule` po nadejściu nieautoryzowanego żądania. W szczególności rysunek 1 pokazuje żądania anonimowe osoby odwiedzającej dla `ProtectedPage.aspx`, który jest strona, która nie zezwala na dostęp dla użytkowników anonimowych. Ponieważ użytkownik jest anonimowy, `UrlAuthorizationModule` anuluje żądanie i zwraca stan HTTP 401 nieautoryzowane. `FormsAuthenticationModule` Następnie konwertuje stanu 401 na przekierowanie 302 strony logowania. Po uwierzytelnieniu użytkownika za pośrednictwem strony logowania, użytkownik jest przekierowywany do `ProtectedPage.aspx`. Tym razem `FormsAuthenticationModule` identyfikację użytkownika, w oparciu o jego biletu uwierzytelniania. Teraz, gdy użytkownik jest uwierzytelniony, `UrlAuthorizationModule` zezwala na dostęp do tej strony.


[![Uwierzytelnianie formularzy i przepływu pracy autoryzacji adresów URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Rysunek 1**: uwierzytelnianie formularzy i przepływu pracy autoryzacji adresu URL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image3.png))


Rysunek 1 przedstawia interakcję, który występuje, gdy anonimowy użytkownik próbuje uzyskać dostęp do zasobu, który nie jest dostępne dla użytkowników anonimowych. W takim przypadku anonimowy użytkownik jest przekierowywany do strony logowania ze stroną którą użytkownik próbował można znaleźć określonej w zmiennej querystring. Po pomyślnym zalogowaniu się użytkownika użytkownik zostanie automatycznie przekierowany ponownie do zasobów, których ona początkowo próbował wyświetlić.

Gdy użytkownik anonimowy nieautoryzowanego żądania, ten przepływ pracy jest proste, a następnie łatwo dla obiektu odwiedzającego zrozumieć, co się stało i dlaczego. Jednak należy pamiętać, że `FormsAuthenticationModule` nastąpi przekierowanie *wszelkie* nieautoryzowany użytkownik do strony logowania, nawet wtedy, gdy żądanie jest wysyłane przez uwierzytelnionego użytkownika. Może to spowodować mylące środowisko użytkownika uwierzytelnionego użytkownika próba stronę, dla którego nie ma ona urzędu.

Wyobraź sobie, że naszej witryny sieci Web były jej reguł autoryzacji adresów URL skonfigurowany tak, aby strona ASP.NET `OnlyTito.aspx` było accessibly tylko Tito. Teraz Wyobraź sobie, że Sam odwiedza witryny, loguje się i następnie próbuje znaleźć `OnlyTito.aspx`. `UrlAuthorizationModule` Spowoduje zatrzymanie cyklu życia żądania i zwraca stan HTTP 401 nieautoryzowane, który `FormsAuthenticationModule` wykryje i Sam przekierowanie do strony logowania. Ponieważ Sam zalogował się już, jednak marcela się zastanawiać, dlaczego została ona wysłana powrót do strony logowania. Może ona przyczyny, że jej poświadczenia logowania zostały utracone jakiś sposób lub że ona wprowadzono nieprawidłowe poświadczenia. Jeśli Sam wraca swoje poświadczenia na stronie logowania będzie ona zalogowany (ponownie) i Przekierowanie do `OnlyTito.aspx`. `UrlAuthorizationModule` Wykryje, że Sam nie mogą odwiedzić tę stronę i powróci ona do strony logowania.

Rysunek 2 przedstawia to mylące przepływu pracy.


[![Domyślny przepływ pracy może prowadzić do mylące cyklu](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Rysunek 2**: domyślny przepływ pracy może prowadzić do cyklu mylące ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image6.png))


Przepływ pracy, przedstawione na rysunku 2 można szybko befuddle nawet większość komputerów pomysłowy obiekt odwiedzający. Przedstawiony zostanie sposób, aby zapobiec takiej sytuacji mylące cyklu w kroku 2.

> [!NOTE]
> Aplikacja ASP.NET używa dwa mechanizmy, aby ustalić, czy bieżący użytkownik może uzyskać dostęp do określonej strony sieci web: Autoryzacja adresów URL i autoryzacji plików. Plik autoryzacji jest implementowany przez [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), która określa urząd przez consulting żądane pliki listy ACL. Plik autoryzacji jest najczęściej używana z uwierzytelnianiem Windows, ponieważ listy ACL są uprawnienia, które są stosowane do kont Windows. Korzystając z uwierzytelniania formularzy, wszystkie żądania systemu na poziomie systemu operacyjnego i pliku są wykonywane przez tego samego konta Windows, niezależnie od użytkownika w witrynie. Ponieważ w tej serii samouczków skupia się na uwierzytelnianie formularzy, firma Microsoft będzie nie być Omawiając autoryzacji plików.


### <a name="the-scope-of-url-authorization"></a>Zakres Autoryzacja adresów URL

`UrlAuthorizationModule` Jest zarządzanego kodu, który jest częścią środowiska uruchomieniowego programu ASP.NET. Przed firmy Microsoft w wersji 7 [Internet Information Services (IIS)](https://www.iis.net/) serwera sieci web wystąpił distinct barierę między potoku HTTP usług IIS i środowiska uruchomieniowego ASP.NET potoku. W skrócie, w usługach IIS 6 i starszych ASP. NET firmy `UrlAuthorizationModule` wykonywana tylko wtedy, gdy żądanie jest delegowane za pomocą programu IIS do środowiska uruchomieniowego programu ASP.NET. Domyślnie usługi IIS przetwarza zawartość statyczną, sam — jak strony HTML i CSS, JavaScript i plików obrazów — i tylko przekazuje żądania do obsługi platformy ASP.NET, gdy strona z rozszerzeniem `.aspx`, `.asmx`, lub `.ashx` żądania.

Usługi IIS 7, jednak umożliwia zintegrowanych usług IIS i platformy ASP.NET potoków. Przy użyciu kilku ustawień konfiguracji, można skonfigurować usługi IIS 7 do wywołania `UrlAuthorizationModule` dla *wszystkich* żądań, co oznacza, że reguły autoryzacji adresów URL mogą być definiowane dla plików dowolnego typu. Ponadto usług IIS 7 zawiera swój własny aparat autoryzacji adresu URL. Aby uzyskać więcej informacji na temat Integracja platformy ASP.NET i IIS 7 natywnego adresu URL autoryzacji funkcji, zobacz [Autoryzacja adresów URL usług IIS7 opis](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Aby uzyskać więcej informacji na temat przyjrzeć Integracja platformy ASP.NET i IIS 7, wybierz kopię książki Shahram Khosravi *Professional usług IIS 7 i platformy ASP.NET-zintegrowane programowania* (ISBN: 978 0470152539).

Mówiąc w wersjach wcześniejszych niż IIS 7, reguły autoryzacji adresów URL są stosowane tylko do zasobów, obsługiwane przez środowisko uruchomieniowe programu ASP.NET. Ale za pomocą usług IIS 7 można korzystać z natywnych funkcji autoryzacji adresu URL usług IIS ani integrowanie ASP. NET firmy `UrlAuthorizationModule` do potoku HTTP usług IIS, rozszerzając w ten sposób działania tej funkcji na wszystkich żądań.

> [!NOTE]
> Istnieją pewne różnice w sposób subtelne jeszcze ważne ASP. NET firmy `UrlAuthorizationModule` oraz funkcji autoryzacji adresów URL usług IIS 7 przetwarzanie reguł autoryzacji. W tym samouczku nie analizuje funkcji autoryzacji adresów URL usług IIS 7 lub różnice w sposób jej analizuje reguły autoryzacji w porównaniu do `UrlAuthorizationModule`. Więcej informacji na temat tych tematów, można znaleźć w dokumentacji usług IIS 7, MSDN lub na [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Krok 1: Definiowanie reguł autoryzacji adresów URL w`Web.config`

`UrlAuthorizationModule` Określa, czy należy udzielić lub odmówić dostępu do żądanego zasobu dla określonej tożsamości, w oparciu o reguł autoryzacji adresów URL, które są zdefiniowane w konfiguracji aplikacji. Reguły autoryzacji są zapisane w [ `<authorization>` elementu](https://msdn.microsoft.com/library/8d82143t.aspx) w formie `<allow>` i `<deny>` elementów podrzędnych. Każdy `<allow>` i `<deny>` elementu podrzędnego można określić:

- Określonego użytkownika
- Rozdzielana przecinkami lista użytkowników
- Wszyscy użytkownicy anonimowi wskazywane przez znak zapytania (?)
- Wszyscy użytkownicy, oznaczone gwiazdką (\*)

Następujący kod ilustruje sposób użycia reguł autoryzacji adresów URL umożliwiają użytkownikom Tito i Scotta i odmawiać go wszystkie pozostałe:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` Element definiuje, jakie użytkownicy mogą uzyskiwać — Tito i Scott — gdy `<deny>` elementu powoduje, że że *wszystkich* użytkownik nie będzie mógł.

> [!NOTE]
> `<allow>` i `<deny>` elementy można również określić reguły autoryzacji dla ról. Będziemy sprawdzać autoryzacji opartej na rolach w przyszłości zapoznać się z samouczkiem.


Następujące ustawienie udziela dostępu dla każdego z wyjątkiem Sam (w tym gości anonimowe):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Aby zezwolić tylko uwierzytelnionym użytkownikom, użyj następującej konfiguracji, który nie zezwala na dostęp do wszystkich użytkowników anonimowych:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Reguły autoryzacji są zdefiniowane w ramach `<system.web>` element `Web.config` i mają zastosowanie do wszystkich zasobów platformy ASP.NET w aplikacji sieci web. Często aplikacja ma reguły autoryzacji różne dla różnych sekcji. Na przykład w witrynie handlu elektronicznego, wszystkie osoby odwiedzające może przejrzeć produktów, zobacz recenzje produktów, wyszukać w katalogu i tak dalej. Jednak tylko uwierzytelnieni użytkownicy mogą nawiązać połączenia wyewidencjonowanie lub stron, aby zarządzać jednostkowego historii wysyłania. Ponadto może być porcje witryny tylko są dostępne dla wybieranych użytkowników, takich jak Administratorzy witryny.

Program ASP.NET ułatwia do zdefiniowania reguł autoryzacji różne dla różnych plików i folderów w lokacji. Reguły autoryzacji, określone w folderze głównym `Web.config` pliku, Zastosuj do wszystkich zasobów platformy ASP.NET w witrynie. Jednak te domyślne ustawienia autoryzacji może być zastąpiona w przypadku określonego folderu, dodając `Web.config` z `<authorization>` sekcji.

Zaktualizujmy naszą witrynę sieci Web, tak aby tylko uwierzytelnieni użytkownicy mogą odwiedzać strony ASP.NET w `Membership` folderu. W tym musimy dodać `Web.config` plik `Membership` folder i jego ustawienia autoryzacji, aby uniemożliwić użytkownikom anonimowym. Kliknij prawym przyciskiem myszy `Membership` folder w Eksploratorze rozwiązań wybierz menu Dodaj nowy element z menu kontekstowego, a następnie dodaj nowy plik konfiguracji sieci Web o nazwie `Web.config`.


[![Dodaj plik Web.config w folderze członkostwa](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Rysunek 3**: Dodaj `Web.config` plik `Membership` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image9.png))


W tym momencie projekt może zawierać dwóch `Web.config` pliki: jeden w katalogu głównym, a drugi w `Membership` folderu.


[![Aplikacja powinna teraz zawierać dwóch plikach Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Rysunek 4**: Twojej aplikacji powinny teraz zawierać dwa `Web.config` plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image12.png))


Zaktualizuj plik konfiguracji w `Membership` folderu, tak że zabrania dostępu dla użytkowników anonimowych.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

To wszystko jest do niego!

W celu przetestowania tej zmiany, odwiedź stronę główną w przeglądarce i upewnij się, że zalogowano. Ponieważ domyślne zachowanie aplikacji ASP.NET jest umożliwienie wszystkich odwiedzających i firma Microsoft nie wprowadzać żadnych zmian autoryzacji katalog główny `Web.config` pliku, możemy znaleźć pliki w katalogu głównym jako użytkownik anonimowy.

Kliknij link tworzenie kont użytkowników w lewej kolumnie. Spowoduje to przejście do `~/Membership/CreatingUserAccounts.aspx`. Ponieważ `Web.config` w pliku `Membership` folderu definiuje reguły autoryzacji, aby uniemożliwić dostęp anonimowy `UrlAuthorizationModule` anuluje żądanie i zwraca stan HTTP 401 nieautoryzowane. `FormsAuthenticationModule` Modyfikuje to 302 stan przekierowania przesyłania nam do strony logowania. Należy pamiętać, że strona możemy zostały próby uzyskania dostępu do (`CreatingUserAccounts.aspx`) jest przekazywana do strony logowania za pomocą `ReturnUrl` parametr querystring.


[![Ponieważ adres URL autoryzacji zasady Stanów Zjednoczonych zabraniają anonimowy dostęp firma Microsoft nastąpi przekierowanie do strony logowania](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Rysunek 5**: ponieważ Autoryzacja adresów URL reguły Zabroń dostępu anonimowego, firma Microsoft nastąpi przekierowanie do strony logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image15.png))


Po pomyślnym zalogowaniu możemy są przekierowywane do `CreatingUserAccounts.aspx` strony. Tym razem `UrlAuthorizationModule` zezwala na dostęp do tej strony, ponieważ firma Microsoft nie są już anonimowe.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Stosowanie reguł autoryzacji adresów URL do określonej lokalizacji

Ustawienia autoryzacji zdefiniowane w `<system.web>` części `Web.config` mają zastosowanie do wszystkich zasobów platformy ASP.NET, w tym katalogu i jego podkatalogach (do momentu, w przeciwnym razie zastąpiona przez inną `Web.config` pliku). W niektórych przypadkach jednak może chcemy wszystkich zasobów platformy ASP.NET w podanym katalogu konfiguracji określonego autoryzacji, z wyjątkiem jednego lub dwóch określonych stron. Można to osiągnąć przez dodanie `<location>` element `Web.config`, wskazując je do pliku, w której reguły autoryzacji różnią się i tam Definiowanie regułach autoryzacji unikatowy.

Aby zilustrować, za pomocą `<location>` elementu, aby zastąpić ustawienia konfiguracji dla określonego zasobu, możemy dostosować ustawienia autoryzacji, tak, aby odwiedzić tylko Tito `CreatingUserAccounts.aspx`. Aby to zrobić, należy dodać `<location>` elementu `Membership` folderu `Web.config` pliku i zaktualizować jego znaczników wygląda podobnie do poniższego:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>` Element `<system.web>` definiuje reguły domyślny adres URL autoryzacji programu ASP.NET w `Membership` folderze i jego podfolderach. `<location>` Elementu pozwala zastąpić te reguły dla określonego zasobu. W powyższym znaczników `<location>` odwołania do elementu `CreatingUserAccounts.aspx` strony i określa jej autoryzacji reguł pozwalać Tito, ale Odmów inne osoby.

W celu przetestowania tej zmiany autoryzacji, należy uruchomić, odwiedzając witrynę sieci Web jako użytkownik anonimowy. Jeśli użytkownik podejmie próbę stronę w `Membership` folderu, takich jak `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` odmówi żądania i nastąpi przekierowanie do strony logowania. Po zalogowaniu się jako powiedzieć, Scott, możesz odwiedzić stronę dowolnej stronie w `Membership` folderu *z wyjątkiem* dla `CreatingUserAccounts.aspx`. Podjęto próbę odwiedź `CreatingUserAccounts.aspx` zalogować się jako każda osoba, ale Tito spowoduje podjęto próbę nieautoryzowanego dostępu, przekierowywanie do strony logowania.

> [!NOTE]
> `<location>` Element musi znajdować się poza konfiguracji `<system.web>` elementu. Należy użyć oddzielnego `<location>` elementu dla każdego zasobu, którego ustawienia autoryzacji, które chcesz zastąpić.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Jak się`UrlAuthorizationModule`udzielić lub odmówić dostępu przy użyciu reguł autoryzacji

`UrlAuthorizationModule` Określa, czy do autoryzowania określonej tożsamości dla określonego adresu URL, analizując Autoryzacja adresów URL reguły pojedynczo, zaczynając od pierwszego i pracą z nimi drodze w dół. Gdy tylko zostanie znalezione dopasowanie, użytkownik jest udzielić lub odmówić dostępu, w zależności od jeśli dopasowanie został znaleziony w `<allow>` lub `<deny>` elementu. <strong>Jeśli nie zostanie znalezione dopasowanie, użytkownik uzyskuje dostęp.</strong> W związku z tym, jeśli chcesz ograniczyć dostęp, jest użycie `<deny>` element jako po ostatnim elemencie w konfiguracji autoryzacja adresu URL. <strong>Jeżeli pominięto</strong><strong>`<deny>`</strong><strong>elementu, wszyscy użytkownicy zostanie przyznany dostęp.</strong>

Aby lepiej zrozumieć proces wykorzystywany przez `UrlAuthorizationModule` ustalenie urząd, rozważ przykład reguły autoryzacji adresów URL przyjrzeliśmy się wcześniej w tym kroku. Pierwsza reguła jest `<allow>` element, który umożliwia dostęp do Tito i Scott. To drugie zasady `<deny>` element, który nie zezwala na dostęp do wszystkich użytkowników. Jeśli użytkownik anonimowy odwiedza, `UrlAuthorizationModule` rozpoczyna się, zadając jest anonimowa Scott lub Tito? Odpowiedź na pytanie, jest oczywiście nie, więc rozpoczynające się od drugiej reguły. Jest anonimowa w zestawie wszyscy? Od czasu odpowiedzi w tym miejscu jest tak, `<deny>` reguły jest umieszczany w mocy i użytkownik jest przekierowywany do strony logowania. Podobnie, jeśli odwiedzania Jisun `UrlAuthorizationModule` rozpoczyna się, zadając Jisun jest Scott lub Tito? Ponieważ ona jest, `UrlAuthorizationModule` przechodzi na drugie pytanie jest Jisun w zestawie wszyscy? Marcela jest tak, jest ona, odmowa dostępu. Ponadto jeśli Tito odwiedza, pierwsze pytanie powodowane `UrlAuthorizationModule` jest twierdząca odpowiedzi, więc Tito udzielany jest dostęp.

Ponieważ `UrlAuthorizationModule` reguły autoryzacji od góry do dołu zatrzymywanie u jakiegokolwiek dopasowania jest istotne dla bardziej szczegółowych reguł występować przed mniej konkretnych te procesy. Oznacza to aby zdefiniować reguły autoryzacji, które może zabronić Jisun i użytkowników anonimowych, ale zezwala na wszystkie inne uwierzytelnionych użytkowników, można będzie rozpoczynać się najbardziej specyficzne reguły — jeden Jisun mogących mieć wpływ na — a następnie przejdź do reguły specyficzne dla języka less - tych, dzięki czemu wszystkie inne uwierzytelnieni użytkownicy, ale wszyscy użytkownicy anonimowi odmowy. Następujące reguły autoryzacji adresów URL implementuje te zasady, najpierw odmawianie Jisun i następnie odmawiając każdy użytkownik anonimowy. Każdy uwierzytelniony użytkownik innych niż Jisun zostanie przyznany dostęp ponieważ żadnej z tych metod `<deny>` instrukcji będą zgodne.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Krok 2: Ustalenie przepływu pracy dla nieautoryzowanych, uwierzytelnionych użytkowników

Jak wspomniano wcześniej w tym samouczku w Przyjrzyj się w sekcji przepływu pracy autoryzacji adresów URL dowolnym techniczną nieautoryzowanego żądania, `UrlAuthorizationModule` anuluje żądanie i zwraca stan HTTP 401 nieautoryzowane. Tego stanu 401 jest modyfikowany przez `FormsAuthenticationModule` do 302 przekierowania stan, który wysyła użytkownika do strony logowania. Ten przepływ pracy jest wykonywana zgodnie z dowolnym nieautoryzowanego żądania, nawet wtedy, gdy użytkownik jest uwierzytelniony.

Zwracanie uwierzytelnionego użytkownika do strony logowania jest prawdopodobne, należy go mylić je, ponieważ mają one już zarejestrowanych w systemie. Za pomocą trochę więcej pracy można ulepszyć ten przepływ pracy, przekierowując uwierzytelnionych użytkowników, którzy tworzą nieautoryzowanych żądań do strony, który objaśnia, że będą oni próbowali dostęp do strony.

Rozpocznij od utworzenia nowej strony programu ASP.NET w folderze głównym aplikacji sieci web o nazwie `UnauthorizedAccess.aspx`; nie należy zapominać skojarzyć tę stronę przy użyciu `Site.master` strony wzorcowej. Po utworzeniu tej strony, należy usunąć formant zawartości, który odwołuje się do `LoginContent` ContentPlaceHolder tak, aby domyślna zawartości strony wzorcowej będą wyświetlane. Następnie dodaj komunikat wyjaśniający sytuacji, a mianowicie użytkownik próbował uzyskać dostęp do chronionego zasobu. Po dodaniu takiego komunikatu `UnauthorizedAccess.aspx` oznaczeniu deklaracyjnym strony powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Teraz musisz zmienić przepływ pracy, dlatego, że jeśli nieautoryzowanego żądania odbywa się przez uwierzytelnionego użytkownika są wysyłane do `UnauthorizedAccess.aspx` strony, a nie na stronie logowania. Logikę, która wykonuje przekierowanie nieautoryzowanych żądań do strony logowania jest ukryty w ramach prywatnej metody `FormsAuthenticationModule` klasy, dzięki czemu firma Microsoft nie można dostosować to zachowanie. Co możemy zrobić, jednak jest dodać własną logikę do strony logowania, który przekierowuje użytkownika do `UnauthorizedAccess.aspx`, jeśli to konieczne.

Gdy `FormsAuthenticationModule` przekierowuje nieautoryzowane osoby odwiedzającej do strony logowania dołącza adres URL żądanego, nieautoryzowanego querystring o nazwie `ReturnUrl`. Na przykład, jeśli nieautoryzowany użytkownik próbował odwiedź `OnlyTito.aspx`, `FormsAuthenticationModule` będzie przekierowywać je do `Login.aspx?ReturnUrl=OnlyTito.aspx`. W związku z tym jeśli strony logowania zostanie osiągnięty przez uwierzytelnionego użytkownika przy użyciu ciągu kwerendy, która obejmuje `ReturnUrl` parametru, a następnie możemy wiedzieć, czy ten nieuwierzytelniony użytkownik po prostu próbował odwiedzenia strony, użytkownik nie ma uprawnień do wyświetlenia. W takim przypadku chcemy przekierować jej `UnauthorizedAccess.aspx`.

Aby to zrobić, Dodaj następujący kod do strony logowania `Page_Load` program obsługi zdarzeń:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Powyższy kod przekierowuje uwierzytelnionego, nieautoryzowanym użytkownikom `UnauthorizedAccess.aspx` strony. Aby wyświetlić tę logikę w działaniu, odwiedź witrynę jako użytkownik anonimowy i kliknij łącze tworzenie kont użytkowników w lewej kolumnie. Spowoduje to przejście do `~/Membership/CreatingUserAccounts.aspx` strony, która w kroku 1, firma Microsoft skonfigurowany wyłącznie w celu zezwolenia na dostęp do Tito. Ponieważ użytkownicy anonimowi są niedozwolone, `FormsAuthenticationModule` przekierowuje nam powrót do strony logowania.

W tym momencie możemy anonimowe, więc `Request.IsAuthenticated` zwraca `false` i firma Microsoft nie nastąpi przekierowanie do `UnauthorizedAccess.aspx`. Zamiast tego zostanie wyświetlona strona logowania. Zaloguj się jako użytkownik inny niż Tito, takich jak Bruce. Po wprowadzeniu odpowiednimi poświadczeniami, logowania stronie przekierowuje nam z powrotem do `~/Membership/CreatingUserAccounts.aspx`. Jednak ponieważ ta strona jest dostępna wyłącznie dla Tito, firma Microsoft jest brak autoryzacji do wyświetlania go i niezwłocznie powrót do strony logowania. Tym razem jednak `Request.IsAuthenticated` zwraca `true` (i `ReturnUrl` parametr querystring istnieje), dzięki czemu możemy są przekierowywane do `UnauthorizedAccess.aspx` strony.


[![Uwierzytelniony, nieautoryzowani użytkownicy są przekierowywane do UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Rysunek 6**: uwierzytelniony, nieautoryzowani użytkownicy są przekierowywane do `UnauthorizedAccess.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image18.png))


Ten przepływ dostosowane przedstawia bardziej rozsądne i proste środowisko użytkownika przez krótki circuiting cyklu przedstawiona na rysunku 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Krok 3: Ograniczanie funkcjonalności na podstawie aktualnie zalogowanego użytkownika

Autoryzacja adresów URL ułatwia określenie reguł autoryzacji zdalnego. Jak widzieliśmy w kroku 1, przy użyciu Autoryzacja adresów URL firma Microsoft może krótkiej formie stanu tożsamości, które są dozwolone, oraz te, które są zabronione możliwości wyświetlenia określonej strony lub wszystkich stron w folderze. W niektórych scenariuszach jednak może chcemy zezwala wszystkim użytkownikom na stronę, ale ograniczenia funkcji strony, na podstawie użytkownika, odwiedzając go.

Należy wziąć pod uwagę wielkość liter w witrynie sieci Web handlu elektronicznego, który umożliwia uwierzytelnionym Przejrzyj ich produktów. Anonimowy użytkownik odwiedzi stronę produktu, widział tylko informacje o produkcie i może nie mieć możliwość przesyłali recenzje. Jednak uwierzytelnionego użytkownika, odwiedzając stronę tego samego widział recenzowania interfejsu. Uwierzytelniony użytkownik nie ma jeszcze przejrzane tego produktu, interfejsu umożliwia im recenzje; w przeciwnym razie spowoduje to pokazanie ich ich wcześniej przesłanych przeglądu. Aby móc w tym scenariuszu krok dalej, stronę produktu może zawiera dodatkowe informacje i oferują rozszerzone funkcje dla tych użytkowników, którzy pracują dla firmy handlu elektronicznego. Na przykład stronę produktu może być lista spisu w magazynie i obejmują opcje, aby edytować opis po odwiedzeniu przez pracownika i cena produktu.

Takie zasady autoryzacji szczegółową można zaimplementować deklaratywne lub programowo (lub za pomocą kombinacji obu). W następnej sekcji Zobaczymy się, jak zaimplementować szczegółowej autoryzacji za pomocą kontrolki widoku logowania. Poniżej omówimy techniki programistyczne. Przed można przyjrzymy się stosowanie reguł autoryzacji szczegółową, jednak najpierw należy utworzyć stronę, na których funkcji zależy od użytkownika, odwiedzając go.

Utwórzmy strona, która wyświetla listę plików w katalogu określonym w ramach GridView. Wraz z listą nazwę każdego pliku, rozmiar i inne informacje, widoku GridView będzie zawierać dwie kolumny LinkButtons: jeden pod tytułem widoku i jeden zatytułowanym Delete. Jeśli kliknięto element LinkButton widoku zostanie wyświetlona zawartość wybranego pliku; Usuń element LinkButton po kliknięciu pliku zostaną usunięte. Początkowo Utwórzmy na tej stronie taki sposób, że jej widok funkcji i usuwania, jest dostępna dla wszystkich użytkowników. W używanie kontrolki widoku logowania i programowo Ograniczanie funkcjonalności sekcje należy sprawdzić, jak włączyć lub wyłączyć te funkcje, na podstawie użytkownika, odwiedzając stronę.

> [!NOTE]
> Strony ASP.NET, który będziemy tworzyć używa kontrolki widoku siatki, aby wyświetlić listę plików. Ponieważ ten samouczek, który seria skupia się na uwierzytelnianie formularzy, autoryzacji, konta użytkowników i ról nie chcę poświęcać zbyt dużo czasu, omawiając przebiega w kontrolce GridView. Chociaż ten samouczek zawiera określone instrukcje krok po kroku dotyczące konfigurowania tej strony, nie delve szczegóły Dlaczego wprowadzono niektórych opcji, lub w wyniku renderowania posiadane szczególne właściwości efektu. Dla głębszego zbadania kontrolki GridView, zapoznaj się z moich *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* serii samouczków.


Zacznij od otwarcia `UserBasedAuthorization.aspx` w pliku `Membership` folderu i dodaniu kontrolki widoku siatki do strony o nazwie `FilesGrid`. W widoku GridView tagu inteligentnego kliknij łącze Edytowanie kolumn, aby uruchomić okno dialogowe pól. W tym miejscu należy usunąć zaznaczenie pola wyboru pól automatycznego generowania, w lewym dolnym rogu. Następnie dodaj przycisk wyboru, przycisk Usuń i BoundFields dwa w lewym górnym rogu (przyciski Wybierz i usuwania można znaleźć w obszarze Typ CommandField). Wybierz przycisk Ustaw `SelectText` właściwości widoku i elementu pierwszy BoundField `HeaderText` i `DataField` właściwości nazwy. Ustaw drugiego elementu BoundField `HeaderText` właściwość rozmiar w bajtach, jego `DataField` właściwości długości, jego `DataFormatString` właściwości {0:N0} i jego `HtmlEncode` wartość False dla właściwości.

Po skonfigurowaniu kolumn GridView, kliknij przycisk OK, aby zamknąć okno dialogowe pól. W oknie właściwości ustaw GridView `DataKeyNames` właściwość `FullName`. W tym momencie oznaczeniu deklaracyjnym GridView powinien wyglądać następująco:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Za pomocą znaczników GridView utworzone firma Microsoft przystąpić do pisania kodu, który pobierze pliki w określonym katalogu i wiązania ich z kontrolki GridView. Dodaj następujący kod na stronę `Page_Load` program obsługi zdarzeń:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Powyższy kod używa [ `DirectoryInfo` klasy](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) Aby uzyskać listę plików w folderze głównym. [ `GetFiles()` Metoda](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) zwraca wszystkie pliki w katalogu jako tablica [ `FileInfo` obiektów](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), który następnie jest powiązany z kontrolki GridView. `FileInfo` Obiekt ma pewną liczbę właściwości, takie jak `Name`, `Length`, i `IsReadOnly`, między innymi. Jak widać w jego oznaczeniu deklaracyjnym widoku GridView po prostu wyświetla `Name` i `Length` właściwości.

> [!NOTE]
> `DirectoryInfo` i `FileInfo` klasy znajdują się w [ `System.IO` przestrzeni nazw](https://msdn.microsoft.com/library/system.io.aspx). W związku z tym, będzie muszą poprzedzony nazwy klas z ich nazwami przestrzeni nazw lub mają zaimportowane do pliku klasy przestrzeni nazw (za pośrednictwem `using System.IO`).


Poświęć chwilę, aby odwiedzić tę stronę za pośrednictwem przeglądarki. Wyświetli listę plików znajdujących się w katalogu głównym aplikacji. Wyświetl lub usuń LinkButtons kliknięcie spowoduje odświeżenie strony, ale żadna akcja będzie być fakt, że zostały wykonane następujące kroki jeszcze do tworzenie obsługi zdarzeń wymagane.


[![Kontrolki GridView Wyświetla listę plików w katalogu głównym aplikacji sieci Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Rysunek 7**: widoku GridView Wyświetla listę plików w katalogu głównym aplikacji sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image21.png))


Potrzebujemy sposób, aby wyświetlić zawartość wybranego pliku. Wróć do programu Visual Studio i Dodaj pole tekstowe o nazwie `FileContents` powyżej widoku GridView. Ustaw jego `TextMode` właściwości `MultiLine` i jego `Columns` i `Rows` właściwości do 95% i 10, odpowiednio.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Następnie należy utworzyć procedurę obsługi zdarzeń dla GridView [ `SelectedIndexChanged` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) i Dodaj następujący kod:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Ten kod korzysta z GridView `SelectedValue` właściwości, aby określić nazwę cały plik wybranego pliku. Wewnętrznie `DataKeys` odwołuje się do kolekcji w celu uzyskania `SelectedValue`, więc konieczne jest ustawienie GridView `DataKeyNames` właściwość na nazwę, jak opisano wcześniej w tym kroku. [ `File` Klasy](https://msdn.microsoft.com/library/system.io.file.aspx) służy do odczytywania zawartość wybranego pliku w ciąg, który jest przypisywany do `FileContents` pola `Text` właściwości, w ten sposób wyświetlania zawartości wybranego pliku na stronie.


[![Zawartość pliku wybrana są wyświetlane w polu tekstowym](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Rysunek 8**: wybrany plik zawartość jest wyświetlana w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Jeśli wyświetlanie zawartości pliku, który zawiera kod znaczników HTML, a następnie ponów próbę przeglądanie lub usuwanie pliku, zostanie wyświetlony `HttpRequestValidationException` błędu. Dzieje się tak, ponieważ na zwrot zawartość pole tekstowe są wysyłane z powrotem do serwera sieci web. Domyślnie program ASP.NET zgłasza `HttpRequestValidationException` błąd zawsze wtedy, gdy potencjalnie niebezpieczną treść zwrotu, takie jak kod znaczników HTML, zostanie wykryta. Aby wyłączyć ten błąd występuje, wyłącz weryfikację żądań dla strony, dodając `ValidateRequest="false"` do `@Page` dyrektywy. Aby uzyskać więcej informacji o zaletach weryfikacji żądania jako oraz jakie środki ostrożności należy wykonać, gdy wyłączenie go, przeczytaj [żądanie weryfikacji — zapobieganie atakom skryptów](https://asp.net/learn/whitepapers/request-validation/).


Na koniec należy dodać program obsługi zdarzeń, używając następującego kodu dla GridView [ `RowDeleting` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Kod po prostu Wyświetla pełną nazwę pliku do usunięcia w `FileContents` TextBox *bez* faktycznego usuwania pliku.


[![Kliknięcie przycisku Usuń nie rzeczywistego usunięcia pliku](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Rysunek 9**: klikając polecenie Usuń przycisk rzeczywistości nie usuwa plik ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image27.png))


W kroku 1 skonfigurowaliśmy reguł autoryzacji adresów URL, aby uniemożliwić użytkownikom anonimowym wyświetlania stron w `Membership` folderu. Aby lepiej następującej liczby etapów stwierdzono szczegółową uwierzytelniania, użytkowników anonimowych odwiedzić zezwolimy na `UserBasedAuthorization.aspx` strony, ale z ograniczoną funkcjonalnością. Aby otworzyć tę stronę można uzyskać dostęp przez wszystkich użytkowników, Dodaj następujący kod `<location>` elementu `Web.config` w pliku `Membership` folderu:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Po dodaniu `<location>` elementu, testowanie nowych reguł autoryzacji adresów URL, logując się poza lokacji. Jako użytkownik anonimowy użytkownik powinny zostać dopuszczone do odwiedzenia `UserBasedAuthorization.aspx` strony.

Obecnie żadnych uwierzytelnieni lub anonimowi użytkownik może odwiedzić `UserBasedAuthorization.aspx` strony i wyświetlania lub usuwania plików. Załóżmy, że tak, aby tylko uwierzytelnieni użytkownicy mogą wyświetlać zawartość pliku i tylko Tito można usunąć pliku. Deklaratywne, programowo lub za pomocą kombinacji obu tych metod można zastosować takich szczegółową reguł autoryzacji. Użyjmy podejścia deklaratywnego, aby ograniczyć, kto może wyświetlać zawartość pliku; użyjemy programowe podejście do limit, który można usunąć pliku.

### <a name="using-the-loginview-control"></a>Za pomocą kontrolki widoku logowania

Jak widzieliśmy w poprzednich samouczkach kontrolki widoku logowania jest przydatny do wyświetlania różne interfejsy dla użytkowników uwierzytelnionych i anonimowych i zapewnia prosty sposób, aby ukryć funkcjonalność, która nie jest dostępna dla użytkowników anonimowych. Ponieważ użytkownicy anonimowi nie mogą wyświetlić lub usunąć pliki, musimy pokazanie `FileContents` pole tekstowe, gdy strona jest kontrolowane przez uwierzytelnionego użytkownika. Aby to osiągnąć, należy dodać kontrolki widoku logowania do strony, nadaj jej nazwę `LoginViewForFileContentsTextBox`i Przenieś `FileContents` pole tekstowe w oznaczeniu deklaracyjnym do kontrolki widoku logowania `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Kontrolki sieci Web w szablonach widoku logowania nie są już dostępne bezpośrednio z kodem klasę. Na przykład `FilesGrid` GridView `SelectedIndexChanged` i `RowDeleting` procedury obsługi zdarzeń obecnie odwoływać się do `FileContents` Formant TextBox z kodu, takich jak:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Jednak ten kod nie jest już prawidłowy. Przenosząc `FileContents` pole tekstowe do `LoggedInTemplate` pole tekstowe nie są bezpośrednio dostępne. Zamiast tego trzeba użyć `FindControl("controlId")` metodę, aby programowo odwoływać się do kontrolki. Aktualizacja `FilesGrid` procedury obsługi zdarzeń, aby odwoływać się do pola tekstowego w następujący sposób:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Po przeniesieniu pole tekstowe do widoku logowania `LoggedInTemplate` i aktualizowania kodu strony z odwołaniem do pola tekstowego przy użyciu `FindControl("controlId")` wzorca, odwiedź stronę jako użytkownik anonimowy. Jak pokazano na rysunku nr 10, `FileContents` nie jest wyświetlane pole tekstowe. Element LinkButton widok nadal jest wyświetlany.


[![Kontrolki widoku logowania renderuje tylko pole tekstowe FileContents dla uwierzytelnionych użytkowników](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Na rysunku nr 10**: widoku logowania kontrolka renderuje tylko `FileContents` pole tekstowe dla użytkowników uwierzytelnionych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image30.png))


Jednym ze sposobów, aby ukryć przycisk Widok dla użytkowników anonimowych to aby przekonwertować pola GridView TemplateField. Spowoduje to wygenerowanie szablonu, który zawiera oznaczeniu deklaracyjnym LinkButton widoku. Firma Microsoft może dodać kontrolki widoku logowania TemplateField i umieścić element LinkButton w widoku logowania `LoggedInTemplate`, a tym samym ukrywanie przycisku Widok od odwiedzających anonimowe. Aby to zrobić, kliknij link Edytuj kolumny z GridView tagu inteligentnego, aby uruchomić okno dialogowe pól. Następnie kliknij przycisk Wybierz z listy w lewym dolnym rogu, a następnie kliknij przycisk Convert to pole do łącza TemplateField. Ten sposób zmodyfikuje oznaczeniu deklaracyjnym pole od:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Do: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

W tym momencie możemy dodać widoku logowania do TemplateField. Następujący kod przedstawia element LinkButton widok tylko do uwierzytelnionych użytkowników.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Na ilustracji 11 pokazano, w rezultacie nie jest że dość jako widok kolumny jest nadal wyświetlany, mimo że LinkButtons widok, w kolumnie są ukryte. Omówimy sposób ukrywania całą kolumnę GridView (i nie tylko element LinkButton) w następnej sekcji.


[![Kontrolki widoku logowania ukrywa LinkButtons widoku dla użytkowników anonimowych](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Rysunek 11**: kontrolki widoku logowania ukrywa LinkButtons widoku dla użytkowników anonimowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programowe ograniczanie funkcji

W niektórych sytuacjach deklaratywne technik są niewystarczające do ograniczania funkcjonalności do strony. Na przykład dostępność niektórych funkcji strony może być zależny od kryteriów powyżej, czy użytkownik, odwiedzając stronę jest anonimowa lub uwierzytelniony. W takich przypadkach różne elementy interfejsu użytkownika, można wyświetlić lub ukryte w sposób programowy.

Aby programowo ograniczenie funkcjonalności, należy wykonać dwa zadania:

1. Określić, czy użytkownik, odwiedzając stronę mogą uzyskiwać dostęp do funkcji, a
2. Programowego modyfikowania interfejsu użytkownika, w oparciu o tego, czy użytkownik ma dostęp do funkcji w danym.

Aby zademonstrować aplikację te dwa zadania, zezwolimy tylko na Tito usunąć pliki z kontrolki GridView. Nasze pierwsze zadanie następnie ma na celu określenie, czy jest ono Tito, odwiedzając stronę. Po określeniu, musimy ukryć (lub pokazuj) prvku GridView Usuń kolumnę. Kolumny w widoku GridView są dostępne za pośrednictwem jego `Columns` właściwości kolumny jest renderowana tylko, jeśli jego `Visible` właściwość jest ustawiona na `true` (ustawienie domyślne).

Dodaj następujący kod do `Page_Load` program obsługi zdarzeń przed wiązanie danych do kontrolki GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Tak jak Omówiliśmy to w [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczku `User.Identity.Name` zwraca nazwę tożsamości. Odpowiada to nazwa użytkownika wprowadzona w formancie logowania. Jeśli jest Tito, odwiedzając strony, druga kolumna w widoku GridView `Visible` właściwość jest ustawiona na `true`; w przeciwnym razie jest równa `false`. Wynikiem jest, że gdy ktoś inny niż Tito odwiedzin strony innego użytkownika uwierzytelnionego lub użytkownik anonimowy kolumny usuwania nie są odtwarzane (patrz rysunek 12); Jednak gdy Tito odwiedzających stronę, Usuń kolumnę jest obecny (zobacz rysunek 13).


[![Usuń kolumnę jest nie renderowane podczas odwiedzone przez kogoś innego niż Tito (na przykład Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Rysunek 12**: Usuń kolumnę jest nie renderowane podczas odwiedzone przez kogoś innego niż Tito (na przykład Bruce) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image36.png))


[![Usuń kolumnę nie jest renderowany Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Rysunek 13**: Usuń kolumnę nie jest renderowany Tito ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Krok 4: Stosowanie reguł autoryzacji do metod i klas

W kroku 3 możemy niedozwolone, użytkowników anonimowych, wyświetlanie zawartość pliku i będzie to zabronione wszystkich użytkowników, z wyjątkiem Tito usuwania plików. To było wykonywane przez ukrycie elementów interfejsu użytkownika skojarzonego dla nieautoryzowanych użytkowników zewnętrznych za pomocą technik deklaracyjne i programowe. W naszym przykładzie prostego prawidłowo ukrywanie elementów interfejsu użytkownika zostało utrudnione, ale co bardziej złożonych witryn w przypadku, gdy może istnieć wiele różnych sposobów, aby wykonać te same funkcje? Ograniczanie funkcjonalność dla nieautoryzowanych użytkowników, co się stanie, firma Microsoft nie pamięta ukryć lub Wyłącz wszystkie elementy interfejsu użytkownika dotyczy?

Prosty sposób, aby upewnić się, że do określonego elementu funkcje nie są dostępne przez nieautoryzowanego użytkownika jest do dekorowania tej klasy lub metody za pomocą [ `PrincipalPermission` atrybutu](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowisko uruchomieniowe platformy .NET używa klasy lub wykonuje jeden z jego metod, sprawdza, sprawdź, czy bieżący kontekst zabezpieczeń ma uprawnienia do używania klasy lub wykonać metodę. `PrincipalPermission` Atrybutu zapewnia mechanizm, za pomocą którego możemy zdefiniować te reguły.

Zobaczmy, jak działają przy użyciu `PrincipalPermission` atrybutu GridView `SelectedIndexChanged` i `RowDeleting` programów obsługi zdarzeń do Stanów Zjednoczonych zabraniają wykonywania przez użytkowników anonimowych, jak i użytkowników innych niż Tito, odpowiednio. To wszystko, co należy zrobić, Dodaj odpowiedni atrybut na jego podstawie Każda definicja funkcji:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Atrybut `SelectedIndexChanged` określają procedury obsługi zdarzeń, które tylko uwierzytelnionym użytkownikom można wykonać obsługi zdarzeń, gdzie jako atrybut na `RowDeleting` program obsługi zdarzeń ogranicza wykonanie Tito.

Jeśli jakiś sposób, próbuje wykonać użytkownik inny niż Tito `RowDeleting` programu obsługi zdarzeń lub bez uwierzytelnienia użytkownik podejmuje próbę wykonania `SelectedIndexChanged` programu obsługi zdarzeń środowiska uruchomieniowego .NET zgłosi `SecurityException`.


[![Jeśli kontekst zabezpieczeń nie ma autoryzacji do wykonania metody, jest zgłaszany securityexception —](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Rysunek 14**: Jeśli kontekst zabezpieczeń nie ma autoryzacji do wykonania metody `SecurityException` zgłaszany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Aby zezwolić na wiele konteksty zabezpieczeń dostępu do klasy lub metody, dekoracji klasy lub metody za pomocą `PrincipalPermission` atrybutu dla każdego kontekstu zabezpieczeń. Oznacza to umożliwiające Tito i Bruce do wykonania `RowDeleting` procedura obsługi zdarzeń, Dodaj *dwóch* `PrincipalPermission` atrybuty:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Oprócz stron ASP.NET wiele aplikacji ma architekturę, która obejmuje różne warstwy, takie jak logiki biznesowej i warstwy dostępu do danych. Te warstwy są zwykle implementowane jako bibliotek klas i oferują klasy i metody służące do wykonywania funkcji powiązanych logiki i danych biznesowych. `PrincipalPermission` Atrybut jest przydatne w przypadku stosowania reguł autoryzacji do tych warstw.

Aby uzyskać więcej informacji na temat korzystania z `PrincipalPermission` atrybutu, aby zdefiniować reguły autoryzacji dla klasy i metody, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [Dodawanie reguły autoryzacji w celu biznesowych i danych zapomocąwarstwy`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku zobaczyliśmy, jak zastosować reguły autoryzacji na podstawie użytkownika. Zaczęliśmy od przyjrzeć się ASP. Struktura autoryzacji adresu URL w sieci. Dla każdego żądania, aparatu ASP.NET firmy `UrlAuthorizationModule` sprawdza reguł autoryzacji adresów URL, które są zdefiniowane w konfiguracji aplikacji w celu ustalenia, czy tożsamość jest autoryzowany do uzyskania dostępu do żądanego zasobu. Krótko mówiąc Autoryzacja adresów URL można łatwo określić reguły autoryzacji dla określonej strony lub dla wszystkich stron w określonym katalogu.

Struktura adresu URL autoryzacji stosuje reguły autoryzacji na podstawie strony strona. Za pomocą Autoryzacja adresów URL żądania tożsamości jest autoryzowany dostęp do określonego zasobu lub nie. Wiele scenariuszy, jednak wymagają więcej reguł autoryzacji szczegółową. Zamiast definiować, kto może uzyskać dostęp do strony, firma Microsoft może być konieczne umożliwi wszystkim dostępu do strony, ale wyświetlanie różnych danych lub oferują różne funkcje, w zależności od użytkownika, odwiedzając stronę. Uwierzytelnianie na poziomie strony zazwyczaj polega na ukrywanie elementów interfejsu użytkownika, aby uniemożliwić dostęp do funkcji zabronionych przez nieautoryzowanych użytkowników. Ponadto istnieje możliwość ograniczania dostępu do klas i wykonania jego metod dla niektórych użytkowników za pomocą atrybutów.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dodawanie reguły autoryzacji w celu biznesowej i warstwy danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autoryzacja w programie ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Zmian między wersjami usług IIS 6 i zabezpieczeń usług IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurowanie określonych plików i podkatalogów](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Ograniczanie funkcji modyfikacji danych na podstawie użytkownika](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Przewodniki Szybki Start kontrolki widoku logowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Informacje o autoryzacji adresów URL usług IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Dokumentacja techniczna](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](validating-user-credentials-against-the-membership-user-store-cs.md)
> [dalej](storing-additional-user-information-cs.md)
