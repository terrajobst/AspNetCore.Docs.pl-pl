---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Członkostwo i Administracja | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 23d08d5a05a8321fbc794e2c9b54cc39c9b5baf6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752058"
---
<a name="membership-and-administration"></a>Członkostwo i Administracja
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku dowiesz się, jak zaktualizować przykładowej aplikacji Wingtip Toys Dodaj rolę niestandardową i używać tożsamości ASP.NET. Go również pokazano, jak zaimplementować na stronie Administracja, z którego użytkownik o roli niestandardowej można dodawać i usuwać produkty z witryny sieci Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) jest systemu członkostwa, używany do tworzenia aplikacji sieci web ASP.NET i jest dostępny w programie ASP.NET 4.5. ASP.NET Identity jest używany w szablon projektu Visual Studio 2013 Web Forms, a także szablony dla [platformy ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), i [aplikacji jednostronicowej ASP.NET](../../../../single-page-application/index.md). Można również specjalnie zainstalować system tożsamości ASP.NET, za pomocą narzędzia NuGet, po uruchomieniu z pustą aplikację sieci Web. W tej serii samouczków można jednak użyć **formularzy sieci Web**projecttemplate, w tym system tożsamości ASP.NET. ASP.NET Identity ułatwia integrowanie danych profilu określonego użytkownika z danych aplikacji. Ponadto produktu ASP.NET Identity umożliwia wybierz model stanów trwałych dla profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych programu SQL Server lub innym magazynem danych, w tym *NoSQL* magazyny danych, takie jak tabele magazynu systemu Windows Azure.

Ten samouczek opiera się na poprzednim samouczku pod tytułem "Wyewidencjonowania i płatności za pomocą systemu PayPal" w tej serii samouczka Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać niestandardową rolę i użytkownika do aplikacji za pomocą kodu.
- Jak ograniczyć dostęp do folderu administracji i strony.
- Jak zapewnić nawigacji dla użytkownika, który należy do roli niestandardowej.
- Jak używać wiązania modelu do wypełniania [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) kontrolki z kategorii produktów.
- Jak przekazać plik do aplikacji sieci web przy użyciu [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) kontroli.
- Jak korzystać z kontrolek weryfikacji do zaimplementowania walidacji danych wejściowych.
- Jak dodać i usunąć produktów z aplikacji.

## <a name="these-features-are-included-in-the-tutorial"></a>W tym samouczku znajdują się następujące funkcje:

- ASP.NET Identity
- Konfiguracja i autoryzacji
- Wiązanie modelu
- Sprawdzania poprawności dyskretnego kodu

ASP.NET Web Forms zapewnia możliwości członkostwa. Za pomocą domyślnego szablonu, masz funkcje wbudowane członkostwo, które można użyć od razu po uruchomieniu aplikacji. W tym samouczku dowiesz się, jak za pomocą tożsamości ASP.NET Dodaj rolę niestandardową i przypisać użytkownika do tej roli. Dowiesz się jak ograniczyć dostęp do folderu administracji. Strona dodasz do folderu administracji, który umożliwia użytkownikowi rolę niestandardową do dodawania i usuwania produktów i wersji zapoznawczej produktu po został dodany.

## <a name="adding-a-custom-role"></a>Dodawanie roli niestandardowej

Za pomocą tożsamości ASP.NET, możesz dodać niestandardową rolę i przypisać użytkownika do roli przy użyciu kodu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *logiki* folder i Utwórz nową klasę.
2. Nadaj nowej klasie *RoleActions.cs*.
3. Modyfikowanie kodu, aby była ona wyświetlana w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. W **Eksploratora rozwiązań**, otwórz *Global.asax.cs* pliku.
5. Modyfikowanie *Global.asax.cs* pliku, dodając kod wyróżniony na żółto, tak aby była wyświetlana w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Należy zauważyć, że `AddUserAndRole` jest podkreślone na czerwono. Kliknij dwukrotnie kodu AddUserAndRole.  
   Literę "A" na początku metody wyróżniony zostanie podkreślone.
7. Umieść kursor nad literę "A", a następnie kliknij pozycję interfejs użytkownika, który służy do generowania szkieletu metody dla `AddUserAndRole` metody. 

    ![Członkostwo i Advministration — Generuj szkielet metody](membership-and-administration/_static/image1.png)
8. Kliknij opcję o nazwie:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Otwórz *RoleActions.cs* plik wchodzącej w skład *logiki* folderu.  
   `AddUserAndRole` Metoda została dodana do pliku klasy.
10. Modyfikowanie *RoleActions.cs* pliku przez usunięcie `NotImplementedeException` i dodawanie kod wyróżniony na żółto, aby była ona wyświetlana w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Powyższy kod najpierw ustanawia kontekst bazy danych dla bazy danych członkostwa. Bazy danych członkostwa także jest przechowywany jako *.mdf* w pliku *aplikacji\_danych* folderu. Będzie można wyświetlić tej bazy danych, gdy pierwszy użytkownik zalogował się w tej aplikacji sieci web. 

> [!NOTE] 
> 
> Jeśli użytkownik chce przechowywać danych o członkostwie wraz z danymi produktów, można rozważyć, korzystając z tych samych **DbContext** umożliwia przechowywanie danych produktu w powyższym kodzie.


 *Wewnętrzny* — słowo kluczowe jest modyfikatorem dostępu dla typów (na przykład klasy) i elementy członkowskie typu (np. metody lub właściwości). Typy wewnętrzne lub elementy członkowskie są dostępne tylko w obrębie pliki znajdujące się w tym samym zestawie *(.dll* pliku). Podczas kompilowania aplikacji, plik zestawu *(.dll*) jest tworzony zawierający kod, który jest wykonywany po uruchomieniu aplikacji. 

Element `RoleStore` obiektu, który umożliwia zarządzanie rolami, jest tworzony w zależności od kontekstu bazy danych.

> [!NOTE] 
> 
> Należy zauważyć, że w przypadku `RoleStore` tworzony jest obiekt używa ona ogólnych `IdentityRole` typu. Oznacza to, że `RoleStore` jest dozwolona tylko w celu uwzględnienia `IdentityRole` obiektów. Również za pomocą typów ogólnych, zasoby pamięci są obsługiwane lepiej.


Następnie `RoleManager` obiektu, jest tworzony na podstawie `RoleStore` obiektu, który został utworzony. `RoleManager` obiektu ujawnia powiązany z rolą interfejs API, który może służyć do automatycznie zapisują zmiany `RoleStore`. `RoleManager` Jest dozwolona tylko w celu uwzględnienia `IdentityRole` obiektów, ponieważ kod używa `<IdentityRole>` typu ogólnego.

Należy wywołać `RoleExists` metodę pozwala ustalić, czy rola "canEdit" znajduje się w bazie danych członkostwa. Jeśli nie jest dostępne, możesz utworzyć rolę.

Tworzenie `UserManager` obiekt wydaje się bardziej skomplikowane niż `RoleManager` kontrolować, jednak są prawie takie same. Po prostu kod w jednym wierszu, a nie dla kilku. W tym miejscu parametr, który kończy się sukcesem jest utworzenie wystąpienia jako nowy obiekt zawarte w nawiasach.

Następnie należy utworzyć użytkownika "canEditUser" przez utworzenie nowego `ApplicationUser` obiektu. Następnie jeśli pomyślnie utworzyć użytkownika, użytkownik zostanie dodany do nowej roli.

> [!NOTE] 
> 
> Obsługa błędów będzie aktualizowany podczas samouczek "Obsługę błędów programu ASP.NET" w dalszej części tej serii samouczków.


Przy następnym uruchomieniu aplikacji użytkownik o nazwie "canEditUser" zostanie dodany jako roli o nazwie "canEdit" aplikacji. W dalszej części tego samouczka będzie zalogować się jako użytkownik "canEditUser", aby wyświetlić dodatkowe możliwości, które będą dodane w ramach tego samouczka. Interfejs API szczegółowe informacje na temat tożsamości ASP.NET, zobacz [Namespace Microsoft.AspNet.Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Aby uzyskać dodatkowe szczegóły dotyczące inicjowania systemu tożsamości ASP.NET, zobacz [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Ograniczanie dostępu do strony administrowania

Przykładowej aplikacji Wingtip Toys umożliwia zarówno użytkowników anonimowych i zalogowanych użytkowników wyświetlić i nabywać produkty. Jednak zalogowanego użytkownika, który ma rolę niestandardowe "canEdit" dostęp ograniczony strony, aby można było dodawać i usuwać produktów.

#### <a name="add-an-administration-folder-and-page"></a>Dodaj Folder administracji i strony

Następnie utworzysz folder o nazwie *administratora* dla użytkownika "canEditUser" należącego do roli niestandardowej firmy Wingtip Toys przykładowej aplikacji.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **Dodaj**  - &gt; **nowy Folder**.
2. Nadaj nazwę nowego folderu *administratora*.
3. Kliknij prawym przyciskiem myszy *administratora* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
4. Wybierz <strong>Visual C#</strong> - &gt; <strong>Web</strong> grupy szablonów po lewej stronie. Wybierz z listy środkowej <strong>formularz sieci Web ze stroną wzorcową</strong>, nadaj jej nazwę <em>AdminPage.aspx</em><strong>,</strong> , a następnie wybierz <strong>Dodaj</strong>.
5. Wybierz *Site.Master* jako stronę wzorcową, a następnie wybierz **OK**.

#### <a name="add-a-webconfig-file"></a>Dodanie pliku Web.config

Dodając *Web.config* plik *administratora* folderu, można ograniczyć dostęp do stron znajdujących się w folderze.

1. Kliknij prawym przyciskiem myszy *administratora* i wybierz polecenie **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz z listy szablonów sieci web programu Visual C#, <strong>pliku konfiguracji sieci Web</strong>z środkową listę Zaakceptuj domyślną nazwę <em>Web.config</em><strong>,</strong> , a następnie wybierz <strong>Dodaj</strong>.
3. Zastąp istniejący kod XML zawartość w *Web.config* pliku następującym kodem:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Zapisz *Web.config* pliku. *Web.config* plik Określa ten tylko użytkownik należący do roli "canEdit" aplikacji można uzyskać dostęp do strony, zawarte w *administratora* folderu.

### <a name="including-custom-role-navigation"></a>W tym rolę niestandardową nawigacji

Aby umożliwić użytkownikowi roli niestandardowej "canEdit" Przejdź do sekcji Administracja aplikacji, należy dodać link do *Site.Master* strony. Tylko użytkownicy, którzy należą do roli "canEdit", będą mogli zobaczyć **administratora** łącze i uzyskać dostęp do sekcji Administracja.

1. W Eksploratorze rozwiązań należy znaleźć i otworzyć *Site.Master* strony.
2. Aby utworzyć link do użytkownika w roli "canEdit", należy dodać znaczników wyróżniony na żółto następująca Lista nieuporządkowana `<ul>` element, aby jako zostanie wyświetlona lista poniżej:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Otwórz *Site.Master.cs* pliku. Wprowadź **administratora** łącze widoczne tylko dla użytkownika "canEditUser", dodając kod wyróżniony na żółto do `Page_Load` programu obsługi. `Page_Load` Obsługi będzie wyglądać następująco:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Po załadowaniu strony kod sprawdza, czy zalogowany użytkownik ma rolę "canEdit". Jeśli użytkownik należy do roli "canEdit", element span zawierającą łącze do *AdminPage.aspx* strony (i w związku z tym link wewnątrz zakresu) jest widoczny.

### <a name="enabling-product-administration"></a>Włączanie administracji produktu

Do tej pory utworzono rolę "canEdit" i dodane przez użytkownika "canEditUser", folder administracji i strony Administracja. Ustawiono prawa dostępu do folderu administracji i strony, a następnie dodano łącze nawigacyjne dla użytkownika w roli "canEdit" do aplikacji. Następnie dodasz znaczników w celu *AdminPage.aspx* strony, a kod *AdminPage.aspx.cs* pliku związanego z kodem, który umożliwi użytkownikowi z rolą "canEdit" Dodawanie i usuwanie produktów.

1. W **Eksploratora rozwiązań**, otwórz *AdminPage.aspx* plik wchodzącej w skład *administratora* folderu.
2. Zastąp istniejący kod znaczników następujących czynności:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Następnie otwórz *AdminPage.aspx.cs* pliku związanego z kodem, klikając prawym przyciskiem myszy *AdminPage.aspx* i klikając **Wyświetl kod**.
4. Zastąp istniejący kod w *AdminPage.aspx.cs* pliku związanego z kodem następującym kodem:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

W kodzie, który wprowadzono dla *AdminPage.aspx.cs* pliku związanego z kodem, klasę o nazwie `AddProducts` wykonuje faktyczną pracę dodawania produktów w bazie danych. Ta klasa jeszcze nie istnieje, więc będzie można utworzyć go teraz.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **kodu** grupy szablonów po lewej stronie. Następnie wybierz **klasy**ze środka listy i nadaj mu nazwę *AddProducts.cs*.   
   Zostanie wyświetlony nowy plik klasy.
3. Zastąp istniejący kod następujących czynności:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* strona pozwala na użytkowników należących do roli "canEdit", do dodawania i usuwania produktów. Gdy zostanie dodany nowy produkt, szczegółowe informacje o produkcie są weryfikowane i następnie wprowadzany do bazy danych. Nowy produkt jest natychmiast dostępne dla wszystkich użytkowników w aplikacji sieci web.

#### <a name="unobtrusive-validation"></a>Sprawdzania poprawności dyskretnego kodu

Szczegóły produktu, które użytkownik udostępnia na *AdminPage.aspx* strony są weryfikowane przy użyciu kontrolkami walidacji (`RequiredFieldValidator` i `RegularExpressionValidator`). Te kontrolki automatycznie używać sprawdzania poprawności dyskretnego kodu. Sprawdzania poprawności dyskretnego kodu umożliwia kontrolek weryfikacji do logiki weryfikacji po stronie klienta, co oznacza, że strona nie wymaga dwustronnej z serwerem, aby zostać uwierzytelnionym przy użyciu języka JavaScript. Domyślnie, sprawdzania poprawności dyskretnego kodu jest objęta *Web.config* plik oparty na następujące ustawienia konfiguracji:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Wyrażenia regularne

Cena produktu na *AdminPage.aspx* strony jest weryfikowany przy użyciu **RegularExpressionValidator** kontroli. Ten formant sprawdza, czy wartość kontrolki wejściowe skojarzone (poletekstowe "AddProductPrice") jest zgodny ze wzorcem określonym przez wyrażenia regularne. Wyrażenie regularne jest notacji dopasowania do wzorca, który pozwala na szybkie znalezienie i wzory znaków specyficznych dopasowania. **RegularExpressionValidator** formant zawiera właściwość o nazwie `ValidationExpression` zawiera wyrażenie regularne służące do sprawdzania poprawności danych wejściowych ceny, jak pokazano poniżej:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Kontrolka fileUpload

Oprócz danych wejściowych i sprawdzania poprawności formantów dodawanych **FileUpload** kontrolę *AdminPage.aspx* strony. Formant ten zapewnia możliwość przekazywania plików. W tym przypadku są pozwalając tylko pliki obrazów do przekazania. W pliku związanym z kodem (*AdminPage.aspx.cs*), gdy `AddProductButton` zostanie kliknięty kontroli kodu `HasFile` właściwość **FileUpload** kontroli. Jeśli kontrolka ma plik, a jeśli typ pliku (na podstawie rozszerzenia pliku) jest dozwolone, obraz, który jest zapisywany do *obrazów* folder i *obrazów/Thumbs* folderu aplikacji.

#### <a name="model-binding"></a>Wiązanie modelu

We wcześniejszej części tej serii samouczków wiązania modelu używanych do wypełniania **ListView** kontroli **FormsView** kontrolki, **GridView** kontroli i  **DetailView** kontroli. W tym samouczku Użyj wiązania modelu do wypełnienia **DropDownList** kontrolki z listy kategorii produktów.

Kod znaczników, który został dodany do *AdminPage.aspx* plik zawiera **DropDownList** formant nazywany `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Umożliwia powiązanie modelu wypełnić to pole **DropDownList** , ustawiając `ItemType` atrybutu i `SelectMethod` atrybutu. `ItemType` Atrybut określa, że używasz `WingtipToys.Models.Category` wpisz podczas wypełniania kontrolki. Definicja tego typu na początku tej serii samouczków, tworząc `Category` klasy (pokazana poniżej). `Category` Klasa się zebrała *modeli* folder wewnątrz *Category.cs* pliku.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Atrybutu **DropDownList** kontroli określa, że używasz `GetCategories` — metoda (pokazana poniżej) oznacza to zawarte w pliku związanym z kodem (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Ta metoda określa, że `IQueryable` interfejs jest używany do oceny zapytania dotyczącego `Category` typu. Zwrócona wartość jest używana do wypełniania **DropDownList** w znaczniku strony (*AdminPage.aspx*).

Tekst wyświetlany dla każdego elementu na liście jest określany przez ustawienie `DataTextField` atrybutu. `DataTextField` Atrybutu używa `CategoryName` z `Category` klasy (pokazany powyżej), aby wyświetlić każdej kategorii w **DropDownList** kontroli. Rzeczywista wartość, która jest przekazywana, gdy element jest zaznaczony na **DropDownList** formant opiera się na `DataValueField` atrybutu. `DataValueField` Ma ustawioną wartość atrybutu `CategoryID` jak zdefiniować w `Category` klasy (jak pokazano powyżej).

### <a name="how-the-application-will-work"></a>Działanie aplikacji

Gdy użytkownik należący do roli "canEdit" przechodzi do strony po raz pierwszy `DropDownAddCategory` **DropDownList** kontroli jest wypełniana zgodnie z powyższym opisem. `DropDownRemoveProduct` **DropDownList** kontrolka również zostanie wypełniona przy użyciu tej samej metody produktów. Użytkownika należącego do roli "canEdit" wybiera typ kategorii i dodaje szczegóły produktu (**nazwa**, **opis**, **cena**, i **plik obrazu**). Po kliknięciu przez użytkownika należącego do roli "canEdit" **Dodaj produkt** przycisku `AddProductButton_Click` zostanie wywołany program obsługi zdarzeń. `AddProductButton_Click` Programu obsługi zdarzeń znajdujących się w pliku związanym z kodem (*AdminPage.aspx.cs*) sprawdza, czy plik obrazu, aby upewnić się, jest on zgodny typów plików dozwolonych *(.gif*, *.png*, *JPEG*, lub *.jpg*). Następnie ten obraz zostanie zapisany w folderze przykładowej aplikacji Wingtip Toys. Następnie nowy produkt jest dodawany do bazy danych. Aby osiągnąć, dodawanie nowego produktu, nowe wystąpienie klasy `AddProducts` klasy jest tworzony i o nazwie produktów. `AddProducts` Klasa ma metodę o nazwie `AddProduct`, oraz obiekt produktów wywołuje tę metodę, aby dodać produktów w bazie danych.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Jeśli kod pomyślnie dodaje nowy produkt do bazy danych, strony są ładowane z wartością ciągu zapytania `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Gdy ponownie ładuje stronę, ciąg zapytania są zawarte w adresie URL. Przez ponownie załadować stronę, użytkownika należącego do roli "canEdit" mogą natychmiast zobaczyć aktualizacje w **DropDownList** formantów na *AdminPage.aspx* strony. Ponadto umieszczając ciąg zapytania do adresu URL, stronę można wyświetlić komunikat o powodzeniu użytkownika należącego do roli "canEdit".

Gdy *AdminPage.aspx* stronie ładunki `Page_Load` zdarzenie jest wywoływane.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Program obsługi zdarzeń sprawdza, czy wartość ciągu zapytania i określa, czy ma być wyświetlany komunikat o powodzeniu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikację teraz, aby zobaczyć sposób dodawania, usuwania i aktualizacji elementów w koszyku. Suma koszyka zakupów, zostanie naliczona łączny koszt wszystkich elementów w koszyku.

1. W Eksploratorze rozwiązań, naciśnij klawisz **F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
   Przeglądarki otwiera się i pokazuje *Default.aspx* strony.
2. Kliknij przycisk **Zaloguj** widocznego u góry strony. 

    ![Członkostwo i Administracja — Zaloguj się w Link](membership-and-administration/_static/image2.png)

   *Login.aspx* zostanie wyświetlona strona.
3. Użyj następującej nazwy użytkownika i hasła:  
   Nazwa użytkownika: canEditUser@wingtiptoys.com  
   Hasło: Pa$ $word1 

    ![Członkostwo i Administracja — strony logowania](membership-and-administration/_static/image3.png)
4. Kliknij przycisk **Zaloguj** przycisk w dolnej części strony.
5. W górnej części następnej strony wybierz **administratora** link, aby przejść do *AdminPage.aspx* strony. 

    ![Członkostwo i Administracja - Link administratora](membership-and-administration/_static/image4.png)
6. Aby przetestować walidacji danych wejściowych, kliknij pozycję **Dodaj produkt** przycisk bez dodawania żadnych szczegółów produktu. 

    ![Członkostwo i Administracja — strony administratora](membership-and-administration/_static/image5.png)

   Należy zauważyć, że są wyświetlane komunikaty wymaganego pola.
7. Dodaj szczegóły nowego produktu, a następnie kliknij przycisk **Dodaj produkt** przycisku. 

    ![Członkostwo i Administracja — Dodaj produkt](membership-and-administration/_static/image6.png)
8. Wybierz **produktów** menu górnym menu nawigacyjnym, aby wyświetlić nowy produkt dodane. 

    ![Członkostwo i Administracja — Pokaż nowego produktu](membership-and-administration/_static/image7.png)
9. Kliknij przycisk **administratora** link umożliwiający powrót do strony administrowania.
10. W **Usuń produkt** części strony, wybierz nowego produktu, które zostały dodane w **DropDownListBox**.
11. Kliknij przycisk **Usuń produkt** przycisk, aby usunąć nowy produkt z aplikacji. 

    ![Członkostwo i Administracja — usuwanie produktu](membership-and-administration/_static/image8.png)
12. Wybierz **produktów** menu górnym menu nawigacyjnym, aby upewnić się, że produkt został usunięty.
13. Kliknij przycisk **wylogować** istnieć trybu administracji.   
    Należy zauważyć, że w górnym okienku nawigacji nie jest już wyświetlana **administratora** elementu menu.

## <a name="summary"></a>Podsumowanie

W tym samouczku dodaje niestandardową rolę i użytkownika należącego do roli niestandardowej ograniczony dostęp do folderu administracji i strony i podano nawigacji dla użytkownika należącego do roli niestandardowej. Wiązanie modelu jest używana do wypełniania **DropDownList** kontrolki z danymi. Możesz zaimplementować **FileUpload** kontroli i sprawdzania poprawności formantów. Ponadto mają przedstawiono sposób dodawania i usuwania produktów z bazy danych. W następnym samouczku dowiesz się, jak zaimplementować routingu platformy ASP.NET.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Web.config — autoryzacja — Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](checkout-and-payment-with-paypal.md)
> [dalej](url-routing.md)
