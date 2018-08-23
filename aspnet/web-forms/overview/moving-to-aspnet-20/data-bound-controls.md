---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Kontrolki powiązania danych | Dokumentacja firmy Microsoft
author: microsoft
description: Większość aplikacji ASP.NET, zależy od pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały pivotal część w interakcje...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: b115109c7307d05dc9e620378a51a71407204740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754506"
---
<a name="data-bound-controls"></a>Kontrolki powiązania danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Większość aplikacji ASP.NET, zależy od pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały pivotal część interakcji z danymi w dynamicznych aplikacji sieci Web. Program ASP.NET 2.0 wprowadzono znaczne ulepszenia do formantów powiązanych z danymi, w tym nową klasę BaseDataBoundControl i składni deklaratywnej.


Większość aplikacji ASP.NET, zależy od pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały pivotal część interakcji z danymi w dynamicznych aplikacji sieci Web. Program ASP.NET 2.0 wprowadzono znaczne ulepszenia do formantów powiązanych z danymi, w tym nową klasę BaseDataBoundControl i składni deklaratywnej.

BaseDataBoundControl działa jako klasę bazową dla klasy DataBoundControl i klasy HierarchicalDataBoundControl. W tym module omówimy następujące klasy, które wynikają z DataBoundControl:

- AdRotator
- Kontrolki listy
- GridView
- FormView
- DetailsView

Omówimy również następujące klasy, które pochodzą z klasy HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Klasa DataBoundControl

Klasa DataBoundControl jest klasą abstrakcyjną (oznaczonych MustInherit w języku Visual Basic) używane do interakcji z tabelarycznych lub danych styl listy. Następujące elementy sterujące są niektóre formanty, które wynikają z DataBoundControl.

## <a name="adrotator"></a>AdRotator

Sterowanie AdRotator umożliwia wyświetlanie transparentu grafiki na stronie sieci Web, która jest połączona z określonych adresów URL. Obraca się grafiki, która jest wyświetlana przy użyciu właściwości formantu. Częstotliwość wyświetlania ad określonego na stronie można skonfigurować za pomocą **wyświetleń** właściwości i pokazywania reklam można filtrować przy użyciu słowa kluczowego filtrowania.

Formanty AdRotator na użytek pliku XML lub tabeli w bazie danych. Następujące atrybuty są używane w plikach XML do konfigurowania kontroli AdRotator.

### <a name="imageurl"></a>ImageUrl
Adres URL obrazu do wyświetlenia dla usług ad.

### <a name="navigateurl"></a>NavigateUrl
Adres URL, który użytkownik należy podjąć w celu po kliknięciu ad. Powinna to być zakodowane w adresie URL.

### <a name="alternatetext"></a>AlternateText
Alternatywny tekst, który jest wyświetlany w etykietce narzędzia i odczytany przez czytniki zawartości ekranu. Wyświetla się również, gdy obraz określony przez ImageUrl nie jest dostępna.

### <a name="keyword"></a>Słowo kluczowe
Definiuje słowo kluczowe, które mogą być używane podczas filtrowania — słowo kluczowe. Jeśli zostanie określony, będą wyświetlane tylko reklam ze słowem kluczowym zgodnych z filtrem — słowo kluczowe.

### <a name="impressions"></a>Wyświetleń
Liczba wagi, która określa, jak często określonego ad prawdopodobnie są wyświetlane. Jest ona określana względem wrażenie innych reklam, w tym samym pliku. Maksymalna wartość zbiorowe wrażenia dla wszystkich ogłoszeń w pliku XML jest 2,048,000,000 1.

### <a name="height"></a>Wysokość
Wysokość ad w pikselach.

### <a name="width"></a>Szerokość
Szerokość ad w pikselach.


> [!NOTE]
> Atrybuty wysokość i szerokość przesłonić wysokość i szerokość dla samej kontrolce AdRotator.


Typowy plik XML może wyglądać następująco:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

W powyższym przykładzie ad "contoso" prawdopodobnie dwukrotnie jak są traktowane jako ad dla witryny sieci Web ASP.NET z powodu wartość atrybutu wyświetleń.

Aby wyświetlić reklam z pliku XML powyżej, Dodaj do strony formant AdRotator i ustaw **AdvertisementFile** właściwości, aby wskazać plik XML, jak pokazano poniżej:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Jeśli zdecydujesz się użyć tabeli bazy danych jako źródła danych dla kontrolki AdRotator, należy najpierw skonfigurować bazę danych przy użyciu następującego schematu:

| **Nazwa kolumny** | **Typ danych** | **Opis** |
| --- | --- | --- |
| ID | int | Klucz podstawowy. W tej kolumnie mogą mieć dowolną nazwę. |
| ImageUrl | nvarchar (*długość*) | Względny lub bezwzględny adres URL obrazu do wyświetlenia dla usług ad. |
| NavigateUrl | nvarchar (*długość*) | Docelowy adres URL dla usług ad. Jeśli wartość nie zostanie określona, ad nie jest hiperłącze. |
| AlternateText | nvarchar (*długość*) | Tekst wyświetlany, jeśli nie można odnaleźć obrazu. W niektórych przeglądarkach tekst jest wyświetlany jako etykietka narzędzia. Tekst alternatywny służy także do ułatwień dostępu, aby użytkownicy, którzy nie są widoczne grafiki słyszalny jego opis, Czytaj na głos. |
| Słowo kluczowe | nvarchar (*długość*) | Kategoria dla usługi ad, w którym można filtrować strony. |
| Wyświetleń | int(4) | Liczba, która określa prawdopodobieństwo częstotliwość ad jest wyświetlana. Im większa liczba im częściej ad będzie wyświetlany. Suma wszystkich wartości wyświetleń pliku XML nie może przekraczać 2,048,000,000-1. |
| Szerokość | int(4) | Szerokość obrazu w pikselach. |
| Wysokość | int(4) | Wysokość obrazu w pikselach. |

W przypadkach, gdy masz już bazę danych z innym schematem, można użyć **AlternateTextField**, **ImageUrlField**, i **NavigateUrlField** właściwości do mapowania Funkce AdRotator atrybuty do istniejącej bazy danych. Aby wyświetlić dane z bazy danych w formancie AdRotator, na stronie Dodaj kontrolę źródła danych, skonfigurować parametry połączenia do kontroli źródła danych wskazywał bazy danych i ustawić kontrolkę AdRotator **DataSourceID** Właściwość Identyfikator formantu źródła danych. W przypadkach, gdy istnieje potrzeba programowe Konfigurowanie AdRotator reklam przy użyciu zdarzenia AdCreated. Zdarzenie AdCreated przyjmuje dwa parametry; jeden obiekt, a inne wystąpienia AdCreatedEventArgs. AdCreatedEventArgs jest odwołanie do usługi ad, która jest tworzona.

Poniższy fragment kodu ustawia ImageUrl NavigateUrl i AlternateText dla usługi ad programowe:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Kontrolki listy

Kontrolki listy zawierają pola listy, DropDownList, CheckBoxList, RadioButtonList i BulletedList. Każda z tych kontrolek można dane powiązane ze źródłem danych. Użyj jedno pole w źródle danych tekstem, a opcjonalnie można użyć drugiego pola jako wartość elementu. Elementy mogą być również dodawane statycznie w czasie projektowania, a można łączyć elementy statyczne i dynamiczne elementy dodane źródła danych.

Dane powiązać kontrolkę listy, na stronie Dodaj kontrolę źródła danych. Określ polecenie SELECT do kontroli źródła danych, a następnie ustaw właściwość DataSourceID formantu listy identyfikator formantu źródła danych. Użyj **DataTextField** i **DataValueField** właściwości, aby określić tekst wyświetlany i wartość dla formantu. Ponadto można użyć **DataTextFormatString** właściwość, aby sterować wyglądem wyświetlania tekstu w następujący sposób:

| **Wyrażenie** | **Opis** |
| --- | --- |
| Cena: {0:C} | Aby uzyskać dane liczbowe/dziesiętnych. Wyświetla literału "Cena:" następują cyfry w formacie waluty. Format waluty zależy od ustawień kultury określonej w atrybucie kultury na **strony** dyrektywy lub w pliku Web.config. |
| {0:D4} | Dla danych liczb całkowitych. Nie można używać z liczb dziesiętnych. Liczby całkowite są wyświetlane w polu o zerowej, która jest cztery znaki dwubajtowe. |
| {0:N2}% | Aby uzyskać dane liczbowe. Wyświetla liczbę, stosując miejsce dziesiętne 2 dokładności następuje literału "%". |
| {0:000.0} | Aby uzyskać dane liczbowe/dziesiętnych. Liczby są zaokrąglane do jednego miejsca dziesiętnego. Liczby od mniej niż trzy cyfry są wypełniane przez zera. |
| {0:D} | Aby uzyskać dane daty/godziny. Wyświetla format daty długiej ("czwartek, 06 sierpnia 1996"). Format daty, zależy od ustawienia kulturowe strony lub pliku Web.config. |
| {0:d} | Aby uzyskać dane daty/godziny. Wyświetla daty krótkiej formatu ("12/31/99"). |
| {0:yy-MM-dd} | Aby uzyskać dane daty/godziny. Wyświetla datę w formacie liczbowym rok, miesiąc, dzień (96-08-06) |

## <a name="gridview"></a>GridView

W kontrolce GridView umożliwia dane tabelaryczne wyświetlania i edytowania, przy użyciu podejścia deklaratywnego i jest następcą programu Formant DataGrid. Następujące funkcje są dostępne w kontrolce GridView.

- Powiązanie z danymi kontroli źródła, takich jak SqlDataSource.
- Wbudowane funkcje sortowania.
- Wbudowane aktualizowanie i usuwanie możliwości.
- Wbudowane możliwości stronicowania.
- Wiersz wbudowanej możliwości wyboru.
- Dostęp programowy do modelu obiektu GridView, aby dynamicznie ustawić właściwości, obsługa zdarzeń i tak dalej.
- Wiele pól kluczy.
- Wiele pól danych w kolumnach hiperłącze.
- Można dostosować wygląd za pośrednictwem kompozycje i style.

**Pola kolumn**

Każda kolumna w kontrolce GridView jest reprezentowany przez obiekt DataControlField. Domyślnie ustawiono właściwość właściwość AutoGenerateColumns miała **true**, która tworzy obiekt AutoGeneratedField dla każdego pola w źródle danych. Każde pole jest następnie renderowany jako kolumny w kontrolce GridView w kolejności każde pole pojawia się w źródle danych. Można też ręcznie kontrolować kolumny, które pola są wyświetlane w **GridView** kontroli przez ustawienie **właściwość AutoGenerateColumns miała** właściwości **false** , a następnie własne Kolekcja pól kolumn. Typy pól innej kolumny określają zachowanie kolumn w formancie.

W poniższej tabeli wymieniono typy pól innej kolumny, których można użyć.

| **Typ pola kolumn** | **Opis** |
| --- | --- |
| Elementu BoundField | Wyświetla wartość pola w źródle danych. Jest to domyślny typ kolumny kontrolki GridView. |
| ButtonField | Wyświetla przycisk polecenia dla każdego elementu w kontrolce GridView. Dzięki temu można utworzyć kolumnę formanty przycisków niestandardowych, takich jak dodawanie lub przycisk Usuń. |
| CheckBoxField | Wyświetla pole wyboru dla każdego elementu w kontrolce GridView. Ten typ pola kolumny najczęściej jest używana do wyświetlania pól z wartością logiczną. |
| CommandField | Zawiera wstępnie zdefiniowane przycisków poleceń do wykonania, wybierając, edytowanie lub usuwanie operacji. |
| Pole hiperłącza HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperłącze. Ten typ pola kolumny pozwala powiązać drugie pole adresu URL hiperłącza. |
| ImageField | Wyświetla obraz dla każdego elementu w kontrolce GridView. |
| TemplateField | Wyświetla zawartość zdefiniowanych przez użytkownika dla każdego elementu w kontrolce GridView, zgodnie z określonym szablonem. Ten typ pola kolumny umożliwia tworzenie pola kolumny niestandardowej. |

Aby zdefiniować sposób deklaratywny kolekcji pól kolumn, najpierw dodać otwierające i zamykające **&lt;kolumn&gt;** tagi między otwierającym i zamykającym tagiem kontrolki GridView. Następnie na liście pól kolumn, które chcesz uwzględnić między otwierającym i zamykającym **&lt;kolumn&gt;** tagów. Kolumny określone są dodawane do kolekcji kolumny w podanej kolejności. **Kolumn** kolekcja przechowuje wszystkie kolumny pola w kontrolce i umożliwia programistyczne Zarządzanie pola kolumn w kontrolce GridView.

W połączeniu z pola kolumn automatycznie generowanych, mogą być wyświetlane pola kolumn zadeklarowany w sposób jawny. Gdy używane są obie, zadeklarowany w sposób jawny kolumny są renderowane pola najpierw następuje pola kolumn automatycznie generowanych.

## <a name="binding-to-data"></a>Wiązanie z danymi

Kontrolki GridView może być powiązana z kontroli źródła danych (takich jak **SqlDataSource**, **ObjectDataSource**i tak dalej), jak również wszelkie źródła danych, który implementuje System.Collections.IEnumerable Interfejs (na przykład System.Data.DataView System.Collections.ArrayList lub System.Collections.Hashtable). Do wiązania kontrolki GridView typ źródła danych, użyj jednej z następujących metod:

- Aby powiązać formant źródła danych, ustaw właściwość DataSourceID kontrolki GridView wartości Identyfikatora do kontroli źródła danych. W kontrolce GridView wiąże określone dane do kontroli źródła i automatycznie korzystać z zalet źródła danych kontrolki możliwości sortowania, aktualizowanie, usuwanie i funkcje stronicowania. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje interfejs System.Collections.IEnumerable, programowo ustawić właściwości DataSource kontrolki GridView ze źródłem danych, a następnie wywołać metodę powiązań danych. Przy użyciu tej metody, w kontrolce GridView nie zapewnia wbudowanego sortowanie, aktualizowanie, usuwanie i stronicowanie funkcji. Musisz podać tę funkcję samodzielnie.

## <a name="data-operations"></a>Operacje na danych

W kontrolce GridView udostępnia wiele wbudowanych możliwości, które umożliwiają użytkownikowi sortowania, aktualizowanie, usuwanie, wybierz i strony za pomocą elementów w formancie. Po powiązaniu formantu GridView do kontroli źródła danych, kontrolki GridView można korzystać z zalet źródła danych funkcje formantu i zapewnia automatyczne sortowanie, aktualizowanie i usuwanie funkcji.

> [!NOTE]
> W kontrolce GridView można zapewnić obsługę sortowania, aktualizowanie i usuwanie przy użyciu innych typów źródeł danych; należy jednak zapewnić program obsługi zdarzeń odpowiedniej implementacji dla tych operacji.


Sortowanie umożliwia użytkownikowi sortowanie elementów w kontrolce GridView w odniesieniu do określonej kolumny, klikając nagłówek kolumny. Aby włączyć sortowanie, ustaw właściwość AllowSorting na **true**.

Funkcje automatyczne aktualizowanie, usuwanie i wybór są włączone, gdy przycisk w **ButtonField** lub **TemplateField** pole kolumny o nazwie polecenie "Edytuj", "Delete" i "Wybierz" odpowiednio kliknięciu. W kontrolce GridView może automatycznie dodać **CommandField** pole kolumny z Edytuj, usuń lub wybierz przycisk, jeśli ustawiono właściwość AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateSelectButton **true**odpowiednio.

> [!NOTE]
> Wstawianie rekordów do źródła danych nie są bezpośrednio obsługiwane przez kontrolki GridView. Jednak jest możliwe wstawianie rekordów przy użyciu kontrolki GridView w połączeniu z DetailsView lub FormView kontroli.


Zamiast wyświetlania wszystkich rekordów w źródle danych, w tym samym czasie, kontrolki GridView automatycznie rozbić rekordy na stronach. Aby włączyć stronicowania, ustaw właściwość właściwość AllowPaging na **true**.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Przez ustawienie właściwości stylu dla różnych części kontrolki, można dostosować wygląd kontrolki GridView. W poniższej tabeli wymieniono właściwości innego stylu.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu przemiennych wierszy danych w kontrolce GridView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane przemian ustawienia RowStyle i **AlternatingRowStyle** ustawienia. |
| EditRowStyle | Ustawienia stylu dla wierszy edytowany w kontrolce GridView. |
| EmptyDataRowStyle | Ustawienia stylu dla wiersza danych puste, wyświetlany w kontrolce GridView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wierszy stopce kontrolki GridView. |
| HeaderStyle | Ustawienia stylu dla wiersza nagłówka w kontrolce GridView. |
| PagerStyle | Ustawienia stylu dla wiersza pagera w kontrolce GridView. |
| RowStyle | Ustawienia stylu dla wierszy danych w kontrolce GridView. Gdy **AlternatingRowStyle** właściwość jest również ustawiona, wiersze danych są wyświetlane na przemian **RowStyle** ustawienia i **AlternatingRowStyle** ustawienia. |
| SelectedRowStyle | Ustawienia stylu dla wybranego wiersza w kontrolce GridView. |

Możesz pokazać lub ukryć różnych części kontrolki. W poniższej tabeli wymieniono właściwości, które kontrolują, części, które zostaną pokazane lub ukryte.

| **Property** | **Opis** |
| --- | --- |
| ShowFooter | Pokazuje lub ukrywa stopce kontrolki GridView. |
| ShowHeader | Pokazuje lub ukrywa sekcji nagłówka w kontrolce GridView. |

### <a name="events"></a>Zdarzenia

Formant widoku GridView zapewnia kilka zdarzeń, które można programować względem. Dzięki temu można uruchomić procedury niestandardowe zawsze wtedy, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia obsługiwane przez kontrolki GridView.

| **Event** | **Opis** |
| --- | --- |
| PageIndexChanged | Występuje po kliknięciu jednego z przyciski pagera, ale po kontrolki GridView obsługuje stronicowanie. To zdarzenie jest często używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innej strony w formancie. |
| PageIndexChanging | Występuje po kliknięciu jednego z przyciski pagera, ale przed widoku GridView kontroli obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |
| RowCancelingEdit | Występuje, gdy kliknięto przycisk Anuluj wiersza, ale przed kontrolki GridView zamyka tryb edycji. To zdarzenie jest często używana do zatrzymywania anulowania operacji. |
| RowCommand | Występuje po kliknięciu przycisku w kontrolce GridView. To zdarzenie jest często używane do wykonywania zadań, po kliknięciu przycisku w formancie. |
| RowCreated | Występuje po utworzeniu nowego wiersza w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersza po utworzeniu wiersza. |
| RowDataBound | Występuje, gdy wiersz danych jest powiązany z danymi w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersz po wierszu jest powiązany z danymi. |
| RowDeleted | Występuje po kliknięciu przycisku usuwania wiersza, ale po kontrolki GridView usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby sprawdzić wyniki operacji usuwania. |
| RowDeleting | Występuje, gdy kliknięto przycisk usuwania wiersza, ale przed widoku GridView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| RowEditing | Występuje, gdy kliknięto przycisk Edytuj wiersz, ale przed widoku GridView kontrola przechodzi do trybu edycji. To zdarzenie jest często używane do anulowania operacji edycji. |
| RowUpdated | Występuje, gdy kliknięto przycisk Aktualizuj wiersz, ale po kontrolki GridView aktualizacji wiersza. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| — RowUpdating | Występuje, gdy kliknięto przycisk Aktualizuj wiersz, ale przed widoku GridView kontroli aktualizuje wiersz. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| Selectedindexchanged. | Występuje po kliknięciu przycisku Wybierz wiersz, ale po kontrolki GridView obsługuje operacji wyboru. To zdarzenie jest często używane do wykonywania zadań, po wybraniu wiersza w formancie. |
| SelectedIndexChanging | Występuje, gdy kliknięto przycisk Zaznacz wiersz, ale przed widoku GridView uchwyty operacji wyboru. To zdarzenie jest często używane do anulowania operacji zaznaczenia. |
| Sortowane | Występuje po kliknięciu hiperlinku, aby posortować kolumnę, ale po kontrolki GridView obsługuje operacji sortowania. To zdarzenie najczęściej jest używana do wykonywania zadań, po użytkownik kliknie hiperlink, aby posortować kolumnę. |
| Sortowanie | Występuje po kliknięciu hiperlinku, aby posortować kolumnę, ale przed widoku GridView uchwyty operacji sortowania. To zdarzenie jest często używane, aby anulować operację sortowania lub wykonać niestandardowe procedury sortowania. |

## <a name="formview"></a>FormView

Kontrolka FormView służy do wyświetlenia pojedynczy rekord ze źródła danych. Jest on podobny do kontrolce DetailsView, z wyjątkiem Wyświetla zdefiniowanych przez użytkownika szablonów, zamiast pola wiersza. Tworzenie własnych szablonów zapewnia większą elastyczność w kontrolowaniu, jak dane są wyświetlane. Kontrolka FormView obsługuje następujące funkcje:

- Powiązanie z danymi kontrolki źródła, takich jak SqlDataSource i kontrolki ObjectDataSource.
- Wbudowane możliwości Wstawianie.
- Wbudowane aktualizowanie i usuwanie możliwości.
- Wbudowane możliwości stronicowania.
- Programowy dostęp do modelu obiektów FormView, aby dynamicznie ustawić właściwości, obsługa zdarzeń i tak dalej.
- Można dostosować wygląd za pomocą szablonów zdefiniowanych przez użytkownika, kompozycje i style.

## <a name="templates"></a>Szablony

Dla formantu FormView do wyświetlania zawartości należy utworzyć szablony dla różnych części kontrolki. Większość szablonów są opcjonalne; Jednakże należy utworzyć szablonu tryb, w którym skonfigurowano formantu. Na przykład formant FormView, który obsługuje Wstawianie rekordów musi mieć zdefiniowany szablon elementu insert. W poniższej tabeli wymieniono różne szablony, które można utworzyć.

| **Typ szablonu** | **Opis** |
| --- | --- |
| EditItemTemplate | Definiuje zawartość wiersza danych, jeśli formant FormView jest w trybie edycji. Ten szablon zawiera zazwyczaj kontrolek wejściowych oraz przyciski poleceń, z którą użytkownik może edytować istniejący rekord. |
| EmptyDataTemplate | Definiuje zawartość pustym wierszu danych wyświetlane, gdy kontrolka FormView jest powiązana ze źródłem danych, który nie zawiera żadnych rekordów. Ten szablon zawiera zazwyczaj zawartości, aby ostrzec użytkownika, że źródło danych nie zawiera żadnych rekordów. |
| FooterTemplate | Definiuje zawartość wiersza stopki. Ten szablon zawiera zazwyczaj dodatkowej zawartości, którą chcesz wyświetlić w wierszu stopki. Jako alternatywę można po prostu określić tekst do wyświetlenia w wierszu stopki, ustawiając właściwość FooterText. |
| HeaderTemplate | Definiuje zawartość wiersza nagłówka. Ten szablon zawiera zazwyczaj dodatkowej zawartości, które mają być wyświetlane w wierszu nagłówka. Jako alternatywę można po prostu określić tekst do wyświetlenia w wierszu nagłówka ustawiając właściwość HeaderText. |
| Właściwości ItemTemplate | Definiuje zawartość wiersza danych, gdy kontrolka FormView jest w trybie tylko do odczytu. Ten szablon zawiera zazwyczaj zawartość, aby wyświetlić wartości istniejącego rekordu. |
| InsertItemTemplate | Definiuje zawartość wiersza danych, jeśli formant FormView jest w trybie wstawiania. Ten szablon zawiera zazwyczaj kontrolek wejściowych oraz przyciski poleceń, z którym użytkownik może dodać nowy rekord. |
| PagerTemplate | Definiuje zawartość wiersza pagera, wyświetlane po włączeniu funkcji stronicowania (Jeśli ustawiono właściwość właściwość AllowPaging **true**). Ten szablon zawiera zazwyczaj formantów, z którymi użytkownik może przejść do innego rekordu. |

Kontrolek wejściowych w edycji szablonu elementu i Wstaw szablon elementu może być powiązana z polami źródła danych za pomocą wyrażenia określają powiązanie dwukierunkowe. Umożliwia to kontrolę FormView automatycznie wyodrębnić wartości kontrolki wprowadzania aktualizacji lub operacji wstawiania. Wyrażenia określają powiązanie dwukierunkowe również umożliwiać kontrolek wejściowych w szablonie elementu edycji, aby automatycznie wyświetlać oryginalnej wartości pól.

### <a name="binding-to-data"></a>Wiązanie z danymi

Formant FormView może być powiązany do kontroli źródła danych (takich jak **SqlDataSource**, AccessDataSource, **ObjectDataSource** i tak dalej), lub do żadnego źródła danych, który implementuje Interfejsu System.Collections.IEnumerable (na przykład System.Data.DataView System.Collections.ArrayList i System.Collections.Hashtable). Użyj jednej z następujących metod, aby powiązać formant FormView typ źródła danych:

- Aby powiązać formant źródła danych, ustaw właściwość DataSourceID formantu FormView wartości Identyfikatora do kontroli źródła danych. Formant widoku FormView wiąże określone dane do kontroli źródła i automatycznie korzystać z zalet źródła danych kontrolki możliwości w celu określenia, wstawianie, aktualizowanie, usuwanie i funkcje stronicowania. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje **System.Collections.IEnumerable** interfejsu, programowo ustaw właściwość formantu FormView ze źródłem danych, a następnie wywołać metodę powiązań danych. Przy użyciu tej metody, formant FormView nie zawiera wbudowane Wstawianie, aktualizowanie, usuwanie i funkcje stronicowania. Musisz podać tę funkcję za pomocą odpowiedniego zdarzenia.

## <a name="data-operations"></a>Operacje na danych

Formant widoku FormView udostępnia wiele wbudowanych możliwości, które pozwalają użytkownikowi na aktualizowanie, usuwanie, Wstaw i strony za pomocą elementów w formancie. Gdy formant FormView jest powiązany do kontroli źródła danych, kontroli FormView można zalet źródła danych funkcje formantu i zapewnia automatyczne aktualizowanie, usuwanie, wstawianie i stronicowanie funkcji. Formant widoku FormView można zapewnić obsługę update, delete, insert i operacje stronicowania przy użyciu innych typów źródeł danych; Jednak program obsługi zdarzeń odpowiednie użytkownik musi podać implementacja dla tych operacji.

Ponieważ kontroli FormView korzysta z szablonów, nie zapewnia możliwości automatycznego generowania przycisków poleceń do wykonania, aktualizowanie, usuwanie lub operacje wstawiania. Przyciski te polecenia musisz ręcznie dołączyć odpowiedni szablon. Kontrolka FormView rozpoznaje określone przyciski, które mają ich **CommandName** równa określonej wartości właściwości. Poniższa tabela zawiera listę przycisków poleceń, które rozpoznaje kontroli FormView.

| **Przycisk** | **Wartość CommandName** | **Opis** |
| --- | --- | --- |
| Anuluj | "Cancel" | Używane w aktualizacji lub operacji wstawiania, aby anulować operację i odrzucić wartości wprowadzonej przez użytkownika. Formant widoku FormView powraca do trybu określony przez wartość właściwości DefaultMode. |
| Usuwanie | "Delete" | Używany podczas operacji usuwania, można usunąć wyświetlany rekord ze źródła danych. Wywołuje zdarzenia ItemDeleting i ItemDeleted. |
| Edytowanie | "Edit" | Umożliwia podczas aktualizowania operacji put kontroli FormView w trybie edycji. Zawartości określona w **EditItemTemplate** właściwości są wyświetlane dla wiersza danych. |
| Insert | "Insert" | Używane w operacji wstawiania, aby wstawić nowy rekord w źródle danych przy użyciu wartości dostarczone przez użytkownika. Wywołuje zdarzenia ItemInserting i ItemInserted. |
| New | "New" | Używana w operacji wstawiania do umieścić formant FormView w trybie wstawiania. Zawartości określona w **InsertItemTemplate** właściwości są wyświetlane dla wiersza danych. |
| Strona | "Page" | Używana w operacjach stronicowania do reprezentowania przycisku w wierszu pagera, który wykonuje stronicowania. Aby określić stronicowanie, ustaw **Właściwość CommandArgument** właściwości przycisku "Dalej", "Wstecz", "First", "Last" lub indeks strony, dla której chcesz przejść. Wywołuje zdarzenia PageIndexChanging i PageIndexChanged. |
| Aktualizacja | "Aktualizuj" | Używany podczas aktualizowania operacji próbuje zaktualizować wyświetlany rekord w źródle danych przy użyciu wartości podanych przez użytkownika. Wywołuje zdarzenia ItemUpdating i ItemUpdated. |

W przeciwieństwie do usuwania przycisk (który usuwa wyświetlany rekord natychmiast), po kliknięciu przycisku edycji lub nowy FormView kontrola przechodzi do edycji lub trybie wstawiania odpowiednio. W trybie edycji zawartości znajdujących się w **EditItemTemplate** właściwości są wyświetlane dla bieżącego elementu danych. Edytuj szablon elementu zdefiniowano zazwyczaj taki sposób, że przycisk Edytuj została zastąpiona aktualizacja i przycisk Anuluj. Kontrolki wejściowe, które są odpowiednie dla typu danych pola (na przykład pola tekstowego lub formantu CheckBox) są wyświetlane również zazwyczaj wartość pola użytkownik może go zmodyfikować. Klikając przycisk Aktualizuj aktualizacji rekordu w źródle danych, a kliknięcie przycisku Anuluj porzuca wszystkie zmiany.

Podobnie, zawartość w **InsertItemTemplate** właściwości jest wyświetlana dla elementu danych, gdy kontrolka jest w trybie wstawiania. Wstaw szablon elementu zwykle jest zdefiniowany w taki sposób, że nowy przycisk zostaje zastąpiona opcją wstawiania i przycisk Anuluj, a pusty kontrolki wejściowe są wyświetlane dla użytkownika o podanie wartości dla nowego rekordu. Kliknięcie przycisku Wstaw wstawia rekord w źródle danych, a kliknięcie przycisku Anuluj porzuca wszystkie zmiany.

Formant FormView udostępnia funkcję stronicowania, dzięki czemu można przejść do innych rekordów w źródle danych. Po włączeniu w kontrolce FormView, który zawiera formanty nawigacji na stronie wyświetlany jest wiersz pager. Aby włączyć stronicowania, ustaw **właściwość AllowPaging** właściwości **true**. Przez ustawienie właściwości obiektów zawartych w PagerStyle i właściwości PagerSettings, można dostosować wiersz pager. Zamiast korzystać z wiersza pagera wbudowanego interfejsu użytkownika, można utworzyć własnego interfejsu użytkownika przy użyciu **PagerTemplate** właściwości.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Można dostosować wygląd formantu FormView przez ustawienie właściwości stylu dla różnych części kontrolki. W poniższej tabeli wymieniono właściwości innego stylu.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| EditRowStyle | Ustawienia stylu dla wiersza danych, gdy kontrolka FormView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu dla wiersza danych puste, wyświetlany w kontrolce FormView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wiersza stopce kontrolki FormView. |
| HeaderStyle | Ustawienia stylu wiersz nagłówka kontrolki FormView. |
| InsertRowStyle | Ustawienia stylu dla wiersza danych, gdy kontrolka FormView jest w trybie wstawiania. |
| PagerStyle | Ustawienia stylu dla wierszy pagera, wyświetlany w kontrolce FormView po włączeniu funkcji stronicowania. |
| RowStyle | Ustawienia stylu dla wiersza danych, gdy kontrolka FormView jest w trybie tylko do odczytu. |

## <a name="events"></a>Zdarzenia

Formant widoku FormView zapewnia kilka zdarzeń, które można programować względem. Dzięki temu można uruchomić procedury niestandardowe zawsze wtedy, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia obsługiwane przez kontrolkę FormView.

| **Event** | **Opis** |
| --- | --- |
| ItemCommand | Występuje, gdy kliknięto przycisk w kontrolce FormView. To zdarzenie jest często używane do wykonywania zadań, po kliknięciu przycisku w formancie. |
| ItemCreated | Występuje po wszystkich obiektów FormViewRow są tworzone w kontrolce FormView. To zdarzenie jest często używane do modyfikowania wartości rekordu, przed wyświetleniem. |
| ItemDeleted | Występuje, gdy przycisk Usuń (przycisk z jego **CommandName** właściwość ustawioną na "Delete") po kliknięciu, ale po kontroli FormView usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby sprawdzić wyniki operacji usuwania. |
| ItemDeleting | Występuje, gdy kliknięto przycisk Usuń, ale przed FormView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| ItemInserted | Występuje, gdy przycisk Wstaw (przycisk z jego **CommandName** właściwość ustawioną na "Insert") po kliknięciu, ale po kontroli FormView wstawia rekordu. To zdarzenie jest często używane, aby sprawdzić wyniki operacji wstawiania. |
| ItemInserting | Występuje, gdy kliknięto przycisk Wstaw, ale przed FormView kontroli wstawia rekordu. To zdarzenie jest często używane do anulowania operacji wstawiania. |
| ItemUpdated | Występuje, gdy przycisk Aktualizuj (przycisk z jego **CommandName** właściwość ustawioną na "Aktualizuj") po kliknięciu, ale po kontroli FormView aktualizacji wiersza. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje, gdy kliknięto przycisk aktualizacji, ale przed FormView kontroli aktualizuje rekord. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje po tryby zmienia się kontrolka FormView (w celu edycji, wstawiania lub w trybie tylko do odczytu). To zdarzenie jest często używane do wykonywania zadań w trybach zmianach kontroli FormView. |
| ModeChanging | Występuje przed tryby zmienia się kontrolka FormView (w celu edycji, wstawiania lub w trybie tylko do odczytu). To zdarzenie jest często używane do anulowania zmiany trybu. |
| PageIndexChanged | Występuje po kliknięciu jednego z przyciski pagera, ale po kontroli FormView obsługuje stronicowanie. To zdarzenie jest często używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innego rekordu w kontrolce. |
| PageIndexChanging | Występuje po kliknięciu jednego z przyciski pagera, ale przed FormView kontroli obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="detailsview"></a>DetailsView

W kontrolce DetailsView służy do wyświetlania pojedynczy rekord ze źródła danych w tabeli, gdzie każde pole rekordu jest wyświetlany w wierszu tabeli. Może służyć w połączeniu z kontrolki GridView wzorzec / szczegół scenariuszach. W kontrolce DetailsView obsługuje następujące funkcje:

- Powiązanie z danymi kontroli źródła, takich jak SqlDataSource.
- Wbudowane możliwości Wstawianie.
- Wbudowane aktualizowanie i usuwanie możliwości.
- Wbudowane możliwości stronicowania.
- Programowy dostęp do modelu obiektów DetailsView, aby dynamicznie ustawić właściwości, obsługa zdarzeń i tak dalej.
- Można dostosować wygląd za pośrednictwem kompozycje i style.

## <a name="row-fields"></a>Pola wierszy

Każdy wiersz danych w kontrolce DetailsView jest tworzony przez zadeklarowanie formantu pola. Typy pól innego wiersza określają zachowanie wierszy w formancie. Formanty pól pochodzi od DataControlField. W poniższej tabeli wymieniono typy pól innego wiersza, których można użyć.

| **Typ pola kolumn** | **Opis** |
| --- | --- |
| Elementu BoundField | Wyświetla wartość pola w źródle danych jako tekst. |
| ButtonField | Wyświetla przycisk polecenia w kontrolce DetailsView. Dzięki temu można pobierać za pomocą kontrolki przycisku niestandardowe, takie jak dodawanie lub przycisk Usuń. |
| CheckBoxField | Wyświetla pole wyboru w kontrolce DetailsView. Ten typ pola wiersza najczęściej jest używana do wyświetlania pól z wartością logiczną. |
| CommandField | Wyświetla wbudowanego polecenia przycisków, aby wykonać edycji, wstawiania lub usuwania operacji w kontrolce DetailsView. |
| Pole hiperłącza HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperłącze. Ten typ pola wiersza, pozwala powiązać drugie pole adresu URL hiperłącza. |
| ImageField | Wyświetla obraz w kontrolce DetailsView. |
| TemplateField | Wyświetla zawartość zdefiniowanych przez użytkownika na podstawie wiersza w kontrolce DetailsView zgodnie z określonym szablonem. Ten typ wiersza, pole umożliwia tworzenie pola niestandardowego wiersza. |

Domyślnie ustawiono właściwość AutoGenerateRows **true**, która automatycznie generuje obiekt pola powiązanego wiersza dla każdego pola, które można powiązać typu w źródle danych. Prawidłowe typy możliwej do wiązania to ciąg, daty i godziny, Decimal, identyfikator Guid i zestawów typów podstawowych. Każde pole jest wyświetlony w wierszu jako tekst w kolejności, w którym każde pole pojawia się w źródle danych.

Automatyczne generowanie wiersze zapewnia szybki i łatwy sposób wyświetlania każdego pola w rekordzie. Aby użyć DetailsView kontrolki zaawansowane możliwości musi jawnie deklarować polami wierszy do uwzględnienia w kontrolce DetailsView. Aby zadeklarować polami wierszy, najpierw ustawić **AutoGenerateRows** właściwości **false**. Następnie dodaj otwierające i zamykające **&lt;pola&gt;** tagi między otwierającym i zamykającym tagiem kontrolce DetailsView. Na koniec listy pól wierszy, które chcesz uwzględnić między otwierającym i zamykającym **&lt;pola&gt;** tagów. Pola wierszy określone są dodawane do kolekcji pól w podanej kolejności. **Pola** kolekcji umożliwia programowe zarządzanie polami wierszy w kontrolce DetailsView.

> [!NOTE]
> Automatycznie generowane przez Ciebie pola nie są dodawane do kolekcji pól wiersz.


## <a name="binding-to-data"></a>Wiązanie z danymi

W kontrolce DetailsView może być powiązana do kontroli źródła danych, takich jak **SqlDataSource** lub AccessDataSource, lub do żadnego źródła danych, który implementuje interfejs System.Collections.IEnumerable, takich jak System.Data.DataView, System.Collections.ArrayList i System.Collections.Hashtable.

Użyj jednej z następujących metod, aby powiązać kontrolce DetailsView typ źródła danych:

- Aby powiązać formant źródła danych, ustaw właściwość DataSourceID w kontrolce DetailsView wartości Identyfikatora do kontroli źródła danych. W kontrolce DetailsView automatycznie wiąże określone dane do kontroli źródła. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje **System.Collections.IEnumerable** interfejsu, programowo ustawić właściwości źródła danych w kontrolce DetailsView ze źródłem danych, a następnie wywołać metodę powiązań danych.

## <a name="security"></a>Zabezpieczenia

Ten formant może służyć do wyświetlania danych wejściowych użytkownika, która może obejmować skrypt po stronie klienta złośliwe. Sprawdź wszystkie informacje, które są wysyłane przez klienta dla pliku wykonywalnego skrypt, instrukcji SQL lub inny kod przed wyświetleniem go w aplikacji. Program ASP.NET zapewnia funkcję weryfikacji danych wejściowych żądania bloku skryptu i HTML w danych wejściowych użytkownika.

## <a name="data-operations"></a>Operacje na danych

W kontrolce DetailsView zapewnia wbudowane możliwości, które pozwalają użytkownikowi na aktualizowanie, usuwanie, Wstaw i strony za pomocą elementów w formancie. Gdy kontrolce DetailsView jest powiązana z kontroli źródła danych, kontrolce DetailsView można korzystać z zalet źródła danych funkcje formantu i zapewnia automatyczne aktualizowanie, usuwanie, wstawianie i stronicowanie funkcji.

W kontrolce DetailsView można zapewnić obsługę update, delete, insert i operacje stronicowania przy użyciu innych typów źródeł danych; jednak należy podać implementacja dla tych operacji w obsłudze zdarzeń odpowiednie.

W kontrolce DetailsView może automatycznie dodać **CommandField** pola wiersza, edytowania, usuwania lub nowy przycisk przez ustawienie właściwości AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateInsertButton do **true**odpowiednio. W przeciwieństwie do usunięcia przycisku (które powoduje usunięcie wybranego rekordu natychmiast), po kliknięciu przycisku edycji lub New DetailsView kontrola przechodzi do edycji lub Wstaw tryb, odpowiednio. W trybie edycji przycisk Edytuj jest zastępowany aktualizacji i przycisk Anuluj. Kontrolki wejściowe, które są odpowiednie dla typu danych pola (na przykład pola tekstowego lub formantu CheckBox) są wyświetlane wartość pola użytkownik może go zmodyfikować. Klikając przycisk Aktualizuj aktualizacji rekordu w źródle danych, a kliknięcie przycisku Anuluj porzuca wszystkie zmiany. Podobnie w trybie wstawiania nowego przycisku zostaje zastąpiona opcją wstawiania i przycisk Anuluj, a pusty kontrolki wejściowe są wyświetlane dla użytkownika o wprowadzenie wartości dla nowego rekordu.

W kontrolce DetailsView udostępnia funkcję stronicowanie, dzięki czemu można przejść do innych rekordów w źródle danych. Po włączeniu formanty nawigacji na stronie są wyświetlane w wierszu pager. Aby włączyć stronicowania, ustaw właściwość właściwość AllowPaging na **true**. Za pomocą właściwości PagerStyle i PagerSettings można dostosować wiersz pager.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Można dostosować wygląd kontrolce DetailsView przez ustawienie właściwości stylu dla różnych części kontrolki. W poniższej tabeli wymieniono właściwości innego stylu.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu przemiennych wierszy danych w kontrolce DetailsView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane przemian ustawienia RowStyle i **AlternatingRowStyle** ustawienia. |
| CommandRowStyle | Ustawienia stylu wiersza zawierającego wbudowanego polecenia przycisków w kontrolce DetailsView. |
| EditRowStyle | Ustawienia stylu dla wierszy danych, gdy kontrolce DetailsView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu dla wiersza danych puste, wyświetlany w kontrolce DetailsView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wiersza stopki w kontrolce DetailsView. |
| HeaderStyle | Ustawienia stylu dla wiersza nagłówka w kontrolce DetailsView. |
| InsertRowStyle | Ustawienia stylu dla wierszy danych, gdy znajduje się w kontrolce DetailsView trybu wstawiania. |
| PagerStyle | Ustawienia stylu dla wiersza pagera w kontrolce DetailsView. |
| RowStyle | Ustawienia stylu dla wierszy danych w kontrolce DetailsView. Gdy **AlternatingRowStyle** właściwość jest również ustawiona, wiersze danych są wyświetlane na przemian **RowStyle** ustawienia i **AlternatingRowStyle** ustawienia. |
| FieldHeaderStyle | Ustawienia stylu dla nagłówka kolumny w kontrolce DetailsView. |

## <a name="events"></a>Zdarzenia

W kontrolce DetailsView udostępnia kilka zdarzeń, które można programować względem. Dzięki temu można uruchomić procedury niestandardowe zawsze wtedy, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia objęte kontrolce DetailsView. W kontrolce DetailsView dziedziczy także te zdarzenia z jej klas bazowych: powiązanie danych z danymi, Disposed, Init, obciążenia, PreRender i renderowania.

| **Event** | **Opis** |
| --- | --- |
| ItemCommand | Występuje po kliknięciu przycisku w kontrolce DetailsView. |
| ItemCreated | Występuje po wszystkich obiektów DetailsViewRow są tworzone w kontrolce DetailsView. To zdarzenie jest często używane do modyfikowania wartości rekordu, przed wyświetleniem. |
| ItemDeleted | Występuje, gdy kliknięto przycisk Usuń, ale po kontrolce DetailsView usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby sprawdzić wyniki operacji usuwania. |
| ItemDeleting | Występuje, gdy kliknięto przycisk Usuń, ale przed DetailsView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| ItemInserted | Występuje, gdy kliknięto przycisk Wstaw, ale po kontrolce DetailsView wstawia rekordu. To zdarzenie jest często używane, aby sprawdzić wyniki operacji wstawiania. |
| ItemInserting | Występuje, gdy kliknięto przycisk Wstaw, ale przed DetailsView kontroli wstawia rekordu. To zdarzenie jest często używane do anulowania operacji wstawiania. |
| ItemUpdated | Występuje, gdy kliknięto przycisk aktualizacji, ale po kontrolce DetailsView aktualizacji wiersza. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje, gdy kliknięto przycisk aktualizacji, ale przed DetailsView kontroli aktualizuje rekord. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje po kontrolce DetailsView zmian tryby (Edycja, Wstaw lub trybie tylko do odczytu). To zdarzenie jest często używane do wykonywania zadań w trybach zmianach w kontrolce DetailsView. |
| ModeChanging | Występuje, zanim zmieni się w kontrolce DetailsView tryby (Edycja, Wstaw lub trybie tylko do odczytu). To zdarzenie jest często używane do anulowania zmiany trybu. |
| PageIndexChanged | Występuje po kliknięciu jednego z przyciski pagera, ale po kontrolce DetailsView obsługuje stronicowanie. To zdarzenie jest często używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innego rekordu w kontrolce. |
| PageIndexChanging | Występuje po kliknięciu jednego z przyciski pagera, ale przed DetailsView kontroli obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="the-menu-control"></a>Formant Menu

Formant Menu w programie ASP.NET 2.0 została zaprojektowana jako system nawigacji w pełni funkcjonalne. Może być powiązana z danymi, łatwe do źródeł danych hierarchicznych, takich jak SiteMapDataSource.

Struktura kontrolki Menu mogą być definiowane w sposób deklaratywny lub dynamicznie i składa się z jednym węźle głównym i dowolną liczbę węzłów podrzędnych. Poniższy kod definiuje sposób deklaratywny menu kontrolki Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

W powyższym przykładzie węzeł Home.aspx jest węzeł główny. Wszystkie inne węzły są zagnieżdżone w obrębie węzła głównego na różnych poziomach.

Istnieją dwa typy menu, które można renderować formant Menu; menu statycznych i dynamicznych menu. Menu statyczne składają się z elementów menu, które są zawsze widoczne. Menu dynamiczne składają się z elementów menu, które są widoczne tylko wtedy, gdy użytkownik przesuwa się nad nimi za pomocą myszy. Klienci często może mylić statyczne menu z menu zdefiniowane w sposób deklaratywny i menu dynamiczne z menu, które są z danymi w czasie wykonywania. W rzeczywistości menu statycznych i dynamicznych są powiązane z metody populacji. Warunki *statyczne* i *dynamiczne* dotyczy tylko tego, czy menu jest statycznie wyświetlane domyślnie lub tylko wyświetlane, gdy użytkownik wykona akcję.

**Wartości StaticDisplayLevels** właściwość jest używana do konfigurowania, ile poziomów menu są statyczne i w związku z tym wyświetlane domyślnie. W powyższym przykładzie ustawienie **wartości StaticDisplayLevels** właściwości na wartość 2 spowodowałoby menu do wyświetlenia statycznie, węzeł głównej, węzeł utworów muzycznych i języka node filmów. Wszystkie inne węzły będzie dynamicznie wyświetlana po umieszczeniu wskaźnika węzła nadrzędnego.

**MaximumDynamicDisplayLevels** właściwość konfiguruje maksymalną liczbę poziomów dynamicznych jest umożliwiająca wyświetlanie menu. Wszelkie menu dynamiczne na poziomie wyższym niż wartość określoną przez **MaximumDynamicDisplayLevels** właściwości są odrzucane.

> [!NOTE]
> Jest prawie pewne, że mogą wystąpić sytuacje, gdy menu nie pojawiają się do renderowania ze względu na właściwość MaximumDynamicDisplayLevels. W takich przypadkach upewnij się, że dla właściwości ustawiono wystarczająco, aby umożliwić wyświetlanie menu klientów.


## <a name="data-binding-the-menu-control"></a>Formant Menu powiązania danych

Formant Menu można powiązać każde źródło danych hierarchicznych, takich jak SiteMapDataSource lub elementu XMLDataSource. SiteMapDataSource jest najczęściej używane metody dla powiązania danych z kontrolką Menu jego źródła zniżki w stosunku do pliku Web.sitemap, ponieważ jego schemat zawiera znany interfejs API, aby formant Menu. Lista poniżej zawiera prosty plik Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Należy zauważyć, że istnieje tylko jeden siteMapNode element główny, w tym przypadku element głównej. Dla każdego siteMapNode można skonfigurować kilka atrybutów. Najczęściej używane atrybuty są:

- **adres URL** Określa adres URL do wyświetlenia, gdy użytkownik kliknie element menu. Jeśli ten atrybut nie jest obecny, węzeł po prostu opublikuje ponownie, po kliknięciu.
- **Tytuł** Określa tekst, który jest wyświetlany na element menu.
- **Opis** używany jako dokumentacji dla węzła. Wyświetla również jako etykietka narzędzia gdy wskaźnik myszy jest aktywowany nad węzłem.
- **siteMapFile** umożliwia zagnieżdżonych mapy witryny dla wyszukiwarki. Ten atrybut musi wskazywać na poprawnie sformułowany plik mapy witryny ASP.NET.
- **role** umożliwia wygląd węzła kontrolowany przez dostosowanie do zabezpieczeń platformy ASP.NET.

Należy pamiętać, że te atrybuty są wszystkie opcjonalne, zachowania menu nie mogą być powinien, jeśli nie zostały określone. Na przykład jeśli *adresu url* atrybut jest określony, ale *opis* atrybut nie jest, węzeł nie będzie widoczny i nie będzie można przejść do adresu URL określony.

## <a name="controlling-a-menus-operation"></a>Kontrolowanie operacji menu

Istnieje kilka właściwości, które wpływają na działanie formant ASP.NET Menu; **orientacji** właściwości **DisappearAfter** właściwości **StaticItemFormatString** właściwości i **StaticPopoutImageUrl**właściwość to tylko niektóre z nich.

- **Orientacji** może być ustawiony na *poziomy* lub *pionowe* i określa, czy statycznych elementów menu jest rozmieszczony w poziomie w wierszu lub w pionie i skumulowane siebie nawzajem. Właściwość ta nie wpływa na menu dynamiczne.
- **DisappearAfter** właściwość konfiguruje się, jak długo menu dynamiczne pozostają widoczne po myszy przeniesiony poza. Wartość jest określona w milisekund, a wartość domyślna to 500. Ustawienie tej właściwości na wartość -1 spowoduje, że menu aby nigdy nie znika automatycznie. W takim przypadku menu tylko znikną, gdy użytkownik kliknie poza menu.
- **StaticItemFormatString** właściwości można łatwo utrzymać spójny sposób wyrażania w Twoim systemie menu. Po określeniu tej właściwości *{0}* powinny być wprowadzane zamiast opis, który pojawia się w źródle danych. Na przykład, aby mogła mieć element menu z say ćwiczenie 1 odwiedź nasze strony produktów, itp., możesz określić odwiedź nasze {0} strona StaticItemFormatString. W czasie wykonywania, ASP.NET zastąpi wszystkie wystąpienia {0} prawidłowy opis elementu menu.
- **StaticPopoutImageUrl** właściwość określa obraz, który jest używany do wskazania, że węzeł określonego menu ma węzły podrzędne, które mogą być udostępniane przez zatrzymanie wskaźnika myszy nad nim. Menu dynamiczne, będą w dalszym ciągu używać domyślnego obrazu.

## <a name="templated-menu-controls"></a>Formanty oparte na szablonach Menu

Formant Menu jest formantem z szablonem i pozwala na dwóch różnych elementów; StaticItemTemplate i DynamicItemTemplate. Za pomocą tych szablonów, można łatwo dodać formanty serwera lub kontrolki użytkownika do menu.

Aby edytować szablony w programie Visual Studio .NET, kliknij przycisk tagu inteligentnego w menu i wybrać opcję Edytuj szablony. Następnie można wybrać edycji StaticItemTemplate lub DynamicItemTemplate.

Wszystkie formanty dodane do StaticItemTemplate pojawi się w menu statyczne podczas ładowania strony. Wszystkie formanty dodane do DynamicItemTemplate pojawi się na wszystkich menu podręcznego.

## <a name="menu-events"></a>Zdarzenia menu

Formant Menu ma dwa zdarzenia, które są unikatowe **MenuItemClicked** i **MenuItemDatabound** zdarzeń.

MenuItemClicked zdarzenie jest wywoływane po kliknięciu elementu menu. Zdarzenie MenuItemDatabound jest wywoływane, gdy element menu jest powiązana z danymi. **MenuEventArgs** przekazana na zdarzenie obsługi zapewnia dostęp do elementów menu za pomocą właściwości elementu.

## <a name="controlling-a-menus-appearance"></a>Kontrolowanie wyglądu menu

Może również wpływać na wygląd kontrolki menu przy użyciu co najmniej jedną z wielu stylów dostępne do menu format. Wśród nich znalazły się **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**i **DynamicHoverStyle**. Te właściwości są skonfigurowane przy użyciu standardowego ciągu stylu w kodzie HTML. Na przykład następujące wpłynie na styl menu dynamiczne.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Jeśli używasz styl po wskazaniu wskaźnikiem, należy dodać &lt;head&gt; element na stronę z *runat* element ustawiony na wartość *serwera*.


Formanty menu obsługują także korzystanie z motywów programu ASP.NET 2.0.

## <a name="the-treeview-control"></a>TreeView — kontrolka

TreeView — kontrolka wyświetla dane w strukturze drzewa. Podobnie jak w przypadku kontrolki Menu, można go łatwo dane powiązane z dowolnego źródła danych hierarchicznych, takich jak SiteMapDataSource.

Pierwsze pytanie, na który klienci prawdopodobnie może poprosić o kontrolce widoku drzewa w programie ASP.NET 2.0 jest, czy jest powiązany z widoku drzewa WebControl programu Internet Explorer, która była dostępna dla platformy ASP.NET 1.x. Nie jest dostępne. Kontrolki ASP.NET 2.0 TreeView została napisana od podstaw i zapewnia istotnej poprawy w porównaniu TreeView WebControl programu Internet Explorer, która wcześniej była dostępna.

Nie będzie można przejść do szczegółów jak powiązać kontrolki TreeView do mapy witryny sieci Web, ponieważ jest wykonywana w taki sam sposób, jak formant Menu. TreeView — kontrolka ma jednak pewne różnice distinct w taki sposób, że działa.

Domyślnie kontrolki TreeView pojawia się w pełni rozwinięte. Aby zmienić poziom rozwijania podczas ładowania początkowego, zmodyfikuj **ExpandDepth** właściwości formantu. Jest to szczególnie ważne w przypadkach, gdy jest powiązana z danymi na rozwijanie określonych węzłów w widoku drzewa.

## <a name="databinding-the-treeview-control"></a>Powiązanie danych kontrolki TreeView

W odróżnieniu od formant Menu widoku drzewa pozwala również na obsługi dużych ilości danych. W związku z tym oprócz powiązanie danych z SiteMapDataSource lub elementu XMLDataSource, widoku drzewa jest często dane powiązane z zestawu danych lub innych danych relacyjnych. W przypadkach, gdy kontrolka widoku drzewa jest powiązana z dużych ilości danych najlepiej powiązać tylko z danymi, które są faktycznie widoczne w formancie. Następnie możesz powiązać dane z dodatkowych danych, ponieważ zostaną rozwinięte węzły w widoku drzewa.

W takich przypadkach **PopulateOnDemand** właściwości widoku drzewa powinna być równa *true*. Następnie należy podać implementacja dla **TreeNodePopulate** metody.

## <a name="data-binding-without-postback"></a>Powiązanie bez odświeżania danych

Zwróć uwagę, czy po rozwinięciu węzła w poprzednim przykładzie po raz pierwszy, na stronie publikuje Wstecz i odświeża. Thats nie ma problemu, w tym przykładzie, ale można Wyobraź sobie, że może być w środowisku produkcyjnym z dużą ilością danych. Scenariusz lepiej byłoby jeden w którym widoku drzewa będzie nadal dynamicznie wypełnienia jego węzłów, ale bez wpis z powrotem do serwera.

Ustawiając **PopulateNodesFromClient** i **PopulateOnDemand** właściwości na wartość true, kontrolki ASP.NET TreeView wypełni dynamicznie węzłów bez wpis ponownie. Po rozwinięciu węzła nadrzędnego XMLHttp żądania z klienta i jest wyzwalane zdarzenie OnTreeNodePopulate. Serwer odpowiada przy Wyspy danych XML, który jest następnie używany do danych powiązania węzłów podrzędnych.

ASP.NET dynamicznie tworzy kod klienta, który implementuje tę funkcję. &lt;Skryptu&gt; znaczników, które zawierają skryptu są generowane, wskazując plik AXD. Na przykład lista poniżej zawiera skrypt łącza dla kodu skryptu, który generuje żądanie XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Jeśli przejść pliku AXD powyżej w przeglądarce i otwórz go, pojawi się kod, który implementuje żądania XMLHttp. Ta metoda uniemożliwia klientom modyfikowanie pliku skryptu.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Kontrolowanie operacji kontrolki TreeView

TreeView — kontrolka ma kilka właściwości, które wpływają na działanie kontrolki. Najbardziej typowe właściwości są **ShowCheckBoxes**, **ShowExpandCollapse**, i **ShowLines**.

**ShowCheckBoxes** właściwość ma wpływ na informację określającą, czy węzły wyświetlają pola wyboru podczas renderowania. Prawidłowe wartości dla tej właściwości to **Brak**, **głównego**, **nadrzędnego**, **liścia**, i **wszystkich**. Mają one wpływu na formancie TreeView w następujący sposób:

| **Wartość właściwości** | **Efekt** |
| --- | --- |
| Brak | Pola wyboru nie są wyświetlane na wszystkie węzły. To jest ustawienie domyślne. |
| Główny | Pole wyboru jest wyświetlana tylko dla węzła głównego. |
| Nadrzędny | Pole wyboru jest wyświetlana tylko w ramach tych węzłów, które mają węzłów podrzędnych. Węzły nadrzędne lub węzły liści, może być tych węzłów podrzędnych. |
| Typu liść | Pole wyboru jest wyświetlane tylko w ramach tych węzłów, które mają bez węzłów podrzędnych. |
| Wszystkie | Pole wyboru jest wyświetlane we wszystkich węzłach. |

Gdy używane są pola wyboru, **CheckedNodes** właściwość zwraca kolekcję węzłów w widoku drzewa, które są sprawdzane podczas odświeżania.

**ShowExpandCollapse** właściwość kontroluje wygląd obrazu Rozwiń/Zwiń obok węzłów głównych i element nadrzędny. Jeśli ta właściwość jest ustawiona **false**, węzły w widoku drzewa są renderowane jako hiperłącza i jest rozwinięty/zwinięty, klikając link.

**ShowLines** właściwość określa, czy wiersze są wyświetlane węzły nadrzędne nawiązywania połączenia z węzłami podrzędnymi. Gdy **false** (ustawienie domyślne), są wyświetlane żadne wiersze. Gdy **true**, kontrolki TreeView użyje wiersze obrazów w folderze określonym przez **LineImagesFolder** właściwości.

Dostosowywanie wyglądu wierszy w widoku drzewa, Visual Studio .NET w wersji 2005 zawiera narzędzie wiersza projektanta. Można uzyskać dostęp do tego narzędzia, za pomocą przycisku tagu inteligentnego w kontrolce TreeView jako poniżej.


![](data-bound-controls/_static/image1.jpg)

**Rysunek 1.**


Po wybraniu pozycji **dostosować obrazy liniowe** opcji menu, narzędzie Projektant wiersza zostanie uruchomiony, umożliwiając konfigurowanie wyglądu wierszy w widoku drzewa.

## <a name="treeview-events"></a>Zdarzenia w widoku drzewa

TreeView — kontrolka ma unikatowy następujące zdarzenia:

- Na podstawie SelectedNodeChanged występuje po wybraniu węzła **SelectAction** właściwości.
- TreeNodeCheckChanged występuje, gdy stan checkboxs węzłów jest zmieniany.
- Na podstawie TreeNodeExpanded występuje, gdy węzeł jest rozwinięty **SelectAction** właściwości.
- TreeNodeCollapsed występuje, gdy węzeł jest zwinięta.
- TreeNodeDataBound występuje, gdy odpowiedni węzeł stanie danych powiązane.
- TreeNodePopulate występuje, gdy węzeł zostanie wypełniony.

**SelectAction** Właściwość pozwala na skonfigurowanie, które jest wywoływane po wybraniu węzła. Właściwość SelectAction zawiera następujące akcje:

- TreeNodeSelectAction.Expand zgłasza TreeNodeExpanded po wybraniu węzła.
- TreeNodeSelectAction.None zgłasza żadne zdarzenie po wybraniu węzła.
- TreeNodeSelectAction.Select wywołuje zdarzenie SelectedNodeChanged po wybraniu węzła.
- TreeNodeSelectAction.SelectExpand zgłasza zdarzenie SelectedNodeChanged i zdarzenia TreeNodeExpanded po wybraniu węzła.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu ze stylami

TreeView — kontrolka zawiera wiele właściwości do kontrolowania wyglądu formantu przy użyciu stylów. Dostępne są następujące właściwości.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| HoverNodeStyle | Określa styl węzłów, gdy wskaźnik myszy jest aktywowany nad nimi. |
| LeafNodeStyle | Określa styl węzłów liścia. |
| NodeStyle | Określa styl dla wszystkich węzłów. Style określonego węzła (np. LeafNodeStyle) musi zostać zastąpiona tego stylu. |
| ParentNodeStyle | Określa styl dla wszystkich węzłów nadrzędnych. |
| RootNodeStyle | Określa styl dla węzła głównego. |
| SelectedNodeStyle | Określa styl dla wybranego węzła. |

Każda z tych właściwości jest tylko do odczytu. Jednak będą oni mogli każdy zwracany **TreeNodeStyle** obiektów i właściwości tego obiektu można modyfikować za pomocą *wykorzystanie właściwości* formatu. Na przykład, aby ustawić **ForeColor** właściwość **SelectedNodeStyle**, należy użyć następującej składni:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Należy zauważyć, że nie zamknięto tagu powyżej. To, ponieważ gdy za pomocą składni deklaratywnej pokazano poniżej, można dołączyć węzły TreeViews w kodzie HTML, jak również.

Można również określić ponownego obliczenia właściwości stylu kodu przy użyciu *property.subproperty* formatu. Na przykład, aby ustawić **ForeColor** właściwość **RootNodeStyle** w kodzie, można użyć następującej składni:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Aby uzyskać pełną listę właściwości innego stylu zobacz dokumentację MSDN w obiekcie TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Kontrolki ścieżki mapy witryny

Kontrolki ścieżki mapy witryny zawiera formant linki do stron nadrzędnych nawigacji dla deweloperów platformy ASP.NET. Podobnie jak inne formanty nawigacji może to być łatwo dane powiązane z danymi hierarchicznymi źródeł, takich jak SiteMapDataSource lub elementu XmlDataSource.

Kontrolki ścieżki mapy witryny składa się z SiteMapNodeItem obiektów. Istnieją trzy typy węzłów węzeł główny i węzły nadrzędne, bieżącego węzła. Węzeł główny jest węzłem w górnej części hierarchicznej struktury. Bieżący węzeł reprezentuje bieżącą stronę. Wszystkie inne węzły są węzły nadrzędne.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Kontrolowanie operacji kontrolki ścieżki mapy witryny

Właściwości służące do sterowania działaniem kontrolki ścieżki mapy witryny są następujące:

| **Property** | **Opis właściwości** |
| --- | --- |
| ParentLevelsDisplayed | Określa, ile węzły nadrzędne są wyświetlane. Wartość domyślna to -1, która nakłada żadnych ograniczeń liczby węzłów nadrzędnych wyświetlane. |
| PathDirection | Określa kierunek ścieżki mapy witryny. Prawidłowe wartości to RootToCurrent (ustawienie domyślne) i CurrentToRoot. |
| PathSeparator | Ciąg, który określa znak oddzielający węzłów kontrolki ścieżki mapy witryny. Wartość domyślna to:. |
| RenderCurrentNodeAsLink | Wartość logiczna, która kontroluje, czy bieżący węzeł jest renderowany jako link. Wartość domyślna to False. |
| SkipLinkText | Ułatwia utrzymanie ułatwienia dostępu podczas wyświetlania strony przez czytniki zawartości ekranu. Ta właściwość umożliwia czytniki zawartości ekranu pominąć kontrolki ścieżki mapy witryny. Aby wyłączyć tę funkcję, należy ustawić właściwość do String.Empty. |

## <a name="templated-sitemappath-controls"></a>Formanty SiteMapPath oparte na szablonach

SiteMapControl jest formantem z szablonem, a w efekcie można zdefiniować różne szablony do użycia w trakcie wyświetlania kontrolki. Aby edytować szablony w kontrolki ścieżki mapy witryny, kliknij przycisk tagu inteligentnego w kontrolce menu i wybierz opcję Edytuj szablony. Wyświetla SiteMapTasks menu, jak pokazano poniżej, gdzie można wybrać różne szablony dostępne.


![](data-bound-controls/_static/image2.jpg)

**Rysunek 2**


**NodeTemplate** szablon, który odwołuje się do dowolnego węzła w ścieżki mapy witryny. Jeśli węzeł jest węzłem głównym lub bieżącego węzła i a **RootNodeTemplate** lub **CurrentNodeTemplate** jest skonfigurowany, NodeTemplate zostanie zastąpiona.

## <a name="sitemappath-events"></a>Zdarzenia ścieżki mapy witryny

Kontrolki ścieżki mapy witryny ma dwa zdarzenia, które nie pochodzą z klasy kontrolek; **ItemCreated** zdarzeń i **ItemDataBound** zdarzeń. Zdarzenie ItemCreated jest wywoływane, gdy tworzony jest element ścieżki mapy witryny. ItemDataBound jest wywoływane, gdy wywoływana jest metoda powiązań danych, podczas tworzenia powiązań danych węzła ścieżki mapy witryny. A **SiteMapNodeItemEventArgs** obiekt umożliwia dostęp do określonych SiteMapNodeItem za pomocą właściwości elementu.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu ze stylami

Następujące style są dostępne dla formatowanie kontrolki ścieżki mapy witryny.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| CurrentNodeStyle | Określa styl tekstu dla bieżącego węzła. |
| RootNodeStyle | Określa styl tekstu dla węzła głównego. |
| NodeStyle | Określa styl tekstu dla wszystkich węzłów, przy założeniu, że CurrentNodeStyle lub RootNodeStyle nie ma zastosowania. |

Właściwość NodeStyle zostanie zastąpiona przez CurrentNodeStyle lub RootNodeStyle. Każda z tych właściwości jest tylko do odczytu i zwraca **styl** obiektu. Wpływ na wygląd węzła przy użyciu jednej z tych właściwości, należy ustawić właściwości obiektu styl, który jest zwracany. Na przykład poniższy kod zmienia forecolor właściwość bieżącego węzła.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Właściwości mogą być stosowane również programowo w następujący sposób:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Jeśli szablon jest stosowany, nie można zastosować styl.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratorium 1: Konfigurowanie formant Menu platformy ASP.NET

1. Utwórz nową witrynę sieci Web.
2. Dodaj plik mapy witryny, wybierając plik, nowy, plików i pozycję mapy witryny z listy szablonów pliku.
3. Otwórz mapy witryny (Web.sitemap domyślnie) i zmodyfikuj go tak, aby wygląda jak poniżej listy. Strony, do których prowadzi Link w pliku mapy witryny tak naprawdę nie istnieją, ale który nie będzie to problem dla tego ćwiczenia.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Otwórz domyślnego formularza sieci Web w widoku Projekt.
5. W sekcji nawigacji w przyborniku należy dodać nowy formant Menu do strony.
6. W sekcji danych przybornika dodać SiteMapDataSource nowe. SiteMapDataSource będzie automatycznie używać pliku Web.sitemap w Twojej lokacji. (Plik Web.sitemap *musi* znajdować się w folderze głównym lokacji.)
7. Kliknij formant Menu, a następnie kliknij przycisk tagu inteligentnego, aby wyświetlić okno dialogowe Menu zadania.
8. Na liście rozwijanej wybierz źródło danych wybierz SiteMapDataSource1.
9. Kliknij łącze AutoFormat, a następnie wybierz format menu.
10. W okienku właściwości ustaw **wartości StaticDisplayLevels** właściwość 2. Formant Menu powinien być teraz ustawiony węzeł głównej, produktów i usług w projektancie.
11. Przeglądaj stronę w przeglądarce, aby skorzystać z menu. (Ponieważ strony był dodane do mapy witryny faktycznie nie istnieją, zobaczysz błąd podczas próby przeglądania ich.)

Eksperymentować, zmieniając wartości StaticDisplayLevels i właściwości MaximumDynamicDisplayLevels i zobacz ich wpływ na sposób renderowania menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratorium 2: Dynamiczne powiązanie kontrolki TreeView

To ćwiczenie przyjęto założenie, że masz serwer SQL działa lokalnie i czy bazy danych Northwind jest dostępny w wystąpieniu programu SQL Server. Jeśli te warunki nie są spełnione, Zmień parametry połączenia w próbce. Należy pamiętać o tym, czy też konieczne może być określ uwierzytelnianie programu SQL Server zamiast zaufanego połączenia.

1. Utwórz nową witrynę sieci Web.
2. Przełącz do widoku kodu na Default.aspx i Zastąp cały kod kod przedstawiony poniżej. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Zapisz stronę jako treeview.aspx.
4. Przejdź na stronę.
5. Gdy ta strona jest wyświetlana po raz pierwszy, Wyświetl źródło strony w przeglądarce. Należy zauważyć, że tylko widoczne węzły zostały wysłane do klienta.
6. Kliknij znak plus obok każdego węzła.
7. Wyświetl źródło na stronie ponownie. Należy zauważyć, że nowo wyświetlanego węzłów występują teraz.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratorium 3: Szczegółowe informacje, wyświetlanie i edytowanie danych przy użyciu GridView i DetailsView

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik web.config w witrynie sieci Web.
3. Dodaj parametry połączenia do pliku web.config, jak pokazano poniżej: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Może być konieczna zmiana parametrów połączenia, w zależności od środowiska.
4. Zapisz i zamknij plik web.config.
5. Otwórz Default.aspx i Dodaj nową kontrolkę kontrolką SqlDataSource.
6. Zmień identyfikator kontrolki SqlDataSource **produktów**.
7. W **zadania SqlDataSource** menu, kliknij przycisk **skonfigurować źródło danych**.
8. Wybierz **Northwind** na liście rozwijanej połączeń i kliknij przycisk Dalej.
9. Wybierz **produktów** z **nazwa** listy rozwijanej i sprawdź **ProductID**, **ProductName**, **UnitPrice**, i **UnitsInStock** pola wyboru, jak pokazano poniżej. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Kliknij przycisk **Dalej**.
11. Kliknij przycisk **Zakończ**.
12. Przełącz na widok źródła, a następnie sprawdzić kod, który został wygenerowany. Zwróć uwagę **SelectCommand**, **elementu DeleteCommand**, **element InsertCommand**, i **elementu UpdateCommand** do SqlDataSource dodano formant. Ponadto parametry, które zostały dodane.
13. Przełącz do widoku projektu i dodać nowe kontrolki widoku siatki do strony.
14. Wybierz **produktów** z **wybierz źródło danych** listy rozwijanej.
15. Sprawdź **Włączanie stronicowania** i **Włącz zaznaczanie** jak pokazano poniżej. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Kliknij przycisk **Edytowanie kolumn** link i upewnij się, że **automatyczne generowanie pól** jest zaznaczone.
17. Kliknij przycisk **OK**.
18. Za pomocą kontrolki GridView zaznaczone, kliknij przycisk **DataKeyNames** właściwości w okienku właściwości.
19. Wybierz **ProductID** z **dostępne pola danych** listy, a następnie kliknij przycisk **&gt;** przycisk, aby ją dodać.
20. Kliknij przycisk OK.
21. Dodaj nowe kontrolki SqlDataSource ze stroną.
22. Zmień identyfikator kontrolki SqlDataSource **szczegóły**.
23. Wybierz z menu zadania SqlDataSource **skonfigurować źródło danych**.
24. Wybierz **Northwind** z listy rozwijanej, a następnie kliknij przycisk **dalej**.
25. Wybierz <strong>produktów</strong> z <strong>nazwa</strong> listy rozwijanej i sprawdź <strong> \</ strong > * pola wyboru w <strong>kolumn</strong> listbox.
26. Kliknij przycisk **gdzie** przycisku.
27. Wybierz **ProductID** z **kolumny** listy rozwijanej.
28. Wybierz **=** na liście rozwijanej operatora.
29. Wybierz **kontroli** z **źródła** listy rozwijanej.
30. Wybierz **GridView1** z **identyfikator formantu** listy rozwijanej.
31. Kliknij przycisk **Dodaj** przycisk, aby dodać klauzulę WHERE.
32. Kliknij przycisk **OK**.
33. Kliknij przycisk **zaawansowane** znajdujący się i sprawdź **instrukcje generowania INSERT, UPDATE i DELETE** pola wyboru.
34. Kliknij przycisk **OK**.
35. Kliknij przycisk **dalej** i kliknij przycisk **Zakończ**.
36. Na stronie, Dodaj kontrolce DetailsView.
37. W **wybierz źródło danych** listy rozwijanej wybierz **szczegóły**.
38. Sprawdź **Włącz edytowanie** pole wyboru, jak pokazano poniżej. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Zapisanie strony i Przeglądaj Default.aspx.
40. Kliknij przycisk **wybierz** łącze obok różnych rekordów w celu wyświetlenia aktualizacji DetailsView automatycznie.
41. Kliknij przycisk **Edytuj** łącze w kontrolce DetailsView.
42. Wprowadź zmiany w rekordzie, a następnie kliknij przycisk **aktualizacji**.
