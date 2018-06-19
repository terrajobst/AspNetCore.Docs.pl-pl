---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Członkostwo i administrowanie | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 166bc642ea2083f455be0648e424f0b0ae3b082c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "30889634"
---
<a name="membership-and-administration"></a>Członkostwo i Administracja
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


Ten samouczek pokazuje, jak zaktualizować Wingtip Toys przykładowej aplikacji, aby dodać niestandardową rolę i korzystania z tożsamości ASP.NET. On również przedstawiono sposób wykonania strony Administracja, z którego użytkownik z niestandardowej roli zabezpieczeń można dodawać i usuwać produkty z witryny sieci Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) systemu członkostwa, używany do tworzenia aplikacji sieci web ASP.NET i jest dostępny w programie ASP.NET 4.5. ASP.NET Identity jest używany w szablon projektu Visual Studio 2013 Web Forms, jak również szablonów dla [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), i [ASP.NET jednej strony aplikacji](../../../../single-page-application/index.md). Można również specjalnie instalacji systemu ASP.NET Identity przy użyciu narzędzia NuGet po uruchomieniu z pustą aplikację sieci Web. Jednak w tym samouczku użyto **formularzy sieci Web**projecttemplate, w tym systemie tożsamości ASP.NET. ASP.NET Identity ułatwia integrowanie danych profilu dla użytkownika z danych aplikacji. ASP.NET Identity można też, wybierz model trwałości profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych programu SQL Server lub innym magazynie danych, w tym *NoSQL* magazynów danych, takich jak Windows Azure magazynu tabel.

W tym samouczku opiera się na poprzedniej Samouczek zatytułowany "Wyewidencjonowania i płatności z PayPal" serii samouczek Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać niestandardowej roli zabezpieczeń i użytkownika do aplikacji za pomocą kodu.
- Jak ograniczyć dostęp do folderu administracji i strony.
- Jak zapewnić nawigacji dla użytkownika, który należy do roli niestandardowych.
- Jak używać wiązania modelu do wypełnienia [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) kontrolki z kategorii produktów.
- Jak można przekazać pliku do aplikacji sieci web przy użyciu [przekazywaniem plików](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) formantu.
- Jak używać formanty walidacji do implementacji sprawdzania poprawności danych wejściowych.
- Jak dodać i usunąć produkty z aplikacji.

## <a name="these-features-are-included-in-the-tutorial"></a>Te funkcje dostępne w samouczku:

- ASP.NET Identity
- Konfiguracja i autoryzacji
- Wiązanie modelu
- Sprawdzania poprawności dyskretnego kodu

Formularze sieci Web programu ASP.NET zapewnia funkcje członkostwa. Przy użyciu domyślnego szablonu, masz funkcje wbudowane członkostwa, pomocne od razu po uruchomieniu aplikacji. W tym samouczku przedstawiono sposób korzystania z tożsamości ASP.NET do dodawania niestandardowej roli zabezpieczeń i przypisać do tej roli użytkownika. Dowiesz się jak ograniczyć dostęp do folderu administracji. Dodasz stronę do folderu administracji, który pozwala użytkownikowi z niestandardowej roli zabezpieczeń do dodawania i usuwania produktów i Podgląd produktu po został dodany.

## <a name="adding-a-custom-role"></a>Dodawanie niestandardowej roli zabezpieczeń

Przy użyciu tożsamości platformy ASP.NET, można dodawać niestandardowej roli zabezpieczeń i przypisać do tej roli przy użyciu kodu użytkownika.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *logiki* folderu i Utwórz nową klasę.
2. Nazwa nowej klasy *RoleActions.cs*.
3. Zmodyfikuj kod, aby był on wyświetlany w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. W **Eksploratora rozwiązań**, otwórz *Global.asax.cs* pliku.
5. Modyfikowanie *Global.asax.cs* pliku przez dodanie kodu wyróżnione kolorem żółtym, aby był on wyświetlany w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Zwróć uwagę, że `AddUserAndRole` jest podkreślone na czerwono. Kliknij dwukrotnie AddUserAndRole kodu.  
   Litery "A" na początku metody wyróżnione zostanie podkreślone.
7. Umieść kursor nad litery "A", a następnie kliknij przycisk interfejsu użytkownika, który służy do generowania szkieletu metody dla `AddUserAndRole` metody. 

    ![Członkostwo i Advministration - wygenerować klasy zastępczej — metoda](membership-and-administration/_static/image1.png)
8. Kliknij opcję o nazwie:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Otwórz *RoleActions.cs* plik z *logiki* folderu.  
   `AddUserAndRole` Metody został dodany do pliku klasy.
10. Modyfikowanie *RoleActions.cs* pliku przez usunięcie `NotImplementedeException` i dodawanie kodu wyróżnione kolorem żółtym, aby był on wyświetlany w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Powyższy kod ustanawia najpierw kontekst bazy danych dla bazy danych członkostwa. Bazy danych członkostwa także jest przechowywany jako *.mdf* w pliku *aplikacji\_danych* folderu. Będzie można wyświetlić tej bazy danych, gdy pierwszy użytkownik zalogował się do tej aplikacji sieci web. 

> [!NOTE] 
> 
> Jeśli chcesz przechowywać dane członkostwa wraz z danymi produktu, można rozważyć korzystającej z tego samego **DbContext** umożliwia przechowywanie danych produktu w powyższym kodzie.


 *Wewnętrzny* — słowo kluczowe jest modyfikator dostępu dla typów (takich jak klasy) i elementy członkowskie typu (np. metody lub właściwości). Wewnętrzne typy i elementy członkowskie są dostępne tylko w obrębie plików znajdujących się w tym samym zestawie *(.dll* pliku). Podczas budowania aplikacji, plik zestawu *(.dll*) jest tworzony zawiera kod, który jest wykonywany po uruchomieniu aplikacji. 

A `RoleStore` obiektu, który umożliwia zarządzanie roli, jest tworzony w zależności od kontekstu bazy danych.

> [!NOTE] 
> 
> Zwróć uwagę, że w przypadku `RoleStore` obiekt jest tworzony używa rodzajowego `IdentityRole` typu. Oznacza to, że `RoleStore` jest dozwolona tylko w celu uwzględnienia `IdentityRole` obiektów. Również przy użyciu ogólne, zasoby pamięci są obsługiwane lepiej.


Następnie `RoleManager` obiektów, zostanie utworzony na podstawie `RoleStore` obiektu, który został właśnie utworzony. `RoleManager` obiekt ujawnia powiązany z rolą interfejs API, który może służyć do automatycznie zapisuje zmiany `RoleStore`. `RoleManager` Jest dozwolona tylko w celu uwzględnienia `IdentityRole` obiektów, ponieważ w kodzie użyto `<IdentityRole>` typu ogólnego.

Należy wywołać `RoleExists` metodę, aby określić, czy rola "canEdit" znajduje się w bazie danych członkostwa. Jeśli nie, należy utworzyć rolę.

Tworzenie `UserManager` obiektu wydaje się być bardziej skomplikowane niż `RoleManager` kontrolować, jednak są prawie takie same. Po prostu zakodowanej w jednym wierszu, zamiast kilku. W tym miejscu parametr, który jest przekazywany jest utworzenie wystąpienia jako nowy obiekt zawarte w nawiasach.

Następnie należy utworzyć użytkownika "canEditUser", tworząc nowe `ApplicationUser` obiektu. Następnie jeśli pomyślnie utworzono użytkownika, należy dodać użytkownika do nowej roli.

> [!NOTE] 
> 
> Obsługa błędów będzie aktualizowany podczas samouczek "Obsługi błędu ASP.NET" w dalszej części tego samouczka serii.


Przy następnym uruchomieniu aplikacji użytkownika o nazwie "canEditUser" zostanie dodany jako rola o nazwie "canEdit" aplikacji. W dalszej części tego samouczka będzie zalogować się jako użytkownik "canEditUser", aby wyświetlić dodatkowe funkcje, które będą dodane w tym samouczku. Interfejs API szczegółowe informacje na temat tożsamości platformy ASP.NET, zobacz [Namespace Microsoft.AspNet.Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Aby uzyskać dodatkowe szczegóły dotyczące inicjowania systemu tożsamości platformy ASP.NET, zobacz [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Ograniczanie dostępu do strony Administracja

Przykładowa aplikacja Wingtip Toys umożliwia zarówno użytkowników anonimowych i zalogowanych użytkowników wyświetlić i zakupu produktów. Jednak zalogowanego użytkownika, który ma rolę niestandardowe "canEdit" można uzyskać dostępu do stronę z ograniczeniami w celu dodawania i usuwania produktów.

#### <a name="add-an-administration-folder-and-page"></a>Dodaj Folder administracji i strony

Następnie utworzy folder o nazwie *Admin* dla użytkownika "canEditUser" należącego do roli niestandardowych Wingtip Toys przykładowej aplikacji.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **Dodaj**  - &gt; **nowy Folder**.
2. Nazwa nowego folderu *Admin*.
3. Kliknij prawym przyciskiem myszy *Admin* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
4. Wybierz <strong>Visual C#</strong> - &gt; <strong>Web</strong> grupy szablonów po lewej stronie. Wybierz z listy środkowej <strong>formularza sieci Web ze stroną wzorcową</strong>, nadaj jej nazwę <em>AdminPage.aspx</em><strong>,</strong> , a następnie wybierz <strong>Dodaj</strong>.
5. Wybierz *Site.Master* pliku jako strony głównej, a następnie wybierz **OK**.

#### <a name="add-a-webconfig-file"></a>Dodanie pliku Web.config

Dodając *Web.config* pliku na *Admin* folder, można ograniczyć dostęp do strony zawartych w folderze.

1. Kliknij prawym przyciskiem myszy *Admin* i wybierz polecenie **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz z listy szablonów sieci web Visual C#, <strong>pliku konfiguracji sieci Web</strong>na liście środkowej Zaakceptuj domyślną nazwę <em>Web.config</em><strong>,</strong> , a następnie wybierz <strong>Dodaj</strong>.
3. Zastąp istniejące zawartości w pliku XML *Web.config* pliku następującym kodem:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Zapisz *Web.config* pliku. *Web.config* pliku Określa, że tylko użytkownik należący do roli "canEdit" aplikacji można uzyskać dostęp do strony zawarte w *Admin* folderu.

### <a name="including-custom-role-navigation"></a>W tym nawigacji niestandardowej roli zabezpieczeń

Aby umożliwić użytkownikowi roli niestandardowe "canEdit" Przejdź do sekcji Administracja aplikacji, należy dodać łącze do *Site.Master* strony. Użytkownicy, którzy należą do roli "canEdit" zostanie wyświetlony tylko **Admin** łącze i uzyskać dostęp do sekcji administracji.

1. W Eksploratorze rozwiązań, znajdowanie i otwieranie *Site.Master* strony.
2. Aby utworzyć link do użytkownika w roli "canEdit", Dodaj znaczników wyróżnione kolorem żółtym Poniższa lista Nieuporządkowana `<ul>` element, aby ta lista jest wyświetlana jako następuje:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Otwórz *Site.Master.cs* pliku. Wprowadź **Admin** łącze widoczne tylko dla użytkownika "canEditUser" przez dodanie kodu wyróżnione kolorem żółtym do `Page_Load` obsługi. `Page_Load` Obsługi będzie wyglądać następująco:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Podczas ładowania strony kodu sprawdza, czy zalogowany użytkownik ma rolę "canEdit". Jeśli użytkownik należy do roli "canEdit" element span z łączem do *AdminPage.aspx* strony (i w związku z tym łącze wewnątrz zakresu) stają się widoczne.

### <a name="enabling-product-administration"></a>Włączanie administracji produktu

Do tej pory utworzono rolę "canEdit" i dodać użytkownika "canEditUser", folder administracji i strony Administracja. Ustawiono uprawnienia dostępu dla folderu administracji i strony, a następnie dodano łącza nawigacji dla użytkownika w roli "canEdit" do aplikacji. Następnie należy dodać znaczników do *AdminPage.aspx* strony i kod, aby *AdminPage.aspx.cs* plik CodeBehind, który umożliwi użytkownikowi z rolą "canEdit" Dodawanie i usuwanie produktów.

1. W **Eksploratora rozwiązań**, otwórz *AdminPage.aspx* plik z *Admin* folderu.
2. Zastąp istniejący kod znaczników następujące czynności:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Następnie otwórz folder *AdminPage.aspx.cs* pliku CodeBehind, klikając prawym przyciskiem myszy *AdminPage.aspx* i klikając **kod widoku**.
4. Zastąp istniejący kod w *AdminPage.aspx.cs* pliku CodeBehind następującym kodem:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

W kodzie wprowadzone *AdminPage.aspx.cs* pliku CodeBehind, klasy o nazwie `AddProducts` wykonuje faktyczną pracę dodawania produktów w bazie danych. Ta klasa jeszcze nie istnieje, więc będzie można utworzyć go teraz.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **kod** grupy szablonów po lewej stronie. Następnie wybierz opcję **klasy**ze środka listy i nadaj mu nazwę *AddProducts.cs*.   
   Zostanie wyświetlony nowy plik klasy.
3. Zastąp istniejący kod poniżej:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* strony umożliwia użytkownikowi należących do roli "canEdit" Dodawanie i usuwanie produktów. Po dodaniu nowego produktu szczegółowe informacje o produkcie są weryfikowane i następnie wprowadzany do bazy danych. Nowy produkt jest natychmiast dostępna dla wszystkich użytkowników w aplikacji sieci web.

#### <a name="unobtrusive-validation"></a>Sprawdzania poprawności dyskretnego kodu

Szczegóły produktu, które użytkownik podaje na *AdminPage.aspx* strony są weryfikowane przy użyciu formanty walidacji (`RequiredFieldValidator` i `RegularExpressionValidator`). Formanty automatycznie używać sprawdzania poprawności dyskretnego kodu. Sprawdzania poprawności dyskretnego kodu umożliwia formanty walidacji logikę weryfikacji po stronie klienta, co oznacza, że strony nie wymaga podróży do serwera, który ma zostać zweryfikowana przy użyciu języka JavaScript. Domyślnie sprawdzania poprawności dyskretnego kodu jest uwzględniona w *Web.config* opartą na plikach na następujące ustawienia konfiguracji:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Wyrażenia regularne

Cena produktu na *AdminPage.aspx* strony jest weryfikowane przy użyciu **RegularExpressionValidator** formantu. Ten formant sprawdza, czy wartość skojarzona kontrolki wprowadzania (pole tekstowe "AddProductPrice") zgodny z wzorcem określona przez wyrażenie regularne. Wyrażenie regularne jest dopasowywanie do wzorca notacji, umożliwiającą szybkie znajdowanie i odpowiada wzorców określonych znaków. **RegularExpressionValidator** formant zawiera właściwość o nazwie `ValidationExpression` zawiera wyrażenie regularne służące do sprawdzania poprawności danych wejściowych cen, jak pokazano poniżej:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Formant przekazywaniem plików

Oprócz kontroli danych wejściowych i sprawdzania poprawności, możesz dodać **przekazywaniem plików** formant *AdminPage.aspx* strony. Ten formant oferuje możliwość przekazywania plików. W takim przypadku tylko umożliwi pliki obrazów do załadowania. W pliku CodeBehind (*AdminPage.aspx.cs*), gdy `AddProductButton` zostanie kliknięty kontroli kodu `HasFile` właściwość **przekazywaniem plików** formantu. Jeśli formant ma plik, jeśli typ pliku (na podstawie rozszerzenia pliku) jest dozwolone, jest zapisywany obraz do *obrazów* folderu i *obrazów/Kciuki* folderu aplikacji.

#### <a name="model-binding"></a>Wiązanie modelu

We wcześniejszej części tego samouczka serii wiązania modelu używany do wypełniania **ListView** kontroli, **FormsView** kontroli, **widoku GridView** kontroli, a  **DetailView** formantu. W tym samouczku użycie wiązania modelu do wypełniania **DropDownList** formantu o listę kategorii produktów.

Kod znaczników, który został dodany do *AdminPage.aspx* plik zawiera **DropDownList** formantu o nazwie `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Użyj wiązania modelu w celu wypełnienia to **DropDownList** przez ustawienie `ItemType` atrybutu i `SelectMethod` atrybutu. `ItemType` Atrybut określa, że używasz `WingtipToys.Models.Category` wpisz podczas wypełniania formantu. Definicja tego typu na początku tego samouczka serii tworząc `Category` klasy (pokazana poniżej). `Category` Klasa znajduje się w *modele* folder wewnątrz *Category.cs* pliku.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Atrybutu **DropDownList** kontroli określa, że używasz `GetCategories` — metoda (pokazana poniżej) który jest zawarty w pliku CodeBehind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Ta metoda określa, że `IQueryable` interfejs jest używany do oceny zapytania dotyczącego `Category` typu. Zwrócona wartość jest używana do wypełniania **DropDownList** w znaczniku strony (*AdminPage.aspx*).

Tekst wyświetlany dla każdego elementu na liście jest określany przez ustawienie `DataTextField` atrybutu. `DataTextField` Używa atrybutu `CategoryName` z `Category` klasy (pokazanym powyżej), aby wyświetlić każdej kategorii w **DropDownList** formantu. Rzeczywista wartość, która jest przekazywany, gdy element jest zaznaczony w **DropDownList** formant jest oparty na `DataValueField` atrybutu. `DataValueField` Atrybut ma ustawioną `CategoryID` jak zdefiniowano w `Category` klasy (pokazanym powyżej).

### <a name="how-the-application-will-work"></a>Działania aplikacji

Gdy użytkownik należący do roli "canEdit" przechodzi do strony po raz pierwszy, `DropDownAddCategory` **DropDownList** kontroli jest wypełniana zgodnie z powyższym opisem. `DropDownRemoveProduct` **DropDownList** kontroli również jest wypełniana przy użyciu tej samej metody produktów. Użytkownika należącego do roli "canEdit" wybiera typ kategorii i dodaje informacje szczegółowe (**nazwa**, **opis**, **cen**, i **plik obrazu**). Po kliknięciu przez użytkownika należącego do roli "canEdit" **Dodaj produkt** przycisku `AddProductButton_Click` zostanie wywołany program obsługi zdarzeń. `AddProductButton_Click` Obsługi zdarzeń znajdujących się w pliku CodeBehind (*AdminPage.aspx.cs*) sprawdza plik obrazu, aby się upewnić, że jest on zgodny typów plików dozwolonych *(.gif*, *.png*, *.jpeg*, lub *.jpg*). Następnie plik obrazu jest zapisywany w folderze Wingtip Toys przykładowej aplikacji. Następnie nowy produkt jest dodawana do bazy danych. Do wykonania, dodanie nowego produktu, nowe wystąpienie klasy `AddProducts` klasy nie zostanie utworzona i o nazwie produktów. `AddProducts` Klasa ma metodę o nazwie `AddProduct`, i obiektu produktów wywołuje tę metodę, aby dodać produktów w bazie danych.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Jeśli kod dodaje pomyślnie nowego produktu do bazy danych, strona zostanie ponownie załadowana z wartością ciągu zapytania `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Gdy ponowne ładowanie strony, ciąg zapytania są zawarte w adresie URL. Poprzez ponowne ładowanie strony, użytkownik należący do roli "canEdit" można natychmiast zobaczyć aktualizacje w **DropDownList** formantów na *AdminPage.aspx* strony. Ponadto umieszczając ciąg zapytania z adresem URL strony można wyświetlić komunikat informujący użytkownika należącego do roli "canEdit".

Gdy *AdminPage.aspx* strony ładunki `Page_Load` zdarzenie jest wywoływane.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Program obsługi zdarzeń sprawdza wartość ciągu kwerendy i określa, czy ma być wyświetlany komunikat z potwierdzeniem.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikacji teraz, aby zobaczyć sposób dodawania, usuwania i aktualizacji elementów w koszyku. Łączna wartość koszyka zakupów będzie odzwierciedlać całkowity koszt wszystkich elementów w koszyku.

1. W Eksploratorze rozwiązań, naciśnij klawisz **F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
   W przeglądarce zostanie otwarty i przedstawia *Default.aspx* strony.
2. Kliknij przycisk **Zaloguj** łącze w górnej części strony. 

    ![Członkostwo i administrowanie — dziennik łącza](membership-and-administration/_static/image2.png)

   *Login.aspx* zostanie wyświetlona strona.
3. Użyj następującej nazwy użytkownika i hasła:  
   Nazwa użytkownika: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![Członkostwo i administrowanie — strony logowania](membership-and-administration/_static/image3.png)
4. Kliknij przycisk **Zaloguj** znajdujący się u dołu strony.
5. U góry następnej strony, zaznacz pole wyboru **Admin** łącze, aby przejść do *AdminPage.aspx* strony. 

    ![Członkostwo i Administracja - Link administratora](membership-and-administration/_static/image4.png)
6. Aby przetestować sprawdzania poprawności danych wejściowych, **Dodaj produkt** przycisk bez dodawania żadnych szczegółów produktu. 

    ![Członkostwo i administrowanie — strony administratora](membership-and-administration/_static/image5.png)

   Zwróć uwagę, że są wyświetlane komunikaty wymaganego pola.
7. Dodaj szczegóły nowego produktu, a następnie kliknij przycisk **Dodaj produkt** przycisku. 

    ![Członkostwo i administrowanie — Dodawanie produktu](membership-and-administration/_static/image6.png)
8. Wybierz **produktów** z menu górnym menu nawigacyjnym, aby wyświetlić nowy produkt dodane. 

    ![Członkostwo i administrowanie — Pokaż nowego produktu](membership-and-administration/_static/image7.png)
9. Kliknij przycisk **Admin** łącze, aby powrócić do strony Administracja.
10. W **Usuń produktu** części strony, wybierz nowego produktu dodane w **DropDownListBox**.
11. Kliknij przycisk **Usuń produktu** przycisk, aby usunąć nowego produktu z aplikacji. 

    ![Członkostwo i administrowanie — Usuń produktu](membership-and-administration/_static/image8.png)
12. Wybierz **produkty** w menu górnym menu nawigacyjnym, aby upewnić się, że produkt został usunięty.
13. Kliknij przycisk **wylogować** istnieć w trybie administracji.   
    Należy zauważyć, że nie jest już wyświetlana w okienku nawigacji w górnym **Admin** elementu menu.

## <a name="summary"></a>Podsumowanie

W tym samouczku dodaniu niestandardowej roli zabezpieczeń i użytkownika należącego do roli niestandardowych ograniczony dostęp do folderu administracji i strony i podano nawigacji dla użytkownika należącego do roli niestandardowych. Wiązania modelu są używane do wypełnienia **DropDownList** formantu z danymi. Zostanie zaimplementowana **przekazywaniem plików** kontroli i formanty walidacji. Ponadto ma przedstawiono sposób dodawania i usuwania produktów z bazy danych. W następnym samouczku nauczysz się, jak zaimplementować routingu platformy ASP.NET.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Web.config — autoryzacji elementu](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, OAuth i bazy danych SQL Azure witrynę sieci Web](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](checkout-and-payment-with-paypal.md)
> [dalej](url-routing.md)
