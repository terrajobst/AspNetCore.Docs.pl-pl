---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: "Tworzenie interfejsu, aby wybrać jedno konto użytkownika z wielu (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku budujemy interfejs użytkownika z siatką stronicowanej, można filtrować. W szczególności naszych interfejsu użytkownika będzie składać się z szeregu LinkButtons dla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a8fb48b8c8cfb653ac4d64f6efe011f92b966b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Tworzenie interfejsu, aby wybrać jedno konto użytkownika z wielu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> W tym samouczku budujemy interfejs użytkownika z siatką stronicowanej, można filtrować. W szczególności naszych interfejs użytkownika zostanie składają się z szeregu LinkButtons filtrowania wyników na podstawie początkowej litery nazwy użytkownika i kontrolce GridView pokazanie zgodnych użytkowników. Zaczniemy poprzez wyszczególnienie wszystkich kont użytkowników w widoku GridView. Następnie w kroku 3 dodamy filtru LinkButtons. Krok 4 analizuje stronicowania wyników filtrowania. Interfejs wykonane kroki od 2 do 4 będzie używany w kolejnych samouczkach do wykonywania zadań administracyjnych dla określonego konta użytkownika.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *przypisywanie ról użytkowników* ](../roles/assigning-roles-to-users-cs.md) samouczek, utworzyliśmy podstawowe interfejs dla administratora, wybierz użytkownika i zarządzać jego role. W szczególności interfejsu przedstawione administrator z listy rozwijanej wszystkich użytkowników. Takiego interfejsu jest odpowiednia w przypadku, gdy istnieją, ale użytkownik dwanaście lub tak kont, ale jest niewygodna witryn z setkami lub tysiącami kont. Siatka stronicowanej, filtrowanie jest odpowiedniejsze interfejsu użytkownika dla witryny sieci Web z typów podstawowych dużej liczby użytkowników.

W tym samouczku budujemy interfejsu użytkownika. W szczególności naszych interfejs użytkownika zostanie składają się z szeregu LinkButtons filtrowania wyników na podstawie początkowej litery nazwy użytkownika i kontrolce GridView pokazanie zgodnych użytkowników. Zaczniemy poprzez wyszczególnienie wszystkich kont użytkowników w widoku GridView. Następnie w kroku 3 dodamy filtru LinkButtons. Krok 4 analizuje stronicowania wyników filtrowania. Interfejs wykonane kroki od 2 do 4 będzie używany w kolejnych samouczkach do wykonywania zadań administracyjnych dla określonego konta użytkownika.

Dzieła!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i dwie firma Microsoft będzie można badanie różnych funkcji związanych z administracją i możliwości. Szereg stron ASP.NET do zaimplementowania tematy zbadać w całym te samouczki są wymagane. Umożliwia tworzenie tych stron i zaktualizuj mapy witryny.

Rozpocznij od utworzenia nowego folderu do projektu o nazwie `Administration`. Następnie dodaj do folderu, łączenie każdej strony z dwóch nowych stron ASP.NET `Site.master` strony wzorcowej. Nazwa strony:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Również dodać dwie strony do katalogu głównego witryny sieci Web: `ChangePassword.aspx` i `RecoverPassword.aspx`.

Te cztery strony w tym momencie powinna mieć dwóch formantów zawartości, jeden dla każdej strony wzorcowej Elementy ContentPlaceHolders: `MainContent` i `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Chcemy wyświetlić kod znaczników domyślnej strony wzorcowej dla `LoginContent` elementu ContentPlaceHolder na tych stronach. W związku z tym usunąć deklaratywne kod znaczników dla `Content2` zawartości formantu. Po wykonaniu tej czynności, znaczników stron powinna zawierać tylko jeden formant zawartości.

Strony ASP.NET w `Administration` folderu są przeznaczone wyłącznie do użytkowników administracyjnych. Dodaliśmy roli administratora systemu w <a id="_msoanchor_2"> </a> [ *tworzenie i zarządzanie rolami* ](../roles/creating-and-managing-roles-cs.md) samouczek; ograniczyć dostęp do tych dwóch stron do tej roli. W tym celu należy dodać `Web.config` pliku `Administration` folder i skonfigurować jej `<authorization>` element przyjmowania użytkowników w roli administratora, aby odmówić wszystkich innych.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

W tym momencie projektu Eksploratora rozwiązań powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 1.


[![Dodano czterech nowych stron i pliku Web.config witryny sieci Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Rysunek 1**: czterech nowych stron i `Web.config` pliku zostały dodane do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Na koniec zaktualizuj mapy witryny (`Web.sitemap`) do uwzględnienia wpis do `ManageUsers.aspx` strony. Dodaj następujący kod XML po `<siteMapNode>` dodaliśmy samouczki ról.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Z mapy witryny zaktualizowany odwiedź witrynę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, nawigacji po lewej stronie teraz obejmuje elementy samouczki administracji.


[![Mapy witryny zawiera węzeł zatytułowany Administracja użytkownikami](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Rysunek 2**: mapy witryny obejmuje Administracja użytkownikami zatytułowany węzła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Krok 2: Lista wszystkich kont użytkowników w widoku GridView

Naszym celem zakończenia na potrzeby tego samouczka jest tworzenie stronicowanej, filtrowanie siatki, za pomocą którego administrator może wybrać konto użytkownika do zarządzania. Zacznijmy od listy *wszystkie* użytkowników w widoku GridView. Po zakończeniu tej operacji, dodamy filtrowania stronicowania interfejsów oraz funkcji i możliwości.

Otwórz `ManageUsers.aspx` strony `Administration` folderu i Dodaj element GridView ustawienie jej `ID` do `UserAccounts`. Za chwilę, firma Microsoft będzie napisać kod, aby powiązać zestaw kont użytkowników przy użyciu widoku GridView `Membership` klasy `GetAllUsers` metody. Zgodnie z opisem w starszych samouczki, metoda GetAllUsers zwraca `MembershipUserCollection` obiektu, który jest kolekcją z `MembershipUser` obiektów. Każdy `MembershipUser` w kolekcji zawiera właściwości, takie jak `UserName`, `Email`, `IsApproved`i tak dalej.

Aby wyświetlić informacje o koncie odpowiedniego użytkownika w widoku GridView, należy ustawić w widoku GridView `AutoGenerateColumns` wartość False dla właściwości i dodać BoundFields dla `UserName`, `Email`, i `Comment` właściwości i CheckBoxFields dla `IsApproved`, `IsLockedOut`, i `IsOnline` właściwości. Ta konfiguracja może odnosić się za pomocą formantu deklaratywne znaczników lub za pomocą okna dialogowego pól. Rysunek 3 przedstawia zrzut ekranu: pola — okno dialogowe po usunięto zaznaczenie pola wyboru pola automatycznego generowania i BoundFields i CheckBoxFields zostały dodane i skonfigurowane.


[![Dodaj trzy BoundFields i trzy CheckBoxFields do widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Rysunek 3**: Dodaj trzy BoundFields i trzy CheckBoxFields do widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


Po skonfigurowaniu sieci GridView, upewnij się, że jego deklaratywne znaczników podobny do następującego:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Następnie należy napisać kod, który wiąże kont użytkowników z widoku GridView. Utwórz metodę o nazwie `BindUserAccounts` do wykonania tego zadania, a następnie wywołać ją z `Page_Load` obsługi zdarzeń pierwszej wizyty strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Poświęć chwilę, aby przetestować za pośrednictwem przeglądarki. Jak pokazano na rysunku 4, `UserAccounts` GridView Wyświetla nazwę użytkownika, adres e-mail i inne informacje istotne konta dla wszystkich użytkowników w systemie.


[![Konta użytkowników są wyświetlane w widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Rysunek 4**: konta użytkowników są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Krok 3: Filtrowanie wyników przez pierwszą literę nazwy użytkownika

Obecnie `UserAccounts` pokazuje GridView *wszystkie* kont użytkowników. Dla witryn sieci Web z setkami lub tysiącami kont użytkowników konieczne jest ten użytkownik mieć szybko nawiasu dół wyświetlane konta. Można to zrobić przez dodanie filtrowania LinkButtons do strony. Dodajmy 27 LinkButtons do strony: jeden zatytułowany wszystkie wraz z jednego LinkButton dla każdej litery alfabetu. Po kliknięciu wszystkich LinkButton widoku GridView będzie pokazywać wszystkich użytkowników. Jeśli odbiorca kliknie konkretną literę, będą wyświetlane tylko tych użytkowników, których nazw rozpoczyna się od wybranej litery.

Naszym pierwszym zadaniem jest Dodaj formanty LinkButton 27. Jedną z opcji byłoby deklaratywnie, Utwórz 27 LinkButtons pojedynczo. Bardziej elastyczne rozwiązaniem jest użycie kontrolce elementu powtarzanego z `ItemTemplate` która renderuje element LinkButton i następnie wiąże opcje filtrowania powtarzanego jako `string` tablicy.

Start, dodając kontrolce elementu powtarzanego do strony powyżej `UserAccounts` widoku GridView. Ustaw elemencie powtarzanym `ID` właściwości `FilteringUI`. Konfigurowanie szablonów w elemencie powtarzanym, aby jego `ItemTemplate` renderuje element LinkButton których `Text` i `CommandName` właściwości są powiązane z bieżącego elementu tablicy. Jak widzieliśmy w <a id="_msoanchor_3"> </a> [ *przypisywanie ról użytkowników* ](../roles/assigning-roles-to-users-cs.md) samouczek, można to zrobić przy użyciu `Container.DataItem` składnia wiązania z danymi. Użyj elementu powtarzanego `SeparatorTemplate` do wyświetlenia pionowych linii między każdego łącza.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Aby wypełnić tego elementu powtarzanego z żądanymi opcjami filtrowania, Utwórz metodę o nazwie `BindFilteringUI`. Pamiętaj wywołać tę metodę z `Page_Load` obsługi zdarzeń pierwszy ładowania strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Ta metoda określa opcje filtrowania jako elementy `string` tablicy `filterOptions`. Dla każdego elementu w tablicy powtarzanego spowoduje, że element LinkButton z jego `Text` i `CommandName` właściwości, przypisane do wartości elementu tablicy.

Rysunek 5. pokazuje `ManageUsers.aspx` strony podczas wyświetlania za pośrednictwem przeglądarki.


[![Powtarzanego wymieniono 27 LinkButtons filtrowania](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Rysunek 5**: elementu powtarzanego wymieniono 27 filtrowania LinkButtons ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Nazwy użytkowników mogą rozpoczynać się od znaków, łącznie z cyfr i znaków interpunkcyjnych. Aby wyświetlić te konta, administrator będzie musiał użyć opcji wszystkie LinkButton. Alternatywnie można dodać LinkButton do zwrócenia wszystkich kont użytkowników rozpoczynających się od numeru. I pozostaw to wykonywania dla czytnika.


Kliknięcie dowolnej z filtrowania LinkButtons powoduje odświeżenie strony i zgłasza elemencie powtarzanym `ItemCommand` zdarzenia, ale nie ma żadnej zmiany w siatce, ponieważ jeszcze mamy do pisania kodu do filtrowania wyników. `Membership` Klasa zawiera [ `FindUsersByName` metody](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) zwracającą kont użytkowników, których nazwa użytkownika jest zgodna z wzorcem wyszukiwania. Możemy użyć tej metody do pobrania tylko tych kont użytkowników, których nazwy użytkowników rozpoczynać się od litery, określony przez `CommandName` z filtrowanych LinkButton, który został kliknięty.

Uruchom aktualizując `ManageUser.aspx` kodem strony klasy co zawierają one właściwość o nazwie `UsernameToMatch`. Ta właściwość będzie się powtarzał username — ciąg filtru między ogłaszania zwrotnego:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch` Właściwość przechowuje wartość jest przypisany do `ViewState` kolekcji przy użyciu klucza UsernameToMatch. Jeśli wartość tej właściwości jest do odczytu, sprawdza, czy wartość istnieje w `ViewState` kolekcji; w przeciwnym razie go zwraca wartość domyślna to ciąg pusty. `UsernameToMatch` Właściwość wykazuje wspólnego wzorca, czyli utrwalanie wartość, aby wyświetlić stan, tak aby zmiany właściwości są zachowywane między ogłaszania zwrotnego. Aby uzyskać więcej informacji na ten wzorzec odczytu [stan widoku ASP.NET opis](https://msdn.microsoft.com/library/ms972976.aspx).

Następnie zaktualizuj `BindUserAccounts` tak to zamiast metod wywoływania `Membership.GetAllUsers`, wywołuje `Membership.FindUsersByName`, przekazując wartość `UsernameToMatch` właściwość dołączona z symbolem wieloznacznym SQL %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Aby wyświetlić tylko tych użytkowników, których nazw rozpoczyna się od litery A, ustaw `UsernameToMatch` A dla właściwości, a następnie wywołać `BindUserAccounts`. To spowoduje wywołanie `Membership.FindUsersByName("A%")`, którego zostanie zwrócona wszystkich użytkowników, których nazw rozpoczyna się od A. Podobnie, aby powrócić *wszystkie* pusty ciąg, aby przypisać użytkowników, `UsernameToMatch` właściwości, aby `BindUserAccounts` zostanie — metoda wywołanie `Membership.FindUsersByName("%")`, a tym samym zwracanie wszystkich kont użytkowników.

Utwórz program obsługi zdarzeń dla elementu powtarzanego `ItemCommand` zdarzeń. To zdarzenie jest wywoływane zawsze, gdy zostanie kliknięty jeden z filtrów LinkButtons; jest przekazywany klikniętej LinkButton `CommandName` wartości za pośrednictwem `RepeaterCommandEventArgs` obiektu. Musimy odpowiednią wartość, aby przypisać `UsernameToMatch` właściwości, a następnie wywołania `BindUserAccounts` metody. Jeśli `CommandName` , jest pusty ciąg, aby przypisać `UsernameToMatch` tak, aby wszystkie konta użytkowników są wyświetlane. W przeciwnym razie przypisać `CommandName` do wartości `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Z tego kodu w miejscu należy przetestować funkcji filtrowania. Po pierwsze odwiedzenia strony, są wyświetlane wszystkie konta użytkowników (odwołują się do rysunek 5). Kliknięcie przycisku A LinkButton powoduje odświeżenie strony i filtruje wyniki, wyświetlanie tylko tych kont użytkowników, które zaczynają się A.


[![Umożliwia wyświetlanie tych użytkowników, których nazw rozpoczyna się od określonej litery LinkButtons filtrowania](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Rysunek 6**: Użyj LinkButtons filtrowania, aby wyświetlić te użytkowników Whose Username zaczynał się literą niektórych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Krok 4: Aktualizowanie widoku GridView do użycia stronicowania

GridView pokazano wartości 5 i 6 zawiera listę wszystkich rekordów zwróconych z `FindUsersByName` metody. Jeśli istnieją setek lub tysięcy kont użytkowników może to prowadzić do przeciążenia informacji podczas wyświetlania wszystkich kont (tak jak w przypadku po kliknięciu przycisku wszystkie LinkButton lub podczas odwiedzania początkowo strony). Aby przedstawić kont użytkowników na łatwiejsze w zarządzaniu fragmentów, Skonfigurujmy widoku GridView, aby wyświetlić 10 kont użytkownika w czasie.

Kontrolki widoku siatki oferuje dwa rodzaje stronicowania:

- **Domyślne stronicowania** — łatwa do wdrożenia, ale nieefektywne. Mówiąc, z domyślnymi stronicowanie w widoku GridView oczekuje *wszystkie* rekordy ze źródła danych. Następnie wyświetla jedynie odpowiednie strony rekordów.
- **Stronicowania niestandardowego** -wymaga więcej pracy, aby zaimplementować, ale jest bardziej efektywne niż domyślne stronicowania, ponieważ z niestandardowych stronicowania danych źródła zwraca dokładny zestaw rekordów do wyświetlenia.

Różnica w wydajności domyślne i niestandardowe stronicowania może być bardzo istotne, gdy stronicowania za pośrednictwem tysiące rekordów. Ponieważ firma Microsoft budowania ten interfejs, zakładając, że mogą być setek lub tysięcy kont użytkowników, Użyjmy stronicowania niestandardowego.

> [!NOTE]
> Dokładniejsze omówienie na temat różnic między domyślne i niestandardowe stronicowania, a także wyzwania związane z implementującej stronicowania niestandardowego, można znaleźć w temacie [wydajnie stronicowania za pośrednictwem dużych ilości danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Dla niektórych analizy różnicy wydajności między domyślnymi i stronicowania niestandardowego [stronicowania niestandardowego w programie ASP.NET z programu SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Aby zaimplementować stronicowania niestandardowego, najpierw musimy mechanizmu za pomocą którego można pobrać dokładne podzestaw będzie wyświetlany w widoku GridView. Dobre wieści jest to, że `Membership` klasy `FindUsersByName` metody przeciążenia, które pozwala określić indeks strony i rozmiar strony i zwraca tylko tych kont użytkowników, które wchodzi w zakres rekordów.

W szczególności tego przeciążenia ma następującą sygnaturą: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*PageIndex* parametr określa strony kont użytkowników ma być zwrócona. *pageSize* wskazuje liczbę rekordów do wyświetlenia na jednej stronie. *TotalRecords* parametr jest `out` parametr, który zwraca liczbę całkowitą konta w magazynie użytkownika.

> [!NOTE]
> Dane zwrócone przez `FindUsersByName` są sortowane według nazwy użytkownika; nie można dostosować kryteriów sortowania.


Korzystanie z stronicowania niestandardowego, ale po powiązany tylko z kontrolki ObjectDataSource można skonfigurować widoku GridView. Kontrolki ObjectDataSource do implementowania stronicowania niestandardowego, wymaga dwóch metod: klucz, który jest przekazywany indeks wiersza początkowego i maksymalną liczbę rekordów do wyświetlenia, i zwraca dokładne podzestaw należące do tego zakresu; i metoda, która zwraca całkowita liczba rekordów jest stronicowana za pośrednictwem. `FindUsersByName` Przeciążenie akceptuje indeksu strony i rozmiar strony i zwraca całkowita liczba rekordów za pośrednictwem `out` parametru. Dlatego jest tutaj niezgodność interfejsu.

Jedną z opcji będzie można utworzyć klasy proxy, która uwidacznia interfejs ObjectDataSource oczekuje, a następnie wewnętrznie wywołuje `FindUsersByName` metody. Inną opcją - i, użyjemy tego artykułu - ma Utwórz swój własny interfejs stronicowania i używać zamiast wbudowanego interfejsu stronicowania w widoku GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Tworzenie pierwszej, poprzedniej, następnie ostatni stronicowania — interfejs

Z pierwszej, poprzedni dalej i ostatniego LinkButtons utworzymy interfejsu stronicowania. Pierwszy LinkButton, po kliknięciu potrwa użytkownika do pierwszej strony danych, natomiast wstecz zostanie mu powrót do poprzedniej strony. Podobnie następny i ostatni przeniesie użytkownika do następnej i ostatniej strony, odpowiednio. Dodaj formanty cztery LinkButton poniżej `UserAccounts` widoku GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla każdego element LinkButton `Click` zdarzenia.

Na rysunku nr 7 przedstawiono cztery LinkButtons podczas wyświetlania za pośrednictwem widoku Visual Web Developer projektu.


[![Dodaj pierwszej, poprzedniej, obok, a ostatni LinkButtons poniżej widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Rysunek 7**: Dodaj pierwszy, poprzedni dalej i ostatniego LinkButtons poniżej widok GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Rejestrowanie informacji o bieżącego indeksu strony

Gdy użytkownik odwiedza najpierw `ManageUsers.aspx` strony lub kliknięcia przycisków jedną filtrowania, chcemy wyświetlić dane na pierwszej stronie w widoku GridView. Gdy użytkownik kliknie nawigacji LinkButtons, jednak należy zaktualizować indeks strony. Aby zachować indeksu strony i liczba rekordów wyświetlanych na każdej stronie, Dodaj następujące dwie właściwości do klasy związane z kodem strony:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Podobnie jak `UsernameToMatch` właściwość `PageIndex` właściwości jego wartość, aby wyświetlić stan będzie się utrzymywał. Tylko do odczytu `PageSize` właściwość zwraca wartość wpisaną na stałe, 10. Zaprosić zainteresowanych czytnika, aby zaktualizować tę właściwość, aby użyć tego samego wzorca jako `PageIndex`, a następnie rozszerzyć `ManageUsers.aspx` strony w taki sposób, że osoby, odwiedzając stronę można określić, jak wiele kont użytkowników do wyświetlenia na jednej stronie.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Pobieranie tylko bieżącej strony rekordy, aktualizowania indeksu strony i włączanie i wyłączanie LinkButtons interfejsu stronicowania

Przy użyciu interfejsu stronicowania w miejscu i `PageIndex` i `PageSize` dodać właściwości, możemy gotowości do zaktualizowania `BindUserAccounts` metodę, tak że używa odpowiednie `FindUsersByName` przeciążenia. Ponadto firma Microsoft musi być tej metody, włączanie lub wyłączanie interfejsu stronicowania w zależności od tego, jaką strona jest wyświetlany. Podczas wyświetlania na pierwszej stronie danych, powinny być wyłączone łącza pierwszy i poprzedni; Następnie i ostatnio powinny być wyłączone podczas wyświetlania ostatniej strony.

Aktualizacja `BindUserAccounts` metodę z następującym kodem:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Należy pamiętać, że całkowita liczba rekordów jest stronicowana za pośrednictwem jest określany przez ostatni parametr `FindUsersByName` metody. Jest to `out` parametru, więc musimy najpierw deklaruje zmienną do przechowywania wartości (`totalRecords`), a następnie poprzedzić go `out` — słowo kluczowe.

Po zwróceniu określonej strony kont użytkowników, cztery LinkButtons są włączone lub wyłączone w zależności od tego, czy jest wyświetlany pierwszej lub ostatniej strony danych.

Ostatnim krokiem jest napisanie kodu dla LinkButtons cztery `Click` procedury obsługi zdarzeń. Te programy obsługi zdarzeń należy zaktualizować `PageIndex` właściwości, a następnie ponownie powiązać dane do widoku GridView za pośrednictwem wywołania `BindUserAccounts`. Pierwszy Wstecz i dalej obsługi zdarzeń są bardzo proste. `Click` Programu obsługi zdarzeń dla ostatniego LinkButton, jest jednak nieco bardziej skomplikowane ponieważ potrzebujemy określić liczbę rekordów są wyświetlane w celu określenia indeksu ostatniej strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Rysunki 8 do 9 Pokaż niestandardowy interfejs stronicowania w akcji. Rysunek nr 8 przedstawia `ManageUsers.aspx` stronie podczas przeglądania na pierwszej stronie danych dla wszystkich kont użytkowników. Należy pamiętać, że są wyświetlane tylko 10 kont 13. Kliknięcie łącza Dalej lub ostatniego powoduje odświeżenie strony, aktualizacje `PageIndex` 1 i powiązania przy użyciu konta użytkownika na drugiej stronie do siatki (patrz rysunek 9).


[![Pierwszy 10 konta użytkowników są wyświetlane.](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Rysunek 8**: pierwszego konta użytkownika 10 są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Klikając łącze do następnej zostanie wyświetlona strona druga kont użytkowników](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Rysunek 9**: klikając łącze do następnej Wyświetla drugiej strony konta użytkowników ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Administratorzy często muszą wybierz użytkownika z listy kont. W samouczkach poprzedniej analizujemy przy użyciu listy rozwijanej wypełnione z użytkownikami, ale ta metoda nie obsługuje również. W tym samouczku będziemy przedstawione lepszym: interfejs można filtrować, których wyniki są wyświetlane w stronicowanych widoku GridView. Z tym interfejsem użytkownika administratorzy mogą szybkie i skuteczne Znajdź i zaznacz jedno konto użytkownika między tysięcy.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Stronicowania niestandardowego w programie ASP.NET z programem SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Efektywne stronicowania z dużą ilością danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Wycofanie własne narzędzia administracyjnego witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Alicja Maziarz. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](recovering-and-changing-passwords-cs.md)
