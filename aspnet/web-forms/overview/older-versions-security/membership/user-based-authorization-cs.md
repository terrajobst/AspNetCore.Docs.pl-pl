---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: "Autoryzacji opartej na użytkownika (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku przedstawiono, ograniczanie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: da03a9c3e22f5a2164534ef7896b5558beb8b6f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="user-based-authorization-c"></a>Autoryzacji opartej na użytkownika (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> W tym samouczku przedstawiono, ograniczanie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik.


## <a name="introduction"></a>Wprowadzenie

Większość aplikacji sieci web, które oferują kont użytkowników to zrobić w części w celu ograniczenia pewnych osób odwiedzających dostęp do określonych stron w witrynie. W lokacjach messageboard najbardziej online na przykład wszystkich użytkowników — anonimowych, jak i uwierzytelnionych — będą mogli wyświetlić wpisów messageboard jednak tylko uwierzytelnieni użytkownicy mogą odwiedzać strony sieci web, aby utworzyć nowy wpis. I może być stron administracyjnych dostępnych tylko dla określonego użytkownika (lub zestaw użytkowników). Ponadto funkcji na poziomie strony może się różnić dla poszczególnych użytkowników przez użytkownika. Podczas wyświetlania listy wpisów, uwierzytelnionym użytkownikom przedstawiono interfejs dla klasyfikacji każdego post ten interfejs nie jest dostępny dla osób odwiedzających anonimowe.

ASP.NET można łatwo zdefiniować reguły autoryzacji na podstawie użytkownika. Po prostu bit z kodu znaczników w `Web.config`, określonych stron sieci web lub całych katalogów można zablokować tak, aby tylko są one dostępne do określonej podgrupy użytkowników. Funkcje na poziomie strony można włączyć lub wyłączyć zależności od aktualnie zalogowanego użytkownika w sposób programowy i deklaratywne.

W tym samouczku przedstawiono, ograniczanie dostępu do stron i ograniczenie funkcji na poziomie strony za pomocą różnych technik. Dzieła!

## <a name="a-look-at-the-url-authorization-workflow"></a>Obejrzyj przepływu pracy autoryzacji adresu URL

Zgodnie z opisem w [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, gdy moduł wykonawczy platformy ASP.NET przetwarza żądanie dla zasobu żądania ASP.NET zgłasza liczbę zdarzeń podczas jego cyklu życia. *Moduły HTTP* są klasami zarządzanymi, którego kod jest wykonywany w odpowiedzi na wystąpienie określonego zdarzenia w cyklu życia żądania. Program ASP.NET jest dostarczany z modułów HTTP wykonywania podstawowych zadań w tle.

Jeden taki moduł HTTP jest [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx). Zgodnie z opisem w poprzedniej samouczki, podstawową funkcją `FormsAuthenticationModule` ma na celu określenie tożsamości bieżącego żądania. Jest to osiągane przez sprawdzenie biletu uwierzytelniania formularzy, które znajduje się w pliku cookie lub osadzone w adresie URL. Ten identyfikator ma miejsce podczas [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Inny moduł HTTP ważne jest [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx), który jest uruchamiany w odpowiedzi na [ `AuthorizeRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx) (która stanie po `AuthenticateRequest` zdarzeń). `UrlAuthorizationModule` Sprawdza, czy konfiguracja znaczników w `Web.config` ustalenie, czy bieżąca tożsamość ma uprawnienia do określonej strony. Ten proces jest nazywany *Autoryzacja adresów URL*.

Zajmiemy się składni reguł autoryzacji adresów URL w kroku 1, ale najpierw umożliwia przyjrzeć się co `UrlAuthorizationModule` jest w zależności od tego, czy żądanie jest autoryzowane. Jeśli `UrlAuthorizationModule` Określa, czy żądanie jest autoryzowane, a następnie go nie działa i żądania jest kontynuowany przez cały cykl jej życia. Jednak jeśli żądanie jest *nie* autoryzowany, a następnie `UrlAuthorizationModule` przerywa cykl życia i powoduje, że `Response` obiektu do zwrócenia [HTTP 401 nieautoryzowane](http://www.checkupdown.com/status/E401.html) stanu. Korzystając z uwierzytelniania formularzy ten stan HTTP 401 nigdy nie jest zwracana do klienta, ponieważ jeśli `FormsAuthenticationModule` wykrywa HTTP 401, jest w stanie modyfikuje go do [HTTP 302 przekierowania](http://www.checkupdown.com/status/E302.html) do strony logowania.

Rysunek 1 pokazuje przepływu pracy potoku ASP.NET `FormsAuthenticationModule`i `UrlAuthorizationModule` po odebraniu nieautoryzowanego żądania. W szczególności rysunek 1 pokazuje żądania przez anonimowy obiekt odwiedzający dla `ProtectedPage.aspx`, która jest strona, która nie zezwala na dostęp do użytkowników anonimowych. Ponieważ użytkownik jest anonimowy, `UrlAuthorizationModule` anuluje żądanie i zwraca stanu HTTP 401 nieautoryzowane. `FormsAuthenticationModule` Następnie konwertuje stanu 401 na przekierowanie 302 strony logowania. Po uwierzytelnieniu użytkownika za pomocą strony logowania, jest on przekierowywane do `ProtectedPage.aspx`. Teraz `FormsAuthenticationModule` identyfikuje użytkownika, w oparciu o jego biletu uwierzytelniania. Teraz, gdy użytkownik jest uwierzytelniony, `UrlAuthorizationModule` zezwala na dostęp do tej strony.


[![Uwierzytelnianie formularzy i przepływu pracy autoryzacji adresu URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Rysunek 1**: uwierzytelnianie formularzy i przepływu pracy autoryzacji adresu URL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image3.png))


Rysunek 1 przedstawia interakcji, gdy anonimowy użytkownik próbuje uzyskać dostęp do zasobu, który nie jest dostępne dla użytkowników anonimowych. W takim przypadku anonimowy użytkownik zostanie przekierowany do strony logowania ze stroną, którą użytkownik próbował można znaleźć określonej w zmiennej querystring. Po pomyślnym zalogowaniu się użytkownika użytkownik zostanie automatycznie przekierowany ponownie do zasób, do którego użytkownik początkowo próby wyświetlenia.

Gdy nieautoryzowanego żądania nawiązuje użytkownik anonimowy, ten przepływ pracy jest proste i łatwo dla obiektu odwiedzającego zrozumieć, jakie miało miejsce i dlaczego. Jednak należy pamiętać, że `FormsAuthenticationModule` przekieruje *żadnych* nieautoryzowanego użytkownika do strony logowania, nawet jeśli żądanie zostało utworzone przez użytkownika uwierzytelnionego. Może to spowodować mylące środowisko użytkownika uwierzytelnionego użytkownika próba odwiedź stronę, dla której nie ma ona urzędu.

Załóżmy, że naszą witrynę sieci Web były jego reguł autoryzacji adresów URL skonfigurowany tak, aby strona ASP.NET `OnlyTito.aspx` accessibly był tylko do Tito. Teraz, załóżmy, że Sam odwiedza witrynę, logowania i podejmuje próbę odwiedź `OnlyTito.aspx`. `UrlAuthorizationModule` Spowoduje zatrzymanie cyklu życia żądania i zwraca stan HTTP 401 nieautoryzowane, który `FormsAuthenticationModule` wykryje i Sam przekierowanie do strony logowania. Ponieważ Sam jest już zalogowany, jednak użytkownik może zastanawiasz się, dlaczego została ona wysłana wróć do strony logowania. Może ona przyczyny, że swoje poświadczenia logowania zostały utracone jakieś operacji lub że ona wprowadzono nieprawidłowe poświadczenia. Jeśli Sam reenters swoje poświadczenia ze strony logowania zostanie ona zalogowany (ponownie) i Przekierowanie do `OnlyTito.aspx`. `UrlAuthorizationModule` Wykryje, że Sam nie można znaleźć tej strony i klika nastąpi powrót do strony logowania.

Rysunek 2 przedstawia to mylące przepływu pracy.


[![Domyślny przepływ pracy może prowadzić do mylące cyklu](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Rysunek 2**: domyślny przepływ pracy może prowadzić do cyklu skomplikowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image6.png))


Przepływ pracy przedstawiono na rysunku 2 można szybko befuddle nawet większość komputerów pomysłowy obiekt odwiedzający. Przedstawiono sposób, aby zapobiec takiej sytuacji skomplikowana cyklu w kroku 2.

> [!NOTE]
> Program ASP.NET używa dwa mechanizmy, aby określić, czy bieżący użytkownik może uzyskiwać dostęp do określonej strony sieci web: Autoryzacja adresów URL i pliku autoryzacji. Autoryzacja pliku jest implementowany przez [ `FileAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.fileauthorizationmodule.aspx), która określa urząd przeglądając pliki żądanej listy ACL. Autoryzacja pliku jest najczęściej używane z uwierzytelnianiem systemu Windows, ponieważ listy ACL są uprawnienia, które są stosowane do kont systemu Windows. Podczas korzystania z uwierzytelniania formularzy, wszystkie żądania systemowe systemu operacyjnego i plików są wykonywane przez to samo konto systemu Windows, niezależnie od użytkownika, w tej witrynie. Ponieważ ta seria samouczek koncentruje się na uwierzytelnianie formularzy, firma Microsoft będzie nie w niniejszym dokumencie pliku autoryzacji.


### <a name="the-scope-of-url-authorization"></a>Zakres autoryzacji adresów URL

`UrlAuthorizationModule` Jest zarządzanego kodu, który jest częścią środowiska uruchomieniowego ASP.NET. Przed firmy Microsoft w wersji 7 [Internet Information Services (IIS)](https://www.iis.net/) serwera sieci web wystąpił różne bariery między potoku HTTP przez usługi IIS i środowiskiem uruchomieniowym ASP.NET, potok. W krótkim, w usługach IIS 6 i starszych ASP. W sieci `UrlAuthorizationModule` wykonywana tylko wtedy, gdy żądanie jest delegowane za pomocą programu IIS do środowiska wykonawczego programu ASP.NET. Domyślnie usługi IIS przetwarza zawartości statycznej, sama — podobnie jak strony HTML i CSS, JavaScript i plików obrazów — i tylko przekazuje żądania do środowiska wykonawczego platformy ASP.NET, gdy strona z rozszerzeniem `.aspx`, `.asmx`, lub `.ashx` jest wymagane.

IIS 7, jednak umożliwia zintegrowanych usług IIS i platformy ASP.NET potoków. Przy użyciu kilku ustawień konfiguracji należy skonfigurować usług IIS 7 do wywołania `UrlAuthorizationModule` dla *wszystkie* żądań, co oznacza, że reguł autoryzacji adresów URL mogą być definiowane dla plików dowolnego typu. Ponadto w IIS 7 obejmuje silnik adresu URL autoryzacji. Aby uzyskać więcej informacji na temat integracji ASP.NET i IIS 7 natywnego adresu URL autoryzacji funkcji, zobacz [Autoryzacja adresów URL usług IIS7 opis](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Aby uzyskać więcej informacji na temat przyjrzeć integracji ASP.NET i IIS 7, wybierz kopię książki Shahram Khosravi *Professional usług IIS 7 i ASP.NET zintegrowanego programowania* (ISBN: 978 0470152539).

Mówiąc w wersjach wcześniejszych niż IIS 7, reguł autoryzacji adresów URL są stosowane tylko do zasobów obsługiwane przez moduł wykonawczy platformy ASP.NET. Jednak z usług IIS 7 jest możliwe, aby korzystać z funkcji autoryzacji adresu URL macierzystego przez usługi IIS lub zintegrować ASP. W sieci `UrlAuthorizationModule` do potoku HTTP przez usługi IIS, rozszerzając w ten sposób tę funkcję do wszystkich żądań.

> [!NOTE]
> Istnieją pewne różnice w sposób niewielkie jeszcze ważne ASP. W sieci `UrlAuthorizationModule` i funkcja autoryzacji adresów URL usług IIS 7 Przetwarzaj reguł autoryzacji. W tym samouczku nie bada funkcjonalności autoryzacji adresów URL IIS 7 lub różnice w sposób analizuje reguły autoryzacji w porównaniu do `UrlAuthorizationModule`. Aby uzyskać więcej informacji o tych tematów, zapoznaj się z dokumentacją usług IIS 7, MSDN lub na [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Krok 1: Definiowanie reguł autoryzacji adresów URL w`Web.config`

`UrlAuthorizationModule` Określa, czy należy udzielić lub odmówić dostępu do żądanego zasobu dla określonej tożsamości na podstawie reguł autoryzacji adresów URL zdefiniowanych w konfiguracji aplikacji. Reguły autoryzacji są zapisane w [ `<authorization>` elementu](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) w formie `<allow>` i `<deny>` elementy podrzędne. Każdy `<allow>` i `<deny>` można określić elementu podrzędnego:

- Określonego użytkownika
- Rozdzielana przecinkami lista użytkowników
- Wszyscy użytkownicy anonimowi, oznaczone znakiem zapytania (?)
- Wszyscy użytkownicy, oznaczone gwiazdką (\*)

Następujący kod ilustruje sposób używania reguł autoryzacji adresów URL do zezwalania użytkownikom Tito i Scott i wszystkie inne odmowy:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` Element definiuje, jakie użytkownicy mogą uzyskiwać - Tito i Scott - podczas `<deny>` nakazuje element, który *wszystkie* użytkownicy są odrzucane.

> [!NOTE]
> `<allow>` i `<deny>` elementy można również określić reguły autoryzacji ról. Omówione autoryzacji opartej na rolach w przyszłości samouczka.


Poniższe ustawienie udziela dostępu nikt inny oprócz Sam (w tym anonimowego odwiedzających):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Aby zezwolić tylko uwierzytelnionych użytkowników, należy użyć następującej konfiguracji, który nie zezwala na dostęp do wszystkich użytkowników anonimowych:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Reguły autoryzacji są zdefiniowane w ramach `<system.web>` element `Web.config` i zastosować do wszystkich zasobów aplikacji sieci web programu ASP.NET. Aplikacja ma często, reguły autoryzacji różne dla różnych sekcji. Na przykład w witrynie handlu elektronicznego wszystkich odwiedzających może przejrzeć produktów, zobacz recenzje produktu wyszukać w wykazie i itd. Jednak tylko uwierzytelnionych użytkowników może osiągnąć wyewidencjonowywanie lub stron do zarządzania historią wysyłki osoby. Ponadto może być części lokacji używanych tylko wybrani użytkownicy, takich jak Administratorzy.

ASP.NET ułatwia określenie reguł autoryzacji różne dla różnych plików i folderów w witrynie. Reguły autoryzacji, określone w folderze głównym `Web.config` plik Zastosuj do wszystkich zasobów programu ASP.NET w witrynie. Jednak można zastąpić te ustawienia domyślne autoryzacji dla danego folderu, dodając `Web.config` z `<authorization>` sekcji.

Umożliwia zaktualizowanie naszą witrynę sieci Web, tak aby tylko uwierzytelnieni użytkownicy mogą odwiedzać strony ASP.NET `Membership` folderu. Należy dodać w tym celu `Web.config` pliku `Membership` folderu i jego ustawienia autoryzacji, aby odmówić użytkowników anonimowych. Kliknij prawym przyciskiem myszy `Membership` folder w Eksploratorze rozwiązań z menu kontekstowego wybierz polecenie Dodaj nowy element menu, a następnie dodaj nowy plik konfiguracji sieci Web o nazwie `Web.config`.


[![Dodaj plik Web.config do folderu członkostwa](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Rysunek 3**: Dodaj `Web.config` pliku `Membership` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image9.png))


W tym momencie projektu powinien zawierać dwa `Web.config` pliki: jeden w katalogu głównym, a drugi w `Membership` folderu.


[![Twoja aplikacja powinna zawierać teraz dwóch plikach Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Rysunek 4**: Twojej aplikacji powinna teraz zawiera dwa `Web.config` plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image12.png))


Aktualizuj plik konfiguracji w `Membership` folder, którego nie uniemożliwia dostęp do użytkowników anonimowych.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

To wszystko jest do niego!

Aby przetestować tę zmianę, odwiedź stronę główną w przeglądarce i upewnij się, że zalogowano. Ponieważ domyślne zachowanie aplikacji ASP.NET jest umożliwienie wszystkich odwiedzających i nie udzielamy jakichkolwiek modyfikacji autoryzacji do katalogu głównego `Web.config` pliku, możemy odwiedź pliki w katalogu głównym jako użytkownik anonimowy.

Kliknij łącze tworzenia kont użytkowników w lewej kolumnie. Spowoduje to przejście do `~/Membership/CreatingUserAccounts.aspx`. Ponieważ `Web.config` w pliku `Membership` folderu definiuje reguły autoryzacji, aby zabronić dostęp anonimowy `UrlAuthorizationModule` anuluje żądanie i zwraca stanu HTTP 401 nieautoryzowane. `FormsAuthenticationModule` Modyfikuje to stanu przekierowania 302, Wyślij do strony logowania. Należy pamiętać, że strony możemy zostały próby uzyskania dostępu do (`CreatingUserAccounts.aspx`) jest przekazywana do strony logowania za pośrednictwem `ReturnUrl` parametr querystring.


[![Od adresu URL autoryzacji reguły zabronić dostępu anonimowego możemy przekierowanie do strony logowania](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Rysunek 5**: ponieważ Autoryzacja adresów URL reguły Zabroń dostępu anonimowego, możemy przekierowanie do strony logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image15.png))


Po pomyślnym zalogowaniu możemy są przekierowywane do `CreatingUserAccounts.aspx` strony. Teraz `UrlAuthorizationModule` zezwala na dostęp do tej strony, ponieważ firma Microsoft nie są już anonimowy.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Stosowanie reguł autoryzacji adresów URL do określonej lokalizacji

Ustawienia autoryzacji zdefiniowane w `<system.web>` sekcji `Web.config` stosowane do wszystkich zasobów platformy ASP.NET w tym katalogu i jego podkatalogach (do czasu, w przeciwnym razie przesłonięta przez inny `Web.config` pliku). W niektórych przypadkach jednak może chcemy wszystkie zasoby platformy ASP.NET w podanym katalogu konfiguracji określonego autoryzacji, z wyjątkiem jednego lub dwóch określonych stron. Można to osiągnąć przez dodanie `<location>` element `Web.config`wskazaniu pliku, w której reguły autoryzacji różnią się i definiowanie jej reguł autoryzacji unikatowy nim.

Aby zilustrować przy użyciu `<location>` elementu, aby zastąpić ustawienia konfiguracji dla określonego zasobu, możemy dostosować ustawienia autoryzacji, aby odwiedzić tylko Tito `CreatingUserAccounts.aspx`. W tym celu należy dodać `<location>` elementu `Membership` folderu `Web.config` plików i zaktualizuj jego znaczników, aby wyglądały one podobnie do następującej:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>` Element `<system.web>` definiuje reguł autoryzacji adresów URL domyślnego programu ASP.NET w `Membership` folderze i jego podfolderach. `<location>` Element pozwala zastąpić te reguły dla określonego zasobu. W znaczniku powyżej `<location>` odwołania do elementu `CreatingUserAccounts.aspx` strony i określa zezwolenie zasady pozwalać Tito, ale Odmów inne osoby.

Aby przetestować tę zmianę autoryzacji, uruchom odwiedzając witrynę sieci Web jako użytkownik anonimowy. Jeśli spróbujesz strony w `Membership` folderu, takich jak `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` odrzuciła żądanie, zostanie i nastąpi przekierowanie do strony logowania. Po zalogowaniu się jako przykład Scott mogą odwiedzać dowolnej strony w `Membership` folderu *z wyjątkiem* dla `CreatingUserAccounts.aspx`. Próba odwiedź `CreatingUserAccounts.aspx` zalogowany jako każdy użytkownik, ale Tito spowoduje próbę nieautoryzowanego dostępu, przekierowywanie można z powrotem do strony logowania.

> [!NOTE]
> `<location>` Elementu musi występować poza konfiguracji `<system.web>` elementu. Należy użyć oddzielnego `<location>` elementu dla każdego zasobu, którego chcesz zmienić ustawienia autoryzacji.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Jak przeglądać`UrlAuthorizationModule`do udzielania lub odmawiania dostępu przy użyciu reguł autoryzacji

`UrlAuthorizationModule` Określa, czy do autoryzacji określonej tożsamości dla określonego adresu URL, analizując Autoryzacja adresów URL reguły pojedynczo, zaczynając od pierwszego z nich i Praca drodze w dół. Jak dopasowania zostanie znaleziony, użytkownik jest udzielono lub odmówiono dostępu, w zależności od if dopasowania został znaleziony w `<allow>` lub `<deny>` elementu. **Jeśli nie znaleziono, użytkownik otrzymuje dostęp.** W związku z tym, jeśli chcesz ograniczyć dostęp, jest użycie `<deny>` element jako ostatni element w konfiguracji autoryzacji adresu URL. **W przypadku pominięcia ***`<deny>`*** elementu, wszyscy użytkownicy będą mieć dostęp.**

Aby lepiej zrozumieć proces wykorzystywany przez `UrlAuthorizationModule` ustalenie urzędu Rozważmy przykład reguł autoryzacji adresów URL analizujemy wcześniej w tym kroku. Pierwsza reguła jest `<allow>` element, który umożliwia dostęp do Tito i Scott. Drugi reguły jest `<deny>` element, który nie zezwala na dostęp wszystkim użytkownikom. Jeśli użytkownik anonimowy odwiedza, `UrlAuthorizationModule` uruchamia pytając, jest anonimowy Scott lub Tito? Odpowiedź na pytanie, jest oczywiście nie, więc będzie kontynuowana, do drugiego reguły. Jest anonimowy w zestawie każdy? Od czasu odpowiedzi w tym miejscu jest tak, `<deny>` reguły jest umieszczany obowiązująca i użytkownik zostanie przekierowany do strony logowania. Podobnie, jeśli odwiedzania Jisun `UrlAuthorizationModule` rozpoczyna się od udzielenia jest Jisun Scott lub Tito? Ponieważ użytkownik nie jest `UrlAuthorizationModule` będzie kontynuowana, drugie pytanie, jest Jisun w zestawie każdy? Ona jest, więc użytkownik, zbyt, odmowa dostępu. Ponadto jeśli Tito odwiedza, pierwsze pytanie powodowane `UrlAuthorizationModule` jest za odpowiedzi, więc Tito uzyskuje dostęp.

Ponieważ `UrlAuthorizationModule` reguły autoryzacji z góry do dołu, zatrzymywania na wszystkie dopasowania, ważne jest, aby mieć bardziej szczegółowych reguł przed szerszym te procesy. Oznacza to można zdefiniować reguły autoryzacji zabraniać Jisun i użytkowników anonimowych, ale zezwala na wszystkie inne uwierzytelnionych użytkowników, można będzie uruchomić z regułą specyficzny — co wpływa na procesy Jisun — a następnie przejdź do mniej dokładne zasady — te, dzięki czemu wszystkie inne Użytkownicy uwierzytelnieni, ale odmawianie wszyscy użytkownicy anonimowi. Następujących reguł autoryzacji adresów URL implementuje te zasady, najpierw odmawianie Jisun, a następnie zezwalających na każdy użytkownik anonimowy. Każdy użytkownik uwierzytelniony, innego niż Jisun otrzyma prawa dostępu ponieważ oba te `<deny>` instrukcje będą zgodne.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Krok 2: Ustalenie przepływu pracy nieautoryzowanego, uwierzytelnionym użytkownikom.

Jak wspomniano wcześniej w tym samouczku w A przyjrzyj sekcji przepływu pracy autoryzacji adresu URL, w każdej chwili techniczną nieautoryzowanego żądania, `UrlAuthorizationModule` anuluje żądanie i zwraca stanu HTTP 401 nieautoryzowane. Tego stanu 401 jest modyfikowany przez `FormsAuthenticationModule` do 302 przekierowania stanu, która wysyła użytkownika do strony logowania. Ten przepływ pracy występuje na wszystkie nieautoryzowanego żądania, nawet wtedy, gdy użytkownik jest uwierzytelniony.

Zwracanie uwierzytelnionego użytkownika do strony logowania jest prawdopodobnie mylić je, ponieważ już są zalogowani do komputera. Z niewielki pracy możemy ulepszyć tego przepływu pracy przez przekierowywanie uwierzytelnionych użytkowników, którzy tworzą nieautoryzowanych żądań do strony, który objaśnia, że będą oni próbowali dostępu stronę z ograniczeniami.

Rozpocznij od utworzenia nowej strony ASP.NET w folderze głównym aplikacji sieci web o nazwie `UnauthorizedAccess.aspx`; Pamiętaj skojarzyć tę stronę w `Site.master` strony wzorcowej. Po utworzeniu tej strony, usuń formant zawartości, który odwołuje się do `LoginContent` ContentPlaceHolder tak, aby domyślny zawartości strony wzorcowej będą wyświetlane. Następnie dodaj komunikat, który wyjaśnia sytuację, czyli użytkownik próbował uzyskać dostęp do chronionych zasobów. Po dodaniu takiego komunikatu `UnauthorizedAccess.aspx` deklaratywne znaczników strony powinien być podobny do następującego:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Teraz musisz zmienić przepływu pracy, tak aby Jeśli nieautoryzowanego żądania odbywa się za pośrednictwem uwierzytelnionego użytkownika są wysyłane do `UnauthorizedAccess.aspx` strony zamiast strony logowania. Logika, który przekierowuje nieautoryzowanych żądań do strony logowania jest ukryty w prywatnej metody `FormsAuthenticationModule` klasy, dlatego firma Microsoft nie można dostosować to zachowanie. Co można zrobić, jednak jest dodawanie do strony logowania, który przekierowuje użytkownika do własnej logiki `UnauthorizedAccess.aspx`, jeśli to konieczne.

Gdy `FormsAuthenticationModule` przekierowuje nieautoryzowany użytkownik do strony logowania dołącza adres URL żądanego, nieautoryzowanego querystring o nazwie `ReturnUrl`. Na przykład, jeśli nieautoryzowany użytkownik próbował odwiedź `OnlyTito.aspx`, `FormsAuthenticationModule` spowoduje przekierowanie do `Login.aspx?ReturnUrl=OnlyTito.aspx`. W związku z tym jeśli strony logowania jest osiągalny uwierzytelnionego użytkownika z ciąg zapytania, który zawiera `ReturnUrl` parametr, a następnie możemy wiedzieć, że ten nieuwierzytelniony użytkownik po prostu próbował odwiedź stronę, użytkownik nie ma uprawnień do wyświetlania. W takim przypadku chcemy się jej przekierowania `UnauthorizedAccess.aspx`.

Aby to zrobić, Dodaj następujący kod do strony logowania `Page_Load` obsługi zdarzeń:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Powyższy kod przekierowuje uwierzytelniony, nieautoryzowanym użytkownikom `UnauthorizedAccess.aspx` strony. Aby wyświetlić tę logikę w akcji, odwiedź witrynę jako użytkownik anonimowy i kliknij łącze tworzenia kont użytkowników w lewej kolumnie. Spowoduje to przejście do `~/Membership/CreatingUserAccounts.aspx` strony, która w kroku 1 został skonfigurowany tylko do Tito zezwolenie na dostęp. Ponieważ użytkownicy anonimowi są niedozwolone, `FormsAuthenticationModule` przekierowuje nam wróć do strony logowania.

Na tym etapie firma Microsoft są anonimowe, więc `Request.IsAuthenticated` zwraca `false` i firma Microsoft nie są przekierowywane do `UnauthorizedAccess.aspx`. Zamiast tego zostanie wyświetlona strona logowania. Zaloguj się jako użytkownik innych niż Tito, takich jak Bruce. Po wprowadzeniu odpowiednie poświadczenia, logowanie strony przekierowuje nam z powrotem do `~/Membership/CreatingUserAccounts.aspx`. Jednak ponieważ ta strona jest dostępna wyłącznie dla Tito, firma Microsoft są bez autoryzacji, aby go wyświetlić i szybko nastąpi powrót do strony logowania. Teraz, jednak `Request.IsAuthenticated` zwraca `true` (i `ReturnUrl` parametr querystring istnieje), dlatego firma Microsoft są przekierowywane do `UnauthorizedAccess.aspx` strony.


[![Uwierzytelniony, nieautoryzowani użytkownicy są przekierowywani do UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Rysunek 6**: uwierzytelniony, nieautoryzowani użytkownicy są przekierowywani do `UnauthorizedAccess.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image18.png))


Ten dostosowane przepływ pracy przedstawia bardziej rozsądnego i proste środowisko użytkownika przez krótki circuiting cyklu przedstawione na rysunku 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Krok 3: Ograniczanie funkcjonalności na podstawie aktualnie zalogowanego użytkownika

Autoryzacja adresów URL ułatwia określenie reguł autoryzacji ogólnym. Jak widzieliśmy w kroku 1 z Autoryzacja adresów URL firma Microsoft może krótkiej formie stanu tożsamości, jakie są dozwolone i które są zabronione wyświetlania określonej strony lub wszystkich stron w folderze. W niektórych scenariuszach jednak może chcemy zezwala wszystkim użytkownikom, odwiedź stronę, ale ograniczenia funkcji strony na podstawie użytkownika, odwiedzając go.

Należy wziąć pod uwagę w przypadku witryny sieci Web handlu elektronicznego, który umożliwia uwierzytelnionym Przejrzyj ich produkty. Użytkownik anonimowy odwiedza stronę produktu, zobaczy tylko informacji o produkcie i może nie mieć możliwość pozostawić przeglądu. Uwierzytelniony użytkownik odwiedzający tej samej stronie zobaczy jednak recenzowania interfejsu. Uwierzytelniony użytkownik nie ma jeszcze przejrzane tego produktu, interfejs umożliwia im Prześlij opinię; w przeciwnym razie go będzie je wyświetlić ich wcześniej zgłoszonych przeglądu. Aby móc w tym scenariuszu krok Ponadto stronę produktu może Pokaż dodatkowe informacje i oferują rozszerzoną funkcje dla tych użytkowników, które działają w firmie handlu elektronicznego. Na przykład strona produktu może listę spisu w magazynie i obejmują opcje do edytowania cen produktu i opis po odwiedzeniu przez pracownika.

Takie zasady autoryzacji poprawnie ziarna można zaimplementować deklaratywnie lub programowo (albo przez kombinację obu). W następnej sekcji zostanie wyświetlone implementowania autoryzacji ziarna poprawnie za pomocą formantu LoginView. Następujące, przeanalizujemy programowe technik. Zanim można przyjrzymy stosowania reguły autoryzacji ziarna poprawnie, jednak najpierw należy utworzyć strony, których funkcji zależy od użytkownika, odwiedzając go.

Umożliwia tworzenie strony z listą plików w katalogu określonym w widoku GridView. Wraz z wyświetlania nazwy każdego pliku, rozmiar i inne informacje, widoku GridView będzie zawierać dwóch kolumn LinkButtons: jeden zatytułowany widoku i jeden zatytułowana Delete. Jeśli kliknięto element LinkButton widoku zostanie wyświetlona zawartość wybranego pliku; Jeśli kliknięto element LinkButton usunąć plik zostanie usunięty. Początkowo Utwórzmy tej strony tak, aby jego Wyświetl i usuń funkcje są dostępne dla wszystkich użytkowników. W używanie sekcje formantu LoginView i programowo Ograniczanie funkcjonalności, należy sprawdzić, jak włączyć lub wyłączyć te funkcje na podstawie użytkownika, odwiedzając stronę.

> [!NOTE]
> Strony ASP.NET, który mamy kompilacji używa kontrolce GridView, aby wyświetlić listę plików. Od tego samouczka, który seria skupia się na uwierzytelnianie formularzy, autoryzacji, kont użytkowników i role nie chcę spędzają zbyt dużo czasu dyskutować przebiega kontrolki widoku siatki. Ten samouczek zawiera określone instrukcje krok po kroku dotyczące konfigurowania tej strony, natomiast nie delve do szczegółów Dlaczego wprowadzono niektórych opcji lub właściwości określonym wpływ ma na renderowanych danych wyjściowych. Zbadania kontrolki widoku siatki można znaleźć w mojej  *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)*  samouczka serii.


Uruchamianie przez otwarcie `UserBasedAuthorization.aspx` w pliku `Membership` folderu i dodaniu kontrolce GridView stronę o nazwie `FilesGrid`. W tagu w widoku GridView kliknij łącze Edytowanie kolumn, aby uruchomić okno dialogowe pola. W tym miejscu należy usunąć zaznaczenie pola wyboru pola automatycznego generowania, w lewym dolnym rogu. Następnie dodaj przycisk Wybierz, przycisk Usuń, a BoundFields dwa z lewym górnym rogu (Wybierz i usuń przyciski można znaleźć w elemencie CommandField typu). Ustaw przycisku Wybierz `SelectText` właściwości widoku i pierwszego elementu BoundField `HeaderText` i `DataField` właściwości Name. Ustaw drugiego elementu BoundField `HeaderText` właściwości rozmiar w bajtach, jego `DataField` właściwości Length, jego `DataFormatString` właściwości do {0: N0} i jego `HtmlEncode` wartość False dla właściwości.

Po skonfigurowaniu kolumny w widoku GridView, kliknij przycisk OK, aby zamknąć okno dialogowe pola. W oknie właściwości należy ustawić w widoku GridView `DataKeyNames` właściwości `FullName`. W tym momencie deklaratywne znaczników w widoku GridView powinna wyglądać następująco:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Adiustację w widoku GridView jest utworzony możemy przystąpić do pisania kodu, który pobiera pliki w określonym katalogu i powiązać je z widoku GridView. Dodaj następujący kod do strony `Page_Load` obsługi zdarzeń:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Powyższy kod używa [ `DirectoryInfo` klasy](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.aspx) do uzyskiwania listy plików w folderze głównym. [ `GetFiles()` Metody](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.getfiles.aspx) zwraca wszystkie pliki w katalogu jako tablica [ `FileInfo` obiektów](https://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx), który następnie jest powiązany z widoku GridView. `FileInfo` Obiekt ma pewną liczbę właściwości, takie jak `Name`, `Length`, i `IsReadOnly`, między innymi. Jak widać z jego deklaratywne znaczników widoku GridView wyświetla tylko `Name` i `Length` właściwości.

> [!NOTE]
> `DirectoryInfo` i `FileInfo` klasy znajdują się w [ `System.IO` przestrzeni nazw](https://msdn.microsoft.com/en-us/library/system.io.aspx). W związku z tym będzie albo należy poprzedzić nazwy klas z ich nazwy przestrzeni nazw lub przestrzeń nazw, zaimportować do pliku klasy (za pośrednictwem `using System.IO`).


Poświęć chwilę na tej stronie za pośrednictwem przeglądarki. Będzie wyświetlany na liście plików znajdujących się w katalogu głównym aplikacji. Kliknięcie dowolnej z widoku lub usunięcie LinkButtons powoduje odświeżenie strony, ale żadna akcja ze strony nastąpi, ponieważ jeszcze mamy do tworzenie obsługi zdarzeń niezbędne.


[![Widoku GridView Wyświetla listę plików w katalogu głównym aplikacji sieci Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Rysunek 7**: widoku GridView Wyświetla listę plików w katalogu głównym aplikacji sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image21.png))


Potrzebujemy, środek do wyświetlenia zawartości wybranego pliku. Wróć do programu Visual Studio i Dodaj pole tekstowe o nazwie `FileContents` powyżej widoku GridView. Ustaw jego `TextMode` właściwości `MultiLine` i jego `Columns` i `Rows` właściwości 95% i 10, odpowiednio.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla w widoku GridView [ `SelectedIndexChanged` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) i Dodaj następujący kod:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Ten kod używa w widoku GridView `SelectedValue` właściwości, aby określić pełną nazwę wybranego pliku. Wewnętrznie `DataKeys` odwołuje się do kolekcji w celu uzyskania `SelectedValue`, więc jest konieczne, aby ustawić w widoku GridView `DataKeyNames` właściwości Name, jak opisano wcześniej w tym kroku. [ `File` Klasy](https://msdn.microsoft.com/en-us/library/system.io.file.aspx) jest używany do odczytu zawartość wybranego pliku jako ciąg, który następnie jest przypisany `FileContents` w pole tekstowe `Text` właściwości, w ten sposób wyświetlania zawartości na stronie wybranego pliku.


[![Zawartość pliku wybrane są wyświetlane w polu tekstowym](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Rysunek 8**: wybrane jego zawartość są wyświetlane w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Jeśli wyświetlanie zawartości pliku, który zawiera kod znaczników HTML, a następnie spróbuj wyświetlić lub usunąć plik, zostanie wyświetlony `HttpRequestValidationException` błędu. Dzieje się tak, ponieważ na odświeżenie strony zawartość pole tekstowe są wysyłane z powrotem do serwera sieci web. Domyślnie program ASP.NET zgłasza `HttpRequestValidationException` błąd, gdy wykryto potencjalnie niebezpieczną zawartość odświeżania strony, np. znaczników HTML. Aby wyłączyć ten błąd występuje, Wyłącz sprawdzanie poprawności żądań na stronie przez dodanie `ValidateRequest="false"` do `@Page` dyrektywy. Aby uzyskać więcej informacji o zaletach weryfikacji żądania jako poprawnie jako jakie środki ostrożności należy wykonać, gdy wyłączenie go, przeczytaj [weryfikacji żądań - zapobieganie atakom skryptu](https://asp.net/learn/whitepapers/request-validation/).


Na koniec należy dodać program obsługi zdarzeń z następującym kodem elementu GridView [ `RowDeleting` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Kod po prostu Wyświetla pełną nazwę pliku do usunięcia w `FileContents` pole tekstowe *bez* faktycznie usuwania pliku.


[![Kliknięcie przycisku Usuń rzeczywistości nie usuwa plik](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Rysunek 9**: kliknięcie przycisku Usuń przycisk rzeczywistości nie usuwa plik ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image27.png))


W kroku 1 został skonfigurowany reguł autoryzacji adresów URL, aby uniemożliwić użytkownikom anonimowym wyświetlania stron w `Membership` folderu. Aby lepiej wykazują ziarna przeprowadzać uwierzytelnianie, umożliwia Zezwalaj użytkownikom anonimowym odwiedź `UserBasedAuthorization.aspx` strony, ale z ograniczoną funkcjonalnością. Otwarcie tej stronie można uzyskać dostępu do wszystkich użytkowników, Dodaj następujący `<location>` elementu `Web.config` w pliku `Membership` folderu:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Po dodaniu to `<location>` elementu testowania nowych reguł autoryzacji adresów URL przez wylogowanie lokacji. Jako użytkownik anonimowy użytkownik ma uprawnienia do odwiedź `UserBasedAuthorization.aspx` strony.

Obecnie każdy użytkownik uwierzytelnieni lub anonimowi mogą odwiedzać `UserBasedAuthorization.aspx` strony i wyświetlania lub usuwania plików. Można wprowadzić go tak, aby tylko uwierzytelnieni użytkownicy mogą wyświetlać zawartość pliku i tylko Tito można usunąć pliku. Deklaratywnie, programowo lub przy użyciu kombinacji obu metod można zastosować takie zasady autoryzacji ziarna poprawnie. Użyjmy deklaratywne podejście, aby ograniczyć, kto może wyświetlać zawartość pliku; użyjemy programowe podejście do limit, który można usunąć pliku.

### <a name="using-the-loginview-control"></a>Za pomocą formantu LoginView

Jak możemy przedstawiono w ciągu ostatnich samouczkach, formantu LoginView przydaje się do wyświetlania różnych interfejsów dla użytkowników uwierzytelnionych i anonimowych i zapewnia prosty sposób, aby ukryć funkcje, które nie jest dostępna dla użytkowników anonimowych. Ponieważ użytkownicy anonimowi nie można wyświetlić lub usunąć pliki, potrzebujemy pokazanie `FileContents` pole tekstowe, gdy strona jest odwiedzanych przez użytkownika uwierzytelnionego. Aby to osiągnąć, należy dodać formantu LoginView do strony, nadaj jej nazwę `LoginViewForFileContentsTextBox`i Przenieś `FileContents` deklaratywne znaczników w pole tekstowe do formantu LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Formanty sieci Web w szablonach LoginView nie są już dostępne bezpośrednio z klasy związane z kodem. Na przykład `FilesGrid` w widoku GridView `SelectedIndexChanged` i `RowDeleting` procedury obsługi zdarzeń obecnie odwołania `FileContents` formantu TextBox z kodem, takich jak:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Jednak ten kod nie jest już prawidłowy. Przenosząc `FileContents` pole tekstowe do `LoggedInTemplate` pole tekstowe nie są bezpośrednio dostępne. Zamiast tego należy używamy `FindControl("controlId")` metodę programowo odwołania formantu. Aktualizacja `FilesGrid` procedury obsługi zdarzeń, aby odwołać się pole tekstowe w następujący sposób:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Po przeniesieniu pole tekstowe do LoginView `LoggedInTemplate` i aktualizowanie kodu strony do odwołania przy użyciu pola tekstowego `FindControl("controlId")` wzorca, odwiedź stronę jako użytkownik anonimowy. Jak pokazano na rysunku nr 10, `FileContents` nie jest wyświetlane pole tekstowe. Element LinkButton widoku nadal jest wyświetlany.


[![Formantu LoginView renderuje tylko pole tekstowe FileContents dla uwierzytelnionych użytkowników](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Na rysunku nr 10**: LoginView sterowania tylko renderuje `FileContents` pole tekstowe dla użytkowników uwierzytelnionych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image30.png))


Jednym ze sposobów Ukryj przycisk Widok dla użytkowników anonimowych jest przekonwertuj GridView pole na pole TemplateField. Spowoduje to wygenerowanie szablon zawierający deklaratywne znaczników dla LinkButton widoku. Firma Microsoft można dodać formantu LoginView TemplateField i umieścić LinkButton w LoginView `LoggedInTemplate`, a tym samym ukrywanie przycisku Widok od odwiedzających anonimowy. Aby to zrobić, kliknij łącze Edytowanie kolumn z tagów inteligentnych w widoku GridView, aby uruchomić okno dialogowe pola. Następnie kliknij przycisk Wybierz z listy w lewym dolnym rogu, a następnie kliknij przycisk Konwertuj to pole na pole TemplateField łącze. W ten sposób spowoduje jej modyfikacji pola deklaratywne znaczników z:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Do: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

W tym momencie można dodać LoginView na pole TemplateField. Następujący kod przedstawia LinkButton widoku tylko dla użytkowników uwierzytelnionych.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Jak pokazano na rysunku nr 11, wynik końcowy nie jest czy pretty jako widok kolumny jest nadal wyświetlany nawet LinkButtons widoku w kolumnie są ukryte. Przedstawiono, jak ukryć całej kolumny widoku GridView (a nie tylko element LinkButton) w następnej sekcji.


[![Formantu LoginView ukrywa LinkButtons widoku dla użytkowników anonimowych](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Rysunek 11**: formantu LoginView ukrywa LinkButtons widoku dla użytkowników anonimowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programowo ograniczenie funkcjonalności

W niektórych sytuacjach deklaratywne techniki są niewystarczające do ograniczania funkcjonalności do strony. Na przykład dostępność niektórych funkcji strony może być zależny od kryteriów poza czy użytkownika, odwiedzając stronę jest anonimowy lub uwierzytelniony. W takich przypadkach można wyświetlić lub ukryte w sposób programowy różne elementy interfejsu użytkownika.

Aby programowo ograniczenie funkcjonalności, należy wykonać dwie czynności:

1. Określić, czy użytkownik, odwiedzając stronę można uzyskać dostęp do funkcji i
2. Zmodyfikuj programowo interfejsu użytkownika oparte na Określa, czy użytkownik ma dostęp do funkcji programu.

Aby zademonstrować stosowania tych dwóch zadań, umożliwia Zezwalaj tylko na Tito usunąć pliki z widoku GridView. Naszym pierwszym zadaniem, wówczas do ustalenia, czy Tito, odwiedzając stronę. Po określeniu który musimy (lub ukryć) w widoku GridView Usuń kolumnę. Kolumny w widoku GridView są dostępne za pośrednictwem jego `Columns` właściwości; kolumna jest renderowany tylko, jeśli jego `Visible` właściwość jest ustawiona na `true` (ustawienie domyślne).

Dodaj następujący kod do `Page_Load` obsługi zdarzeń przed powiązaniu danych widoku GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Jak wspomniano w [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczka `User.Identity.Name` zwraca nazwę tożsamości. Odpowiada podanej nazwy użytkownika w formancie logowania. Jeśli jest Tito odwiedzania strony, druga kolumna w widoku GridView `Visible` właściwość jest ustawiona na `true`; w przeciwnym razie wartość jest równa `false`. Wynikiem jest, że gdy ktoś innych niż Tito odwiedzi strony innego użytkownika uwierzytelnionego lub użytkownik anonimowy, Usuń kolumnę nie są odtwarzane (patrz rysunek 12); Jednak gdy Tito odwiedzających stronę, Usuń kolumnę występuje (patrz rysunek 13).


[![Kolumna usunąć jest nie renderowane podczas odwiedzona przez kogoś innego niż Tito (na przykład Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Rysunek 12**: kolumna usunąć jest nie renderowane podczas odwiedzona przez kogoś innego niż Tito (na przykład Bruce) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image36.png))


[![Usuń kolumnę jest renderowany Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Rysunek 13**: kolumna usunąć jest renderowany Tito ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Krok 4: Stosowanie reguł autoryzacji do klasy i metody

W kroku 3 możemy niedopuszczalne anonimowym użytkownikom wyświetlanie zawartości pliku i wolno wszystkich użytkowników, ale Tito usuwania plików. To osiągnąć ukrywanie elementów interfejsu użytkownika skojarzonego dla nieautoryzowanych użytkowników za pomocą techniki deklaratywne i programowe. W naszym przykładzie prosty prawidłowo ukrywanie elementy interfejsu użytkownika zostało utrudnione, ale co bardziej złożonych witryn gdzie może występować wiele różnych sposobów, aby wykonać te same funkcje? Ograniczanie funkcja nieautoryzowanym użytkownikom, co się stanie, jeśli firma Microsoft Pamiętaj, aby ukryć lub Wyłącz wszystkie elementy interfejsu użytkownika dotyczy?

To prosty sposób sprawdzić, czy z określonym wystąpieniem funkcji nie jest dostępna przez nieautoryzowanego użytkownika do dekoracji tej klasy lub metody za pomocą [ `PrincipalPermission` atrybutu](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowiska uruchomieniowego .NET korzysta z klasy lub wykonuje jeden z jego metod, sprawdza, aby upewnić się, że bieżący kontekst zabezpieczeń ma uprawnienia do korzystania z klasy lub wykonać metody. `PrincipalPermission` Atrybutu zapewnia mechanizm, za pomocą którego możemy można zdefiniować te reguły.

Umożliwia pokazanie sposobu używania `PrincipalPermission` atrybutu w widoku GridView `SelectedIndexChanged` i `RowDeleting` procedury obsługi zdarzeń, aby zabronić wykonywania przez użytkowników anonimowych, jak i użytkowników innych niż Tito, odpowiednio. To wszystko, co należy zrobić, dodać odpowiedni atrybut nad każdym definicji funkcji:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Atrybut `SelectedIndexChanged` określają procedury obsługi zdarzeń, które użytkownicy uwierzytelnieni tylko może zostać uruchomiony program obsługi zdarzeń, gdzie jako atrybut na `RowDeleting` na Tito wykonywania ogranicza obsługi zdarzeń.

Jeśli jakiś sposób, próbuje wykonać użytkownika innego niż Tito `RowDeleting` program obsługi zdarzeń lub z systemem innym niż uwierzytelniony użytkownik podejmuje próbę wykonania `SelectedIndexChanged` obsługi zdarzeń środowiska uruchomieniowego .NET zostanie podniesiony `SecurityException`.


[![Jeśli kontekst zabezpieczeń nie ma uprawnień do wykonania metody, jest zgłaszany SecurityException](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Rysunek 14**: Jeśli kontekst zabezpieczeń nie ma uprawnień do wykonania metody `SecurityException` zgłoszono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Umożliwia wielu kontekstów zabezpieczeń dostępu klasa lub metoda do dekoracji klasy lub metody za pomocą `PrincipalPermission` atrybutu dla każdej kontekstu zabezpieczeń. Oznacza to aby zezwalać na ruch Tito i Bruce, można wykonać `RowDeleting` program obsługi zdarzeń, Dodaj *dwóch* `PrincipalPermission` atrybuty:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Oprócz stron ASP.NET wiele aplikacji ma architekturę, która obejmuje różne warstwy, takie jak logiki biznesowej i warstwy dostępu do danych. Warstwy te zwykle są zaimplementowane jako bibliotek klas i oferują klasy i metody do wykonywania funkcji związanych logikę i dane biznesowe. `PrincipalPermission` Atrybut jest przydatne w przypadku stosowania reguł autoryzacji do tych warstw.

Aby uzyskać więcej informacji na temat używania `PrincipalPermission` atrybutu definiują reguły autoryzacji na klasy i metody, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/)jego wpis w blogu [Dodawanie reguły autoryzacji w celu biznesowych i danych zapomocąwarstw`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku Analizujemy sposób stosowania reguły autoryzacji na podstawie użytkownika. Możemy korzystać z przyjrzeć się ASP. W NET framework autoryzacji adresu URL. Na każdym żądania, aparatu ASP.NET `UrlAuthorizationModule` przeprowadzający reguł autoryzacji adresów URL, które są zdefiniowane w konfiguracji aplikacji, aby określić, czy tożsamość jest autoryzowany do dostępu do żądanego zasobu. Krótko mówiąc Autoryzacja adresów URL ułatwia określenie reguł autoryzacji dla określonej strony lub dla wszystkich stron z określonego katalogu.

Framework autoryzacji adresu URL stosuje reguły autoryzacji na podstawie strony strona. Z Autoryzacja adresów URL żądania tożsamości jest autoryzowany do uzyskania dostępu do określonego zasobu lub nie. Wiele scenariuszy, jednak wymagają więcej reguł autoryzacji ziarna poprawnie. Zamiast określające, kto może uzyskać dostęp do strony, firma Microsoft może być konieczne aby umożliwić wszystkim dostępu do strony, ale pokazywanie różnych danych lub oferować różne funkcje, w zależności od użytkownika, odwiedzając stronę. Zezwolenie na poziomie strony wiąże się zwykle do ukrywania elementów interfejsu użytkownika Aby zapobiec nieautoryzowanym użytkownikom uzyskiwanie dostępu do funkcji zabronione. Ponadto jest możliwość użycia atrybutów do ograniczania dostępu do klasy i wykonywania jej metody dla określonych użytkowników.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dodawanie reguły autoryzacji do działalności biznesowej i warstwy danych przy użyciu`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autoryzacja w programie ASP.NET](https://msdn.microsoft.com/en-us/library/wce3kxhd.aspx)
- [Zmiany między IIS6 i zabezpieczeń IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurowanie konkretne pliki i podkatalogi](https://msdn.microsoft.com/en-us/library/6hbkh9s7.aspx)
- [Ograniczenia funkcji modyfikacji danych oparte na użytkownika](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Przewodniki Szybki Start formantu LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Opis Autoryzacja adresów URL usług IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`Dokumentacja techniczna](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx)
- [Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Poprzednie](validating-user-credentials-against-the-membership-user-store-cs.md)
[dalej](storing-additional-user-information-cs.md)
