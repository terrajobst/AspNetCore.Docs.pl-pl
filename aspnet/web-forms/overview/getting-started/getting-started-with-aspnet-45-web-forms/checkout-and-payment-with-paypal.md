---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Finalizacja zakupu i płatność w systemie PayPal | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: b59a395e255823a732aef1b899612063e09b2424
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755747"
---
<a name="checkout-and-payment-with-paypal"></a>Finalizacja zakupu i płatność w systemie PayPal
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku opisano sposób modyfikowania przykładowej aplikacji Wingtip Toys obejmujący autoryzacji użytkowników, Rejestracja i płatności PayPal. Tylko użytkownicy, którzy są zalogowani mają autoryzację do zakupu produktów. Szablon projektu platformy ASP.NET 4.5 Web Forms wbudowanych rejestracji funkcji już zawiera większość potrzebnych składników. Doda funkcji PayPal Express wyewidencjonowania. W tym samouczku jest użycie developer PayPal środowiska, testowania, dzięki czemu nie rzeczywiste fundusze zostaną przekazane. Na końcu tego samouczka zostanie przetestować aplikację, wybierając produktów do dodania do koszyka, klikając przycisk wyewidencjonowania i przesyłania danych do witryny sieci web z testowania PayPal. W witrynie sieci web testowania PayPal będzie Potwierdź informacje, wysyłki i płatności, a następnie wróć do lokalnego przykładowej aplikacji Wingtip Toys potwierdzenie i ukończenie zakupu.

Obejmuje kilka procesorów doświadczonym płatności specjalizujących zakupy online, że adres skalowalności i bezpieczeństwa. Deweloperzy platformy ASP.NET, należy wziąć pod uwagę zalety przy użyciu rozwiązania płatności innej przed Implementowanie, shopping i zakupu rozwiązań.

> [!NOTE] 
> 
> Przykładowej aplikacji Wingtip Toys zaprojektowano tak, aby widocznym określonych koncepcji platformy ASP.NET i funkcje dostępne dla deweloperów sieci web platformy ASP.NET. Ta przykładowa aplikacja nie została zoptymalizowana dla każdych okolicznościach możliwości w zakresie skalowalności i bezpieczeństwa.


## <a name="what-youll-learn"></a>Zawartość:

- Jak ograniczyć dostęp do określonych stron w folderze.
- Jak utworzyć znanych koszyk sklepowy z anonimowych koszyk sklepowy.
- Jak włączyć protokół SSL dla projektu.
- Jak dodać dostawcę uwierzytelniania OAuth do projektu.
- Jak kupować produkty przy użyciu środowiska testowania PayPal za pomocą systemu PayPal.
- Jak wyświetlać szczegółowe informacje z systemu PayPal w **DetailsView** kontroli.
- Jak zaktualizować bazę danych aplikacji Wingtip Toys ze szczegółowymi informacjami uzyskany z systemu PayPal.

## <a name="adding-order-tracking"></a>Dodawanie śledzenia zamówień

W tym samouczku utworzysz dwie nowe klasy do śledzenia danych od kolejności, w której został utworzony przez użytkownika. Klasy będzie śledzić dane dotyczące informacje o wysyłce, łącznie z zakupu i potwierdzenia płatności.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Dodawanie klasy modelu OrderDetail i kolejności

We wcześniejszej części tej serii samouczków, definicja schematu dla kategorii, produktów, oraz koszyka elementów, tworząc `Category`, `Product`, i `CartItem` klas w *modeli* folderu. Teraz należy dodać dwa nowe klasy do definiowania schematu dla zamówienia produktów oraz szczegóły wybranego zamówienia.

1. W **modeli** folderu, Dodaj nową klasę o nazwie *Order.cs*.   
   Plik nowej klasy jest wyświetlany w edytorze.
2. Zastąp w kodzie domyślnym następujących czynności:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Dodaj *OrderDetail.cs* klasy *modeli* folderu.
4. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` i `OrderDetail` klasy zawiera schemat zdefiniować kolejność informacje używane do kupowania i wysyłania.

Ponadto należy zaktualizować klasy kontekstu bazy danych, który zarządza klas jednostek i który zapewnia dostęp do danych w bazie danych. Aby to zrobić, doda nowo utworzony kolejności i `OrderDetail` klasy do modeli `ProductContext` klasy.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ProductContext.cs* pliku.
2. Dodaj wyróżniony kod do *ProductContext.cs* pliku, jak pokazano poniżej:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Jak wspomniano wcześniej w tej serii samouczków, kod w *ProductContext.cs* dodaje plik `System.Data.Entity` przestrzeni nazw, aby mieć dostęp do wszystkich funkcji programu Entity Framework core. Ta funkcja obejmuje możliwość zapytania, wstawiania, aktualizacji i usuwania danych dzięki współpracy z silnie typizowanych obiektów. Powyższy kod w `ProductContext` klasa dodaje Entity Framework dostępu do nowo dodanych `Order` i `OrderDetail` klasy.

## <a name="adding-checkout-access"></a>Dodawanie dostępu do wyewidencjonowania

Przykładowej aplikacji Wingtip Toys zezwala użytkownikom anonimowym na przejrzenie i dodać produkty do koszyka. Jednak przy wyborze użytkowników anonimowych do zakupu produktów, które są dodawane do koszyka, mogą zalogować się do witryny. Po zalogowaniu, dostęp ograniczony stron aplikacji sieci Web, które obsługują wyewidencjonowanie i proces zakupu. Te strony ograniczone są zawarte w *wyewidencjonowania* folderu aplikacji.

### <a name="add-a-checkout-folder-and-pages"></a>Dodaj Folder wyewidencjonowania i stron

Teraz utworzysz *wyewidencjonowania* folder i stron, których klient zostanie wyświetlony podczas procesu realizowania zamówienia. Te strony zaktualizuje w dalszej części tego samouczka.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **dodać nowy Folder**. 

    ![Finalizacja zakupu i płatność w systemie PayPal — nowy Folder](checkout-and-payment-with-paypal/_static/image1.png)
2. Nadaj nazwę nowego folderu *wyewidencjonowania*.
3. Kliknij prawym przyciskiem myszy *wyewidencjonowania* folder, a następnie wybierz **Dodaj**-&gt;**nowy element**. 

    ![Finalizacja zakupu i płatność w systemie PayPal — nowy element](checkout-and-payment-with-paypal/_static/image2.png)
4. **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
5. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. W środkowym okienku wybierz **formularz sieci Web ze stroną wzorcową**i nadaj mu nazwę *CheckoutStart.aspx*. 

    ![Finalizacja zakupu i płatność w systemie PayPal — Dodaj okno dialogowe nowego elementu](checkout-and-payment-with-paypal/_static/image3.png)
6. Tak jak poprzednio, wybierz *Site.Master* pliku strony wzorcowej.
7. Dodaj następujące dodatkowe strony w celu *wyewidencjonowania* folderu przy użyciu tego samego powyższe kroki:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Dodanie pliku Web.config

Przez dodanie nowego *Web.config* plik *wyewidencjonowania* folderu, można ograniczyć dostęp do wszystkich stron znajdujących się w folderze.

1. Kliknij prawym przyciskiem myszy *wyewidencjonowania* i wybierz polecenie **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. W środkowym okienku wybierz **pliku konfiguracji sieci Web**, zaakceptuj domyślną nazwę *Web.config*, a następnie wybierz pozycję **Dodaj**.
3. Zastąp istniejący kod XML zawartość w *Web.config* pliku następującym kodem:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Zapisz *Web.config* pliku.

*Web.config* plik Określa, że wszyscy użytkownicy nieznanych aplikacji sieci Web muszą mieć dostępu do stron znajdujących się w *wyewidencjonowania* folderu. Jeśli jednak użytkownik ma zarejestrowane konto i jest zalogowany, ich będzie znanych użytkowników i będą mieć dostęp do stron w *wyewidencjonowania* folderu.

Ważne jest, aby należy pamiętać, że konfiguracja ASP.NET następuje hierarchii, gdzie każdy *Web.config* pliku ustawienia konfiguracji zostają zastosowane do folderu, który znajduje się w oraz do wszystkich katalogów podrzędnych.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Włącz protokół SSL dla projektu

 Secure Sockets Layer (SSL) to protokół zdefiniowanymi tak, aby zezwolić na serwerach sieci Web i klientami sieci Web do bardziej bezpiecznego komunikowania się przy użyciu technologii szyfrowania. Jeśli nie jest używany protokół SSL, dane przesyłane między klientem i serwerem jest otwarta dla pakietów wykrywanie, każda osoba mająca fizyczny dostęp do sieci. Ponadto kilka typowych schematów uwierzytelniania nie są bezpieczne przy użyciu zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy Wyślij niezaszyfrowane poświadczeń. Do zabezpieczenia te schematy uwierzytelniania, należy użyć protokołu SSL. 

1. W **Eksploratora rozwiązań**, kliknij przycisk **WingtipToys** projektu, a następnie naciśnij klawisz **F4** do wyświetlenia **właściwości** okna.
2. Zmiana **włączony protokół SSL** do `true`.
3. Kopiuj **adresu URL protokołu SSL** , dzięki czemu można jej użyć później.   
 Adres URL SSL będzie `https://localhost:44300/` , chyba że wcześniej utworzono SSL witryny sieci Web (jak pokazano poniżej).   
    ![Właściwości projektu](checkout-and-payment-with-paypal/_static/image4.png)
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys** projektu, a następnie kliknij przycisk **właściwości**.
5. Na karcie po lewej stronie kliknij **Web**.
6. Zmiana **adres Url projektu** używać **adresu URL protokołu SSL** zapisany wcześniej.   
    ![Właściwości dla projektu sieci Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Zapisz stronę, naciskając klawisz **CTRL + S**.
8. Naciśnij klawisz **kombinację klawiszy Ctrl + F5** do uruchomienia aplikacji. Program Visual Studio spowoduje wyświetlenie opcji, aby możliwe było uniknąć wyświetlania ostrzeżeń dotyczących protokołu SSL.
9. Kliknij przycisk **tak** ufać certyfikatowi SSL usług IIS Express i kontynuować.   
    ![Szczegóły certyfikatu SSL usług IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Zostanie wyświetlone ostrzeżenie zabezpieczeń.
10. Kliknij przycisk **tak** instalacji certyfikatu do usługi hosta lokalnego.   
    ![Okno dialogowe ostrzeżenia o zabezpieczeniach](checkout-and-payment-with-paypal/_static/image7.png)  
 Zostanie wyświetlone okno przeglądarki.

Teraz można łatwo przetestować aplikację sieci Web lokalnie przy użyciu protokołu SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Dodawanie dostawcy uwierzytelniania OAuth 2.0

ASP.NET Web Forms udostępnia rozszerzone opcje członkostwa i uwierzytelniania. Te ulepszenia obejmują OAuth. OAuth jest otwarty protokół, który umożliwia bezpieczne autoryzacji w proste oraz standardowe metody z aplikacji sieci web, mobilnych i klasycznych. Szablon formularzy sieci Web platformy ASP.NET używa protokołu OAuth w celu udostępnienia Facebook, Twitter, Google i Microsoft jako dostawcy uwierzytelniania. Chociaż w tym samouczku jest używany tylko Google jako dostawcy uwierzytelniania, można łatwo zmodyfikować kod, aby użyć innych dostawców. Kroki, aby wdrożyć innych dostawców są bardzo podobne do czynności, które będą widoczne w tym samouczku.

Oprócz uwierzytelniania samouczka będzie także ról można użyć do zaimplementowania autoryzacji. Tylko do tych użytkowników, możesz dodać do `canEdit` roli będą mogli zmieniać dane (Tworzenie, edytowanie lub usuwanie kontaktów).

> [!NOTE] 
> 
> Aplikacje Windows Live akceptować tylko na żywo adresu URL dla roboczej witryny sieci Web, więc nie możesz użyć adresu URL lokalną witrynę sieci Web do testowania nazwy logowania.


Poniższe kroki pozwala dodać dostawcę uwierzytelniania serwisu Google.

1. Otwórz *aplikacji\_Start\Startup.Auth.cs* pliku.
2. Usuń znaki komentarza z `app.UseGoogleAuthentication()` metoda tak, że metoda jest wyświetlany jako poniżej: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/). Należy również zalogować się przy użyciu swojego konta e-mail dewelopera Google (gmail.com). Jeśli nie masz konta Google, wybierz opcję **Tworzenie konta usługi** łącza.   
   Następnie zobaczysz **konsoli deweloperów Google**.   
    ![Konsola deweloperów Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Kliknij przycisk **Tworzenie projektu** przycisk, a następnie wprowadź nazwę projektu i identyfikator (możesz użyć wartości domyślnych). Następnie kliknij przycisk **wyboru umowy** i **Utwórz** przycisku.  

    ![Google - nowy projekt](checkout-and-payment-with-paypal/_static/image9.png)

   W ciągu kilku sekund zostanie utworzony nowy projekt, a przeglądarka wyświetli nową stronę projektów.
5. Na karcie po lewej stronie kliknij **interfejsów API &amp; uwierzytelniania**, a następnie kliknij przycisk **poświadczenia**.
6. Kliknij przycisk **Utwórz nowy identyfikator klienta** w obszarze **OAuth**.   
   **Utwórz identyfikator klienta** zostanie wyświetlone okno dialogowe.   
    ![Google - Utwórz identyfikator klienta](checkout-and-payment-with-paypal/_static/image10.png)
7. W **Utwórz identyfikator klienta** okno dialogowe, zachowaj ustawienie domyślne **aplikacji sieci Web** dla typu aplikacji.
8. Ustaw **autoryzowanych źródeł JavaScript** do adresu URL protokołu SSL używany we wcześniejszej części tego samouczka (`https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL).   
   Ten adres URL jest punkt początkowy aplikacji. W tym przykładzie tylko wprowadź adres URL localhost do testowania. Jednak można wprowadzić wiele adresów URL dla hosta lokalnego i produkcyjnych.
9. Ustaw **autoryzacji identyfikator URI przekierowania** do następującego: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Ta wartość jest identyfikator URI tego OAuth ASP.NET użytkowników do komunikacji z serwerem programu google OAuth. Należy pamiętać, adresu URL protokołu SSL, które zostały użyte powyżej ( `https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL).
10. Kliknij przycisk **Utwórz identyfikator klienta** przycisku.
11. W menu po lewej stronie konsoli deweloperów Google, kliknij polecenie **ekranie wyrażania zgody** element menu, a następnie ustaw swoje wiadomości e-mail adres i nazwy produktu. Po wypełnieniu formularza kliknij **Zapisz**.
12. Kliknij przycisk **interfejsów API** elementu menu, przewiń w dół i kliknij przycisk **poza** znajdujący się obok **interfejsu API Google +**.   
    Akceptowanie tej opcji spowoduje włączenie interfejsu API Google +.
13. Należy również zaktualizować **Microsoft.Owin** pakietu NuGet w wersji 3.0.0.   
    Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** , a następnie wybierz **Zarządzaj pakietami NuGet dla rozwiązania**.  
    Z **Zarządzaj pakietami NuGet** okien, Znajdź i aktualizacji **Microsoft.Owin** pakiet do wersji 3.0.0.
14. W programie Visual Studio, należy zaktualizować `UseGoogleAuthentication` metody *Startup.Auth.cs* strony przez kopiowanie i wklejanie **identyfikator klienta** i **klucz tajny klienta** do metody. **Identyfikator klienta** i **klucz tajny klienta** wartości podanych poniżej są przykłady i nie będzie działać. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby skompilować i uruchomić aplikację. Kliknij przycisk **Zaloguj** łącza.
16. W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.  
    ![Zaloguj się](checkout-and-payment-with-paypal/_static/image11.png)
17. Jeśli potrzebujesz wprowadzić swoje poświadczenia, nastąpi przekierowanie do witryny google, w którym należy wprowadzić poświadczenia.  
    ![Google - logowania](checkout-and-payment-with-paypal/_static/image12.png)
18. Po wprowadzeniu poświadczeń, wyświetli się monit o nadać uprawnienia do aplikacji sieci web, który został utworzony.  
    ![Projekt domyślne konto usługi](checkout-and-payment-with-paypal/_static/image13.png)
19. Kliknij przycisk **zaakceptować**. Teraz nastąpi przekierowanie do **zarejestrować** strony **WingtipToys** aplikacji, gdy zarejestrujesz swoje konto Google.  
    ![Zarejestruj za pomocą konta Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Masz możliwość zmiany nazwy rejestracji lokalny adres e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz zachować aliasu adresu e-mail domyślne (czyli ten, który używany do uwierzytelniania). Kliknij przycisk **Zaloguj** jak pokazano powyżej.

### <a name="modifying-login-functionality"></a>Modyfikowanie funkcji logowania

Jak już wspomniano w tej serii samouczków większość funkcji rejestracja użytkownika została uwzględniona w szablonie formularzy sieci Web ASP.NET domyślnie. Teraz należy zmodyfikować domyślną *Login.aspx* i *Register.aspx* strony, aby wywołać `MigrateCart` metody. `MigrateCart` Metoda kojarzy nowo zalogowanego użytkownika z anonimowych koszyk sklepowy. Przykładowej aplikacji Wingtip Toys skojarzenie użytkownika i koszyk, będzie można utrzymać koszyka użytkownika między wizyty.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *konta* folderu.
2. Modyfikuj stronę związanym z kodem o nazwie *Login.aspx.cs* obejmujący kod wyróżniony na żółto, tak aby była wyświetlana w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Zapisz *Login.aspx.cs* pliku.

Teraz możesz zignorować to ostrzeżenie, że nie ma żadnych definicji `MigrateCart` metody. Dodasz je nieco później w tym samouczku.

*Login.aspx.cs* pliku związanego z kodem obsługuje metodę logowania. Sprawdzając strony Login.aspx, zobaczysz, że ta strona zawiera przycisk "Zaloguj" że w przypadku kliknij wyzwalaczy `LogIn` obsługi na związanym z kodem.

Gdy `Login` metody *Login.aspx.cs* jest wywoływana, nowe wystąpienie klasy koszyka o nazwie `usersShoppingCart` jest tworzony. Identyfikator koszyka (GUID) jest pobierana i `cartId` zmiennej. Następnie `MigrateCart` metoda jest wywoływana z przekazaniem zarówno `cartId` i nazwa zalogowanego użytkownika do tej metody. Podczas migracji koszyka identyfikator GUID używany do identyfikowania anonimowe koszyka jest zastępowana nazwą użytkownika.

Oprócz modyfikowania *Login.aspx.cs* pliku związanego z kodem, aby Migrowanie koszyka, gdy użytkownik loguje się, musisz także zmodyfikować *pliku związanego z kodem Register.aspx.cs* do Migrowanie koszyka gdy użytkownik tworzy nowe konto i loguje się.

1. W *konta* folder, otwórz plik związany z kodem o nazwie *Register.aspx.cs*.
2. Modyfikowanie pliku związanego z kodem, dołączając kod w kolorze żółtym, tak, aby wyglądał następująco:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Zapisz *Register.aspx.cs* pliku. Jeszcze raz zignorować ostrzeżenie dotyczące `MigrateCart` metody.

Należy zauważyć, że kodu użyto w `CreateUser_Click` programu obsługi zdarzeń jest bardzo podobny do kod używany w `LogIn` metody. Kiedy użytkownik rejestruje lub loguje się do witryny wywołania `MigrateCart` metody zostaną wprowadzone.

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Teraz, gdy proces logowania i rejestracji aktualizacji, można dodać kod, aby przeprowadzić migrację do koszyk sklepowy przy użyciu `MigrateCart` metody.

1. W **Eksploratora rozwiązań**, Znajdź *logiki* folder i Otwórz *ShoppingCartActions.cs* pliku klasy.
2. Dodaj kod wyróżniony na żółto z istniejącym kodem w *ShoppingCartActions.cs* pliku, tak, aby kod w *ShoppingCartActions.cs* plik pojawia się w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` Metoda używa istniejących cartId w celu odnalezienia koszyka użytkownika. Następnie kod w pętli wszystkich elementów koszyka zakupów i zastępuje `CartId` właściwości (zgodnie z określonym `CartItem` schematu) o nazwie zalogowanego użytkownika.

### <a name="updating-the-database-connection"></a>Aktualizowanie połączenia z bazą danych

Jeśli korzystasz z tego samouczka przy użyciu **wstępnie** Wingtip Toys przykładowej aplikacji, należy ponownie utworzyć domyślna baza danych członkostwa. Po zmodyfikowaniu domyślne parametry połączenia, bazy danych członkostwa zostanie utworzony przy następnym uruchomieniu aplikacji.

1. Otwórz *Web.config* pliku w folderze głównym projektu.
2. Domyślne parametry połączenia należy zaktualizować tak, aby wyglądał następująco:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrowanie PayPal

PayPal to platforma rozliczania opartego na sieci web akceptujący płatności dokonywanych przez sprzedawców internetowych. Następnie w tym samouczku wyjaśniono, jak integrowanie PayPal Express funkcją finalizacji zakupu w aplikacji. Wyewidencjonuj Express pozwala klientom na użyć PayPal do zapłacenia za elementy dodane do ich koszyk sklepowy.

### <a name="create-paylpal-test-accounts"></a>Tworzenie kont PaylPal testu

Korzystanie z usługi PayPal, w środowisku testowym, należy utworzyć i zweryfikować konta dewelopera testu. Testowe konto dewelopera użyje do utworzenia kupujący konto testowe i testowe konto sprzedawcy. Poświadczenia konta dewelopera testu umożliwi również przykładowej aplikacji Wingtip Toys w celu dostępu do środowiska testowego systemu PayPal.

1. W przeglądarce przejdź do testowania witryny dewelopera PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Jeśli nie masz konta dewelopera systemu PayPal, należy utworzyć nowe konto, klikając **Zarejestruj**i postępowanie rejestracji kroki. Jeśli masz istniejące konto dewelopera systemu PayPal, zaloguj się, klikając **logowanie**. Konieczne będzie konta dewelopera systemu PayPal, aby przetestować przykładowej aplikacji Wingtip Toys w dalszej części tego samouczka.
3. Jeśli właśnie zarejestrowano do konta dewelopera systemu PayPal, może być konieczne Sprawdź konta PayPal dewelopera w systemie PayPal. Aby zweryfikować swoje konto, należy wykonać kroki, które PayPal wysyłane do swojego konta e-mail. Po zweryfikowaniu konta dewelopera systemu PayPal, zaloguj się do testowania witryny dewelopera PayPal.
4. Po użytkownik jest zalogowany do witryny dla deweloperów PayPal za pomocą konta dewelopera systemu PayPal, musisz utworzyć konto PayPal kupujący testu, jeśli już nie mieć jedną. Aby utworzyć konto testu nabywców, kliknij pozycję witryny PayPal **aplikacje** kartę, a następnie kliknij przycisk **kont piaskownicy**.   
 **Piaskownicy testowe konta** strona jest wyświetlana.   

    > [!NOTE] 
    > 
    > Witryny dewelopera PayPal zawiera już konta handlowca testu.

    ![Finalizacja zakupu i płatność w systemie PayPal — piaskownicy testowe konta](checkout-and-payment-with-paypal/_static/image15.png)
5. Na stronie kont piaskownicy testu kliknij **Utwórz konto**.
6. Na **Utwórz testowe konto** strony wybierz kupującego testową wiadomość e-mail z konta i hasła wybranych przez użytkownika.   

    > [!NOTE] 
    > 
    > Konieczne będzie kupujący adresy e-mail i hasło, aby przetestować aplikację przykładową Wingtip Toys na końcu tego samouczka.

    ![Finalizacja zakupu i płatność w systemie PayPal — piaskownicy testowe konta](checkout-and-payment-with-paypal/_static/image16.png)
7. Tworzenie konta testowego nabywców, klikając **Utwórz konto** przycisku.  
 **Konta testowania piaskownicy** zostanie wyświetlona strona. 

    ![Finalizacja zakupu i płatność w systemie PayPal — PaylPal kont](checkout-and-payment-with-paypal/_static/image17.png)
8. Na **piaskownicy testowe konta** kliknij **facylitatora** konto e-mail.  
    **Profil** i **powiadomień** opcje są wyświetlane.
9. Wybierz **profilu** opcji, a następnie kliknij przycisk **poświadczenia API** Aby wyświetlić poświadczenia interfejsu API dla konta handlowca testu.
10. Skopiuj poświadczenia testowanie interfejsu API do Notatnika.

Konieczne będzie wyświetlany poświadczenia klasycznego interfejsu API testu (nazwa użytkownika, hasło i podpis), które wykonywania wywołań interfejsu API z przykładowej aplikacji Wingtip Toys PayPal środowiska testowego. Poświadczenia zostaną dodane w następnym kroku.

### <a name="add-paypal-class-and-api-credentials"></a>Dodaj klasę PayPal i poświadczenia interfejsu API

Spowoduje umieszczenie większość kodu PayPal w jednej klasie. Ta klasa zawiera metody służące do komunikowania się z systemu PayPal. Ponadto doda poświadczenia PayPal do tej klasy.

1. W aplikacji przykładowej aplikacji Wingtip Toys w programie Visual Studio, kliknij prawym przyciskiem myszy **logiki** folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. W obszarze **Visual C#** z **zainstalowane** w okienku po lewej stronie, wybierz opcję **kodu**.
3. W środkowym okienku wybierz **klasy**. Nazwa ta nowa klasa **PayPalFunctions.cs**.
4. Kliknij przycisk **Dodaj**.  
   Plik nowej klasy jest wyświetlany w edytorze.
5. Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Dodaj poświadczenia handlowca interfejsu API (nazwa użytkownika, hasło i podpis), które wyświetlane we wcześniejszej części tego samouczka, aby można było wywołania funkcji do środowiska testowego systemu PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> W tej przykładowej aplikacji są po prostu dodawania poświadczeń do pliku C# (CS). Jednak w rozwiązaniu zaimplementowano, należy rozważyć szyfrowanie poświadczeń w pliku konfiguracji.


Klasa NVPAPICaller zawiera obsługę większości funkcji PayPal. Kod w klasie zapewnia metody potrzebne do dokonywania zakupów w środowisku testowym PayPal testu. Następujące trzy funkcje PayPal są używane do robienia zakupów:

- `SetExpressCheckout` — Funkcja
- `GetExpressCheckoutDetails` — Funkcja
- `DoExpressCheckoutPayment` — Funkcja

`ShortcutExpressCheckout` Metoda zbiera szczegóły informacji i produktu zakupu testu z koszyka zakupów i wywołania `SetExpressCheckout` funkcji PayPal. `GetCheckoutDetails` Metoda potwierdza szczegóły zakupu i wywołania `GetExpressCheckoutDetails` funkcji PayPal przed dokonaniem zakupu testu. `DoCheckoutPayment` Ukończeniu metody zakupu testów w środowisku testowym, wywołując `DoExpressCheckoutPayment` funkcji PayPal. Pozostały kod obsługuje metody płatności PayPal i procesu, takie jak kodowanie ciągów, dekodowania ciągów, przetwarzanie tablic i określanie poświadczeń.

> [!NOTE] 
> 
> PayPal umożliwia dołączanie szczegóły zakupu opcjonalne, na podstawie [specyfikacji interfejsu API PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Rozszerzając kodu w przykładowej aplikacji Wingtip Toys, może zawierać szczegóły lokalizacji, opisy produktów, podatku, numer usługi klienta, a także wiele innych pól opcjonalnych.


Należy zauważyć, że adresów URL zwracane i anulowania, które są określone w **ShortcutExpressCheckout** metody używania numeru portu.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Po uruchomieniu projektu sieci web przy użyciu protokołu SSL dla programu Visual Web Developer często port 44300 jest używany dla serwera sieci web. Jak wspomniano powyżej, numer portu jest 44300. Po uruchomieniu aplikacji, można uzyskać inny numer portu. Twoje potrzeby numeru portu należy poprawnie ustawić w kodzie aby można było pomyślne uruchamianie przykładowej aplikacji Wingtip Toys na końcu tego samouczka. Następna sekcji tego samouczka wyjaśniono, jak pobrać numer portu lokalnego hosta i zaktualizować klasy PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Zaktualizuj numer portu LocalHost w klasie PayPal

Przykładowej aplikacji Wingtip Toys zakupów produktów, przechodząc do witryny testowania PayPal i powrocie do lokalnego wystąpienia programu przykładowej aplikacji Wingtip Toys. Aby mogła mieć PayPal, wróć do prawidłowego adresu URL, należy określić numer portu lokalnego uruchamiania przykładowej aplikacji w kodzie PayPal wymienionych powyżej.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**WingtipToys**) w **Eksploratora rozwiązań** i wybierz **właściwości**.
2. W kolumnie po lewej stronie wybierz **Web** kartę.
3. Pobieranie numer portu z **adres Url projektu** pole.
4. W razie potrzeby zaktualizuj `returnURL` i `cancelURL` w klasie PayPal (`NVPAPICaller`) w *PayPalFunctions.cs* plik do użycia numer portu w aplikacji sieci web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Teraz kod, który został dodany będzie zgodna z oczekiwanym portów lokalnych aplikacji sieci Web. PayPal będzie możliwość powrotu do prawidłowego adresu URL, na komputerze lokalnym.

### <a name="add-the-paypal-checkout-button"></a>Dodawanie przycisku PayPal wyewidencjonowania

Teraz, że podstawowe funkcje PayPal zostały dodane do przykładowej aplikacji, możesz rozpocząć dodawanie znaczników i kodu, wymagane do wywołania tych funkcji. Najpierw należy dodać przycisk wyewidencjonowania, który użytkownik zobaczy na stronie Koszyk sklepowy.

1. Otwórz *ShoppingCart.aspx* pliku.
2. Przewiń w dół pliku i Znajdź `<!--Checkout Placeholder -->` komentarz.
3. Zastąp komentarz z `ImageButton` kontrolki narzutu, który zostanie zastąpiony w następujący sposób:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. W *ShoppingCart.aspx.cs* pliku po `UpdateBtn_Click` dodać program obsługi zdarzeń w pobliżu końca pliku, `CheckOutBtn_Click` program obsługi zdarzeń:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Również w *ShoppingCart.aspx.cs* Dodaj odwołanie do `CheckoutBtn`, dzięki czemu przycisku Nowy obraz, mowa w następujący sposób:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Zapisz zmiany w obu *ShoppingCart.aspx* pliku i *ShoppingCart.aspx.cs* pliku.
7. W menu, wybierz opcję **debugowania**-&gt;**kompilacji WingtipToys**.  
   Projekt zostanie ponownie skompilowany przy użyciu nowo dodanych **ImageButton** kontroli.

### <a name="send-purchase-details-to-paypal"></a>Wysyłać szczegóły zakupu PayPal

Kiedy użytkownik kliknie **wyewidencjonowania** przycisk na stronie koszyka (*ShoppingCart.aspx*), ich rozpocznie się proces zakupu. Poniższy kod wywołuje pierwszej funkcji PayPal potrzebne do zakupu produktów.

1. Z *wyewidencjonowania* folder, otwórz plik związany z kodem o nazwie *CheckoutStart.aspx.cs*.   
   Pamiętaj otworzyć plik związany z kodem.
2. Zastąp istniejący kod następujących czynności:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Po kliknięciu przez użytkownika aplikacji **wyewidencjonowania** przycisk na stronie koszyka, przeglądarka spowoduje przejście do *CheckoutStart.aspx* strony. Gdy *CheckoutStart.aspx* stronie załadowanie `ShortcutExpressCheckout` metoda jest wywoływana. W tym momencie użytkownik jest przenoszona do witryny sieci web z testowania PayPal. W witrynie PayPal użytkownika wprowadzenia poświadczeń systemu PayPal, przeglądy szczegóły zakupu, akceptuje Umowę PayPal i zwraca do przykładowej aplikacji Wingtip Toys gdzie `ShortcutExpressCheckout` ukończeniu metody. Gdy `ShortcutExpressCheckout` metoda będzie gotowa, nastąpi przekierowanie użytkownika do *CheckoutReview.aspx* stronę określoną w `ShortcutExpressCheckout` metody. Dzięki temu użytkownikowi Przejrzyj szczegóły zamówienia w przykładowej aplikacji Wingtip Toys.

### <a name="review-order-details"></a>Przejrzyj szczegóły zamówienia

Po powrocie z systemu PayPal, *CheckoutReview.aspx* strony przykładowej aplikacji Wingtip Toys Wyświetla szczegóły zamówienia. Ta strona umożliwia użytkownikowi Przejrzyj szczegóły zamówienia przed dokonaniem zakupu produktów. *CheckoutReview.aspx* strony musi zostać utworzona w następujący sposób:

1. W *wyewidencjonowania* folder, otwórz stronę o nazwie *CheckoutReview.aspx*.
2. Zastąp istniejący kod znaczników następujących czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Otwórz stronę związanym z kodem o nazwie *CheckoutReview.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** formantu służy do wyświetlenia szczegółów zamówienia, które zostały zwrócone z systemu PayPal. Ponadto powyższy kod zapisuje szczegóły zamówienia do bazy danych o nazwie Wingtip Toys jako `OrderDetail` obiektu. Gdy użytkownik kliknie **całe zamówienie** przycisku, zostanie przekierowany do *CheckoutComplete.aspx* strony.

> [!NOTE] 
> 
> **Porada**
> 
> W znaczniku elementu *CheckoutReview.aspx* stronie należy zauważyć, że `<ItemStyle>` umożliwia zmienianie stylu elementów w tagu **DetailsView** formant w pobliżu dolnej części strony. Wyświetlając stronę w **widoku projektu** (wybierając **projektowania** w lewym dolnym rogu programu Visual Studio), a następnie wybierając pozycję **DetailsView** sterowania i wybierając  **Tag inteligentny** (ikonę strzałki w prawym górnym rogu kontrolki), będą mogli zobaczyć **DetailsView zadania**.
> 
> ![Finalizacja zakupu i płatność w systemie PayPal — edytowanie pól](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Wybierając **Edytuj pola**, **pola** zostanie wyświetlone okno dialogowe. W tym oknie można łatwo kontrolować visual właściwości, takie jak **ItemStyle**, z **DetailsView** kontroli.
> 
> ![Finalizacja zakupu i płatność w systemie PayPal — okno dialogowe pola](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Sfinalizuj zakup

*CheckoutComplete.aspx* strony sprawia, że zakupu z systemu PayPal. Jak wspomniano powyżej, użytkownik musi kliknąć **całe zamówienie** przycisk przed aplikacji spowoduje przejście do *CheckoutComplete.aspx* strony.

1. W *wyewidencjonowania* folder, otwórz stronę o nazwie *CheckoutComplete.aspx*.
2. Zastąp istniejący kod znaczników następujących czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Otwórz stronę związanym z kodem o nazwie *CheckoutComplete.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Gdy *CheckoutComplete.aspx* strona została załadowana, `DoCheckoutPayment` metoda jest wywoływana. Jak wspomniano wcześniej, `DoCheckoutPayment` ukończeniu metody zakupu ze środowiska testowania PayPal. Po zatwierdzeniu PayPal zakupu zamówienie, *CheckoutComplete.aspx* strony wyświetli transakcji płatności `ID` nabywcy.

### <a name="handle-cancel-purchase"></a>Dojście do anulowania zakupu

Jeśli użytkownik zdecyduje się na anulowania zakupu w witrynie, zostanie przekierowany do *CheckoutCancel.aspx* strony, gdzie zobaczą, że ich zamówienie zostało anulowane.

1. Otwórz stronę o nazwie *CheckoutCancel.aspx* w *wyewidencjonowania* folderu.
2. Zastąp istniejący kod znaczników następujących czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Obsługa błędów zakupu

Błędy w procesie zakupu będzie obsługiwany przez *CheckoutError.aspx* strony. Kodem z *CheckoutStart.aspx* stronie *CheckoutReview.aspx* strony i *CheckoutComplete.aspx* będzie każdego kierować do  *CheckoutError.aspx* strony, jeśli wystąpi błąd.

1. Otwórz stronę o nazwie *CheckoutError.aspx* w *wyewidencjonowania* folderu.
2. Zastąp istniejący kod znaczników następujących czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* zostanie wyświetlona strona z szczegóły błędu, gdy wystąpi błąd podczas wyewidencjonowania.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację, aby zobaczyć, jak dokonać zakupu produktów. Należy pamiętać, że będzie działać w systemie PayPal środowiska testowego. Wymienianie nie rzeczywiste pieniądze.

1. Upewnij się, że wszystkie pliki są zapisywane w programie Visual Studio.
2. Otwórz przeglądarkę internetową i przejdź do [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Zaloguj się przy użyciu konta dewelopera systemu PayPal, który został utworzony we wcześniejszej części tego samouczka.  
   Dla piaskownicy dla deweloperów PayPal, musisz zalogować się na [ https://developer.paypal.com ](https://developer.paypal.com/) do testowania express wyewidencjonowania. Dotyczy to tylko piaskownicy PayPal, testowania, aby nie środowiska produkcyjnego PayPal.
4. W programie Visual Studio, naciśnij klawisz **F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
   Po odbudowania bazy danych, przeglądarce otworzy się i Pokaż *Default.aspx* strony.
5. Dodaj trzy różne produkty do koszyka, wybranie kategorii produktów, takich jak "Samochody", a następnie klikając polecenie **Dodaj do koszyka** obok każdego produktu.  
   Koszyk sklepowy wyświetli produktu, który wybrano.
6. Kliknij przycisk **PayPal** przycisku do wyewidencjonowania. 

    ![Finalizacja zakupu i płatność w systemie PayPal — koszyka](checkout-and-payment-with-paypal/_static/image20.png)

   Trwa wyewidencjonowywanie wymagają, że masz konto użytkownika dla przykładowej aplikacji Wingtip Toys.
7. Kliknij przycisk **Google** łącza w prawej części strony, aby zalogować się przy użyciu istniejącego konta e-mail gmail.com.  
   Jeśli nie masz konta gmail.com, możesz utworzyć jedną do testowania w [www.gmail.com](https://www.gmail.com/). Umożliwia także standardowe konta lokalnego, klikając przycisk "Zarejestruj". 

    ![Finalizacja zakupu i płatność w systemie PayPal — Zaloguj się](checkout-and-payment-with-paypal/_static/image21.png)
8. Zaloguj się przy użyciu konta gmail i hasła usługi. 

    ![Finalizacja zakupu i płatność w systemie PayPal — logowanie do usługi Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Kliknij przycisk **Zaloguj** przycisk, aby zarejestrować konta gmail z Twoją nazwą użytkownika aplikacji przykładowej aplikacji Wingtip Toys. 

    ![Finalizacja zakupu i płatność w systemie PayPal — Zarejestruj konto](checkout-and-payment-with-paypal/_static/image23.png)
10. W witrynie testu PayPal, Dodaj swoje **kupujący** adres e-mail odbiorcy oraz hasło, który został utworzony we wcześniejszej części tego samouczka, a następnie kliknij przycisk **logowanie** przycisku. 

    ![Finalizacja zakupu i płatność w systemie PayPal — konta PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Wyraź zgodę na zasady PayPal, a następnie kliknij przycisk **Zgadzam się, kontynuuj** przycisku.  
    Należy zauważyć, że ta strona jest tylko wyświetlane po raz pierwszy używasz tego konta PayPal. Ponownie należy pamiętać, że to konto testowe są wymieniane nie rzeczywiste pieniądze. 

    ![Finalizacja zakupu i płatność w systemie PayPal — zasady PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Przejrzyj informacje o kolejności w systemie PayPal testowanie strony Przegląd środowiska i kliknij przycisk **Kontynuuj**. 

    ![Finalizacja zakupu i płatność w systemie PayPal — Przegląd informacji](checkout-and-payment-with-paypal/_static/image26.png)
13. Na *CheckoutReview.aspx* strony, sprawdź kwota zamówienia i wyświetlić adres wysyłkowy wygenerowany. Następnie kliknij przycisk **całe zamówienie** przycisku. 

    ![Finalizacja zakupu i płatność w systemie PayPal — Przegląd zamówienia](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** zostanie wyświetlona strona z identyfikatorem transakcji płatności. 

    ![Finalizacja zakupu i płatność w systemie PayPal — kompletny wyewidencjonowania](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Przegląd bazy danych

Przeglądając zaktualizowane dane w bazie danych aplikacji Wingtip Toys po uruchomieniu aplikacji, zobaczysz, że aplikacja pomyślnie zarejestrowane zakupu produktów.

Możesz sprawdzić dane zawarte w *Wingtiptoys.mdf* plik bazy danych przy użyciu **Eksplorator bazy danych** okna (**Eksploratora serwera** okna w programie Visual Studio) tak jak we wcześniejszej części tej serii samouczków.

1. Zamknij okno przeglądarki, jeśli jest nadal otwarty.
2. W programie Visual Studio, wybierz **Pokaż wszystkie pliki** ikonę u góry **Eksploratora rozwiązań** umożliwia rozwijanie **aplikacji\_danych** folderu.
3. Rozwiń **aplikacji\_danych** folderu.  
 Musisz wybrać **Pokaż wszystkie pliki** ikonę folderu.
4. Kliknij prawym przyciskiem myszy *Wingtiptoys.mdf* plik bazy danych i wybierz pozycję **Otwórz**.  
    **Eksplorator serwera** jest wyświetlana.
5. Rozwiń **tabel** folderu.
6. Kliknij prawym przyciskiem myszy **zamówienia**tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.  
 **Zamówienia** tabela jest wyświetlana.
7. Przegląd **PaymentTransactionID** kolumny, aby potwierdzić pomyślne transakcje. 

    ![Finalizacja zakupu i płatność w systemie PayPal — Przegląd bazy danych](checkout-and-payment-with-paypal/_static/image29.png)
8. Zamknij **zamówienia** okna tabeli.
9. W Eksploratorze serwera, kliknij prawym przyciskiem myszy **OrderDetails** tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.
10. Przegląd `OrderId` i `Username` wartości w **OrderDetails** tabeli. Należy zauważyć, że te wartości są zgodne `OrderId` i `Username` wartości zawarte w **zamówienia** tabeli.
11. Zamknij **OrderDetails** okna tabeli.
12. Kliknij prawym przyciskiem myszy plik bazy danych o nazwie Wingtip Toys (*Wingtiptoys.mdf*) i wybierz **bliskie połączenie**.
13. Jeśli nie widzisz **Eksploratora rozwiązań** okna, kliknij przycisk **Eksploratora rozwiązań** w dolnej części **Eksploratora serwera** do wyświetlenia w oknie **Eksploratora rozwiązań**  ponownie.

## <a name="summary"></a>Podsumowanie

W tym samouczku została dodana, kolejność i schematów szczegółów zamówienia, do śledzenia do zakupu produktów. Możesz również zintegrowane funkcje PayPal przykładowej aplikacji Wingtip Toys.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie konfiguracji platformy ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

Ten samouczek zawiera przykładowy kod. Takie przykładowy kod znajduje się "w jakim jest" bez gwarancji jakiegokolwiek rodzaju. W związku z tym Microsoft nie gwarantuje dokładności, integralność lub jakości kodu przykładowego. Użytkownik zgadza się użyć przykładowego kodu na własne ryzyko. W żadnym wypadku firma Microsoft nie ponosi dla użytkownika w dowolny sposób dla każdego przykładowego kodu, treści, w tym między innymi wszelkie błędy lub pominięć w każdego przykładowego kodu, zawartość, lub straty lub szkody dowolnego rodzaju poniesionym w wyniku użycia każdego przykładowego kodu. Niniejszym użytkownik udziela firmie są powiadamiani i uzgadniają wypłaty, zapisać i zwolnić firmę Microsoft z odpowiedzialności za wszelkie utraty, roszczeniami, strat, szkód lub szkody dowolnego rodzaju uwzględniając, bez ograniczenia, spowodowane lub wynikających z materiałów, w którym publikowane są, przesłanie, użyj lub polegają na tym między innymi, poglądy wyrażone w nim.

> [!div class="step-by-step"]
> [Poprzednie](shopping-cart.md)
> [dalej](membership-and-administration.md)
