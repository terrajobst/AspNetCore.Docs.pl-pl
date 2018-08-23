---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Tworzenie interfejsu do wybierania jednego konta użytkownika spośród wielu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy interfejs użytkownika z siatką stronicowane, filtrowania. W szczególności interfejsu użytkownika będzie składać się z szeregu LinkButtons dla...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 863ac36ae6a94ece841088db925c04deb3bf36c9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755478"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Tworzenie interfejsu do wybierania jednego konta użytkownika spośród wielu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> W tym samouczku utworzymy interfejs użytkownika z siatką stronicowane, filtrowania. W szczególności interfejsu użytkownika będzie składać się z szeregu LinkButtons filtrowania wyników na podstawie początkowej litery nazwy użytkownika i kontrolki widoku siatki aby pokazać pasujących użytkowników. Zaczniemy od listą wszystkich kont użytkowników w GridView. Następnie w kroku 3, dodamy filtru LinkButtons. Krok 4 patrzy na stronicowanie wyników filtrowania. Interfejs zbudowane w krokach od 2 do 4 będzie służyć w kolejnych samouczkach do wykonywania zadań administracyjnych dla określonego konta użytkownika.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *przypisywanie ról do użytkowników* ](../roles/assigning-roles-to-users-cs.md) samouczku utworzyliśmy podstawowe interfejsu dla administratora, wybierz użytkownika i zarządzanie jej rolami. W szczególności interfejsu przedstawione przez administratora za pomocą listy rozwijanej wszyscy użytkownicy. Ten interfejs jest odpowiednia, gdy istnieją, ale tuzina lub tak użytkownika konta, ale jest niewygodna witryn z setkami lub tysiącami kont. Siatka stronicowane, filtrowanie jest bardziej odpowiedni interfejs użytkownika służący do witryn sieci Web z bazami dużej liczby użytkowników.

W tym samouczku utworzymy interfejsu użytkownika. W szczególności interfejsu użytkownika będzie składać się z szeregu LinkButtons filtrowania wyników na podstawie początkowej litery nazwy użytkownika i kontrolki widoku siatki aby pokazać pasujących użytkowników. Zaczniemy od listą wszystkich kont użytkowników w GridView. Następnie w kroku 3, dodamy filtru LinkButtons. Krok 4 patrzy na stronicowanie wyników filtrowania. Interfejs zbudowane w krokach od 2 do 4 będzie służyć w kolejnych samouczkach do wykonywania zadań administracyjnych dla określonego konta użytkownika.

Zaczynajmy!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i kolejne dwa firma Microsoft będzie mieć badanie różnych funkcji związanych z administracją i możliwości. Musimy szereg stron ASP.NET do zaimplementowania tematy dotyczące badania w tych samouczkach. Umożliwia tworzenie tych stron i zaktualizuj mapy witryny.

Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `Administration`. Następnie dodaj dwa nowe strony programu ASP.NET do folderu, łączenia każdego stronę z `Site.master` strony wzorcowej. Nazwa strony:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Również dodać dwie strony do katalogu głównego witryny sieci Web: `ChangePassword.aspx` i `RecoverPassword.aspx`.

Te cztery strony, w tym momencie powinna mieć dwóch kontrolek zawartości, jeden dla każdej strony wzorcowej kontrolek ContentPlaceHolder: `MainContent` i `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Chcemy wyświetlić znaczników domyślną stronę wzorcową dla `LoginContent` ContentPlaceHolder na tych stronach. W związku z tym, Usuń oznaczeniu deklaracyjnym dla `Content2` formantu zawartości. Po wykonaniu tej czynności, znaczników strony powinna zawierać tylko jeden formant zawartości.

Strony ASP.NET w programie `Administration` folderu są przeznaczone wyłącznie do użytkowników administracyjnych. Dodaliśmy roli administratora systemu w <a id="_msoanchor_2"> </a> [ *tworzenie i zarządzanie rolami* ](../roles/creating-and-managing-roles-cs.md) samouczek; ograniczyć dostęp do tych dwóch stron do tej roli. Aby to zrobić, należy dodać `Web.config` plik `Administration` folder i skonfigurować jej `<authorization>` element przyjmowania użytkowników w roli administratora i odrzucanie wszystkich innych.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

Na tym etapie projektu w Eksploratorze rozwiązań powinny wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 1.


[![Cztery nowe strony i pliku Web.config zostały dodane do witryny sieci Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Rysunek 1**: cztery nowe strony i `Web.config` plików zostały dodane do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Na koniec zaktualizuj mapy witryny (`Web.sitemap`) do uwzględnienia wpis do `ManageUsers.aspx` strony. Dodaj następujący kod XML po `<siteMapNode>` dodaliśmy samouczki ról.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Za pomocą mapy witryny aktualizacji odwiedź witrynę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, nawigacji po lewej stronie teraz zawiera elementy samouczki administracji.


[![Mapa witryny zawiera węzeł o nazwie Administracja użytkownikami](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Rysunek 2**: mapy witryny zawiera węzeł o nazwie użytkownika administracji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Krok 2: Lista wszystkich kont użytkowników w widoku GridView

Naszym celem końcowego na potrzeby tego samouczka jest tworzenie siatki stronicowane, filtrowania, za pomocą których administrator może wybrać opcję konto użytkownika do zarządzania. Zacznijmy od listy *wszystkich* użytkowników w GridView. Po zakończeniu tej operacji, dodamy, filtrowanie i stronicowanie interfejsy i funkcje.

Otwórz `ManageUsers.aspx` strony w `Administration` folderze i Dodaj GridView, ustawiając jego `ID` do `UserAccounts`. Za chwilę, możemy napisać kod, aby powiązać zbiór kont użytkowników przy użyciu GridView `Membership` klasy `GetAllUsers` metody. Zgodnie z opisem w starszych samouczki, metoda GetAllUsers zwraca `MembershipUserCollection` obiektu, który jest kolekcją z `MembershipUser` obiektów. Każdy `MembershipUser` w kolekcji zawiera właściwości, takie jak `UserName`, `Email`, `IsApproved`i tak dalej.

Aby wyświetlić informacje o koncie żądanego użytkownika w widoku GridView, ustaw GridView `AutoGenerateColumns` wartość False dla właściwości i dodać BoundFields dla `UserName`, `Email`, i `Comment` właściwości i CheckBoxFields dla `IsApproved`, `IsLockedOut`, i `IsOnline` właściwości. Tę konfigurację można zastosować za pomocą formantu oznaczeniu deklaracyjnym lub za pomocą okna dialogowego pól. Rysunek 3 przedstawia zrzut ekranu: pola dialogowego po usunięto zaznaczenie pola wyboru pól automatycznego generowania i BoundFields i CheckBoxFields została dodana i skonfigurowana.


[![Dodaj trzy BoundFields i trzy CheckBoxFields do widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Rysunek 3**: Dodaj trzy BoundFields i trzy CheckBoxFields do kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


Po skonfigurowaniu usługi GridView, upewnij się, że jego oznaczeniu deklaracyjnym podobny do następującego:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Następnie należy napisać kod, który wiąże kont użytkowników do widoku GridView. Utwórz metodę o nazwie `BindUserAccounts` do wykonania tego zadania, a następnie wywołać go z `Page_Load` program obsługi zdarzeń pierwszej wizyty strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Poświęć chwilę, aby przetestować stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 4, `UserAccounts` GridView Wyświetla nazwę użytkownika, adres e-mail i innych informacji dotyczących konta dla wszystkich użytkowników w systemie.


[![Konta użytkowników są wyświetlane w widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Rysunek 4**: konta użytkowników są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Krok 3: Filtrowanie wyników za pierwsze litery nazwy użytkownika

Obecnie `UserAccounts` pokazuje GridView *wszystkich* kont użytkowników. Dla witryn sieci Web z setkami lub tysiącami kont użytkowników konieczne jest, że użytkownik będzie mógł szybko okrojenia wyświetlanych kont. Można to osiągnąć przez dodanie filtrowanie LinkButtons ze stroną. Dodajmy 27 LinkButtons do strony: jeden o nazwie wszystkie wraz z jeden element LinkButton dla każdej litery alfabetu. Jeśli użytkownik kliknie przycisk wszystkie łącza, widoku GridView pokaże wszystkich użytkowników. Po kliknięciu polecenia określonej litery, pojawi się tylko do tych użytkowników, których nazwa użytkownika, który rozpoczyna się od litery wybrane.

Naszym pierwszym zadaniem jest dodać formanty LinkButton 27. Jedną z opcji będzie w sposób deklaratywny, utworzyć 27 LinkButtons pojedynczo. Bardziej elastyczne podejściem jest użycie kontrolką elementu powtarzanego z `ItemTemplate` która renderuje element LinkButton, a następnie powiąże opcje filtrowania do elementu powtarzanego jako `string` tablicy.

Początek, dodając kontrolką elementu powtarzanego do strony powyżej `UserAccounts` GridView. Ustaw Repeater `ID` właściwość `FilteringUI`. Konfigurowanie szablonów elementu powtarzanego tak, aby jego `ItemTemplate` renderuje element LinkButton którego `Text` i `CommandName` właściwości są powiązane z bieżącego elementu tablicy. Jak widzieliśmy w <a id="_msoanchor_3"> </a> [ *przypisywanie ról do użytkowników* ](../roles/assigning-roles-to-users-cs.md) samouczków, można to zrobić za pomocą `Container.DataItem` składnia wiązania danych. Użyj elementu powtarzanego `SeparatorTemplate` do wyświetlenia pionowa linia między każdego łącza.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Aby wypełnić tego elementu powtarzanego z odpowiednie opcje filtrowania, należy utworzyć metodę o nazwie `BindFilteringUI`. Pamiętaj wywołać tę metodę z `Page_Load` program obsługi zdarzeń w pierwszym załadowaniem strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Ta metoda określa opcje filtrowania jako elementy `string` tablicy `filterOptions`. Dla każdego elementu w tablicy powtarzanego spowoduje, że element LinkButton z jego `Text` i `CommandName` właściwości przypisanych do wartości elementu tablicy.

Rysunek 5. pokazuje `ManageUsers.aspx` stronie podczas przeglądania za pośrednictwem przeglądarki.


[![Powtarzanego Wyświetla 27 LinkButtons filtrowania](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Rysunek 5**: Repeater Wyświetla 27 filtrowanie LinkButtons ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Nazwy użytkowników mogą rozpoczynać się od dowolny znak, w tym liczby i znaki interpunkcyjne. Aby wyświetlić te konta, administrator będzie musiał użyć opcji LinkButton wszystkich. Alternatywnie można dodać LinkButton do zwrócenia wszystkich kont użytkowników, które rozpoczynają się liczbą. Można pozostawić to w charakterze ćwiczenia dla czytnika.


Kliknięcie dowolnego z filtrowania LinkButtons powoduje odświeżenie strony i zgłasza Repeater `ItemCommand` zdarzenia, ale nastąpiła żadna zmiana w siatce, ponieważ zostały wykonane następujące kroki jeszcze do pisania jakiegokolwiek kodu, aby filtrować wyniki. `Membership` Klasa zawiera [ `FindUsersByName` metoda](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) zwracającego tych kont użytkowników, których nazwa użytkownika jest zgodny ze wzorcem wyszukiwania. Możemy użyć tej metody do pobrania tylko tych kont użytkowników, których nazwy użytkowników rozpoczynać się od litery, określony przez `CommandName` systemu filtrowanej element LinkButton, który został kliknięty.

Rozpocznij, aktualizując `ManageUser.aspx` klasy strony związane z kodem, aby obejmowała właściwość o nazwie `UsernameToMatch`. Ta właściwość username — ciąg filtru są utrwalane w ogłaszania zwrotnego:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch` Właściwość przechowuje wartość jest przypisany do `ViewState` kolekcji za pomocą klucza UsernameToMatch. Jeśli wartość tej właściwości jest do odczytu, sprawdza, czy wartość istnieje w `ViewState` kolekcji; w przeciwnym razie jego zwraca wartość domyślna to ciąg pusty. `UsernameToMatch` Właściwość wykazuje wspólny wzorzec, a mianowicie przechowywanie wartości, aby wyświetlić stan, tak aby zmiany właściwości są utrwalane w ogłaszania zwrotnego. Aby uzyskać więcej informacji na temat tego wzorca, przeczytaj [stany widoków środowiska ASP.NET opis](https://msdn.microsoft.com/library/ms972976.aspx).

Następnie zaktualizuj `BindUserAccounts` tak, to zamiast wywoływania metody `Membership.GetAllUsers`, wywoływanych przez nią `Membership.FindUsersByName`, przekazując wartość `UsernameToMatch` właściwość dołączona z symbolem wieloznacznym SQL %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Aby wyświetlić tylko tych użytkowników, których nazwa użytkownika, który rozpoczyna się od litery A, należy ustawić `UsernameToMatch` właściwość A, a następnie wywołać `BindUserAccounts`. Spowodowałoby to wywołanie `Membership.FindUsersByName("A%")`, co zwróci wszystkich użytkowników których nazw rozpoczyna się od A. Podobnie, aby powrócić *wszystkich* użytkowników, przypisywać pusty ciąg, aby `UsernameToMatch` właściwość tak, aby `BindUserAccounts` będą — metoda wywoływanie `Membership.FindUsersByName("%")`, a tym samym zwracanie wszystkich kont użytkowników.

Utwórz procedurę obsługi zdarzeń dla elementu powtarzanego `ItemCommand` zdarzeń. To zdarzenie jest wywoływane po każdym kliknięciu jednego z filtrów LinkButtons; jest przekazywana kliknięto element LinkButton `CommandName` wartość za pośrednictwem `RepeaterCommandEventArgs` obiektu. Należy przypisać odpowiednie wartość `UsernameToMatch` właściwości i następnie wywołaniu `BindUserAccounts` metody. Jeśli `CommandName` wszystkich, jest pusty ciąg, aby przypisać `UsernameToMatch` tak, aby wszystkie konta użytkowników są wyświetlane. W przeciwnym razie przypisać `CommandName` wartość `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Przy użyciu tego kodu w miejscu przetestowania funkcji filtrowania. Po pierwsze odwiedzenia strony, są wyświetlane wszystkie konta użytkowników (odnoszą się do rysunek 5). Klikając element LinkButton powoduje odświeżenie strony i filtruje wyniki wyświetlanie tylko tych kont użytkowników, które zaczyna się element.


[![Użyj LinkButtons filtrowania, aby wyświetlić tych użytkowników, których nazwa użytkownika, który rozpoczyna się od określonej litery](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Rysunek 6**: umożliwia filtrowanie LinkButtons wyświetlania tych użytkowników Whose Username rozpoczyna się od litery niektóre ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Krok 4: Aktualizowanie GridView używać stronicowania

GridView pokazano na ilustracji 5 i 6, zawiera listę wszystkich rekordów zwracanych z `FindUsersByName` metody. W przypadku setek lub tysięcy kont użytkowników może to prowadzić do przeciążenia informacji podczas wyświetlania wszystkich kont (tak jak w przypadku gdy klikając przycisk wszystkie łącza lub początkowo, odwiedzając stronę). Aby przedstawić kont użytkowników w perspektywie fragmentów, Skonfigurujmy GridView, aby wyświetlić 10 kont użytkowników w danym momencie.

W kontrolce GridView oferuje dwa typy stronicowania:

- **Domyślne stronicowania** — łatwe do wdrożenia, ale nieefektywne. Mówiąc, przy użyciu domyślnego stronicowania widoku GridView oczekuje *wszystkich* rekordy ze źródła danych. Następnie wyświetla odpowiednie strony rekordów.
- **Stronicowania niestandardowego** -wymaga większego nakładu pracy, aby zaimplementować, ale jest bardziej wydajne niż domyślna stronicowania, ponieważ za pomocą niestandardowego stronicowanie danych źródłowych zwraca tylko dokładny zestaw rekordów do wyświetlenia.

Różnica w wydajności domyślne i niestandardowe stronicowania może być bardzo istotne, gdy stronicowanie tysięcy rekordów. Ponieważ tworzymy ten interfejs, zakładając, że może być setek lub tysięcy kont użytkowników, Użyjmy stronicowania niestandardowego.

> [!NOTE]
> Aby uzyskać bardziej dogłębną dyskusję na temat różnic między domyślne i niestandardowe stronicowania, a także wyzwania związane z Implementowanie stronicowania niestandardowego, zobacz [efektywne stronicowanie za pośrednictwem dużych ilości danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Dla niektórych analizy różnic w wydajności między domyślnych i niestandardowych stronicowania, zobacz [stronicowania niestandardowego w technologii ASP.NET za pomocą programu SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Aby zaimplementować niestandardowy stronicowania, najpierw należy pewien mechanizm, za pomocą którego można pobrać dokładne podzestaw rekordów, które są wyświetlane w widoku GridView. Dobra wiadomość jest fakt, że `Membership` klasy `FindUsersByName` metoda musi być przeciążona, który pozwala określić indeks strony i rozmiar strony i zwraca tylko te konta użytkowników, które mieszczą się w tym zakresie rekordów.

W szczególności tego przeciążenia ma następujący podpis: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*PageIndex* parametr określa stronę kont użytkowników ma być zwrócona. *pageSize* wskazuje, ile rekordy do wyświetlenia na jednej stronie. *TotalRecords* parametr `out` parametru, która zwraca liczbę kont wszystkich użytkowników w magazynie użytkownika.

> [!NOTE]
> Dane zwrócone przez `FindUsersByName` są sortowane według nazwy użytkownika; nie można dostosować kryteriów sortowania.


Korzystanie z niestandardowych stronicowania, ale tylko podczas wiązania z formantu ObjectDataSource można skonfigurować widoku GridView. Kontrolka ObjectDataSource zaimplementować niestandardowy stronicowania, wymaga dwóch metod: jeden, który jest przekazywany indeks wiersza początkowego i maksymalną liczbę rekordów do wyświetlenia, i zwraca dokładne podzestaw rekordów, które mieszczą się w tym zakresu i metodę, która zwraca całkowita liczba rekordów jest stronicowana za pośrednictwem. `FindUsersByName` Przeciążenia akceptuje indeks strony i rozmiar strony, a następnie zwraca całkowitej liczbie rekordów za pomocą `out` parametru. Dlatego jest interfejs niezgodność.

Jedną z opcji będzie się do utworzenia klasy serwera proxy, która uwidacznia interfejs kontrolki ObjectDataSource oczekuje, a następnie wywołuje wewnętrznie `FindUsersByName` metody. Inną opcją — i jedną użyjemy w tym artykule — jest utworzyć własną interfejsu stronicowania i używać go zamiast wbudowanego interfejsu stronicowania GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Tworzenie pierwszej poprzedniego, następnie ostatnie stronicowania, interfejs

Z pierwszej, Previous, dalej i ostatniego LinkButtons utworzymy interfejsu stronicowania. Pierwszy element LinkButton, po kliknięciu spowoduje przejście użytkownika do pierwszej strony danych, podczas gdy poprzedni zwraca go do poprzedniej strony. Podobnie następny i ostatni spowoduje przeniesienie użytkownika do dalej i ostatniej strony, odpowiednio. Dodaj cztery kontrolki LinkButton poniżej `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Następnie należy utworzyć procedurę obsługi zdarzeń dla każdego element LinkButton `Click` zdarzenia.

Rysunek nr 7 przedstawia cztery LinkButtons podczas wyświetlania przy użyciu widoku projektowania wizualnego dewelopera sieci Web.


[![Dodać pierwszego, poprzedniego, w następnej kolejności i ostatniej LinkButtons poniżej widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Rysunek 7**: Dodaj pierwszy, Wstecz Dalej i ostatniego LinkButtons poniżej kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Rejestrowanie informacji o indeks bieżącej strony

Gdy użytkownik odwiedza najpierw `ManageUsers.aspx` strony lub kliknięcia jednego z filtrowania przyciski, chcemy wyświetlić stronę pierwszego danych w widoku GridView. Gdy użytkownik kliknie jeden nawigacji LinkButtons, jednak należy zaktualizować indeks strony. Do utrzymania, indeks strony i liczbę rekordów do wyświetlenia na jednej stronie, Dodaj dwie poniższe właściwości do klasy CodeBehind strony:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Podobnie jak `UsernameToMatch` właściwości `PageIndex` właściwości jego wartość, aby wyświetlić stan będzie się powtarzać. Tylko do odczytu `PageSize` właściwość zwraca ustaloną wartość 10. Zaprosić zainteresowane czytnika zaktualizować tę właściwość, aby użyć tego samego wzorca jako `PageIndex`, a następnie rozszerzyć `ManageUsers.aspx` strony w taki sposób, że osoby, odwiedzając stronę można określić, jak wiele kont użytkowników do wyświetlenia na jednej stronie.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Tylko rekordy bieżącej strony pobierania, aktualizowania indeks strony i włączanie i wyłączanie stronicowania LinkButtons interfejsu

Za pomocą interfejsu stronicowania w miejscu i `PageIndex` i `PageSize` dodawać właściwości jesteśmy gotowi zaktualizować `BindUserAccounts` metodę, tak że używa odpowiednie `FindUsersByName` przeciążenia. Ponadto firma Microsoft musi być tej metody, włączanie lub wyłączanie interfejsu stronicowania w zależności od tego, jaki strony są wyświetlane. Podczas wyświetlania na pierwszej stronie danych, należy wyłączyć łącza pierwszy i poprzedni; Następnie i ostatnio powinno być wyłączone podczas wyświetlania ostatniej strony.

Aktualizacja `BindUserAccounts` metoda następującym kodem:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Należy pamiętać, że całkowita liczba rekordów, które są stronicowane za pośrednictwem zależy od ostatni parametr `FindUsersByName` metody. Jest to `out` parametru, musimy najpierw zadeklarować zmienną zawierającą wartość tego parametru (`totalRecords`) i następnie poprzedzić go `out` — słowo kluczowe.

Po określonej strony konta użytkowników są zwracane, cztery LinkButtons są włączone lub wyłączone, w zależności od tego, czy jest wyświetlany pierwszej lub ostatniej strony danych.

Ostatnim krokiem jest napisanie kodu dla czterech LinkButtons `Click` procedury obsługi zdarzeń. Te procedury obsługi zdarzeń konieczne zaktualizowanie `PageIndex` właściwości i następnie ponowne powiązywanie danych w kontrolce GridView poprzez wywołanie `BindUserAccounts`. Pierwszy Wstecz i dalej procedury obsługi zdarzeń są bardzo proste. `Click` Program obsługi zdarzeń dla ostatni element LinkButton, jednak jest nieco bardziej złożone, ponieważ należy ustalić, ile rekordy są wyświetlane w celu ustalenia, której ostatni indeks strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Rysunki 8 i 9 Pokaż niestandardowy interfejs stronicowania w działaniu. Rysunek 8 przedstawia `ManageUsers.aspx` stronie podczas wyświetlania na pierwszej stronie danych we wszystkich kontach użytkownika. Należy pamiętać, że wyświetlane są tylko 10 kont 13. Kliknięcie łącza Dalej lub ostatni powoduje odświeżenie strony, aktualizacje `PageIndex` 1 w celu powiązania z drugiej strony użytkownika konta do siatki (patrz rysunek 9).


[![Pierwsze 10 konta użytkowników są wyświetlane.](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Rysunek 8**: pierwszego konta użytkownika 10 są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Klikając łącze do następnej Wyświetla drugiej stronie kont użytkowników](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Rysunek 9**: klikając łącze do następnej Wyświetla drugiej stronie kont użytkowników ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Administratorzy często muszą wybierz użytkownika z listy kont. W poprzednich samouczkach przyjrzeliśmy się za pomocą listy rozwijanej, wypełnione z użytkownikami, ale to podejście nie działa poprawnie. W tym samouczku rozważyliśmy lepszym: filtrowanie interfejsu, których wyniki są wyświetlane w stronicowane GridView. Z tym interfejsem użytkownika administratorzy mogą szybko i skutecznie zlokalizować i wybierania jednego konta użytkownika spośród tysięcy.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Niestandardowe stronicowania w technologii ASP.NET za pomocą programu SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Efektywne stronicowanie dużych ilości danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Stopniowe własne narzędzia do administrowania witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Alicja Maziarz. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](recovering-and-changing-passwords-cs.md)
