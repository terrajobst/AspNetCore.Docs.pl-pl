---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Wyewidencjonowywanie i płatności w systemie PayPal | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="checkout-and-payment-with-paypal"></a>Wyewidencjonowywanie i płatności w systemie PayPal
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


Ten przewodnik opisuje sposób modyfikowania Wingtip Toys przykładowej aplikacji do uwzględnienia autoryzacji użytkowników, rejestracji i płatności PayPal. Tylko użytkownicy, którzy są zalogowani ma autoryzacji do zakupu produktów. Funkcje rejestracji wbudowanych użytkownika formularzy sieci Web programu ASP.NET 4.5 szablon projektu zawiera już większość potrzebnych. Spowoduje dodanie funkcji PayPal Express wyewidencjonowania. W tym samouczku można za pomocą projektanta PayPal środowiska, testowania, więc nie zostaną przetransferowane żadne rzeczywiste fundusze. Na koniec samouczka będzie przetestować aplikację, wybierając produktów do dodania do koszyka, klikając przycisk wyewidencjonowania i przesyłania danych do witryny sieci web testowania PayPal. W witrynie sieci web testowania PayPal będzie potwierdzić informacje wysyłki i płatności, a następnie wróć do lokalnych aplikacji przykładowej Wingtip Toys potwierdzenie i ukończenie zakupu.

Obejmuje kilka procesorów doświadczonym płatności innych firm, które specialize zakupów online, że adres skalowalności i zabezpieczeń. Deweloperów platformy ASP.NET, należy rozważyć zalety przy użyciu rozwiązania innej płatności przed Implementowanie zakupów i zakupu rozwiązania.

> [!NOTE] 
> 
> Przykładowa aplikacja Wingtip Toys została zaprojektowana w celu pokazano określonych koncepcji ASP.NET i funkcje dostępne dla deweloperów sieci web ASP.NET. Ta przykładowa aplikacja nie została zoptymalizowana dla wszystkich możliwych okolicznościach dotyczących skalowalności i zabezpieczeń.


## <a name="what-youll-learn"></a>Zawartość:

- Jak ograniczyć dostęp do określonych stron w folderze.
- Jak utworzyć znane koszyk z anonimowego koszyk.
- Jak włączyć protokół SSL dla projektu.
- Jak dodać dostawcy OAuth do projektu.
- Jak używać usługi PayPal zakupu produktów przy użyciu usługi PayPal środowiska testowego.
- Jak wyświetlić szczegóły płatności PayPal w **widoku DetailsView** formantu.
- Jak zaktualizować bazę danych aplikacji Wingtip Toys ze szczegółami uzyskane z usługi PayPal.

## <a name="adding-order-tracking"></a>Dodawanie kolejności śledzenia

W tym samouczku utworzysz dwie nowe klasy do śledzenia danych z zamówienia utworzone przez użytkownika. Klasy będzie śledzić dane dotyczące wysyłania informacji, łącznie zakupu i potwierdzenie płatności.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Dodawanie klasy modelu OrderDetail i kolejności

We wcześniejszej części tego samouczka serii zdefiniowany schemat kategorie produktów, oraz koszyk elementów przez utworzenie `Category`, `Product`, i `CartItem` klas w *modele* folderu. Teraz zostaną dodane dwie nowe klasy do definiowania schematu dla kolejności produktu i szczegółów zamówienia.

1. W **modele** folderu, Dodaj nową klasę o nazwie *Order.cs*.   
   Nowy plik klasy jest wyświetlany w edytorze.
2. Zastąp w kodzie domyślnym następujące czynności:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Dodaj *OrderDetail.cs* klasy do *modele* folderu.
4. Zastąp następujący kod w kodzie domyślnym:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` i `OrderDetail` klasy zawiera schematu do definiowania zakupu i wysyłania informacji o kolejności.

Ponadto należy zaktualizować klasy kontekstu bazy danych, która zarządza klas jednostek i który zapewnia dostęp do danych w bazie danych. Aby to zrobić, doda nowo utworzony kolejności i `OrderDetail` klasy do modeli `ProductContext` klasy.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ProductContext.cs* pliku.
2. Dodaj wyróżniony kod, aby *ProductContext.cs* plików, jak pokazano poniżej:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Jak już wspomniano w tym samouczku, kod w *ProductContext.cs* dodaje plik `System.Data.Entity` przestrzeni nazw, aby mieć dostęp do wszystkich podstawowych funkcji programu Entity Framework. Ta funkcja obejmuje możliwość zapytania, wstawiania, aktualizowania i usuwania danych Praca z silnie typizowanych obiektów. Powyższy kod w `ProductContext` klasa dodaje dostępu programu Entity Framework do nowo dodanego `Order` i `OrderDetail` klasy.

## <a name="adding-checkout-access"></a>Dodawanie dostępu wyewidencjonowania

Przykładowa aplikacja Wingtip Toys umożliwia użytkowników anonimowych przejrzeć i Dodaj do koszyka zakupów produktów. Jednak użytkownicy anonimowi wyboru zakupu produktów, dodawane do koszyka, ich zalogować się do witryny. Po zalogowaniu, mogą uzyskiwać dostęp ograniczony stron aplikacji sieci Web, które obsługują wyewidencjonowanie, a proces zakupu. Te strony ograniczeniami są zawarte w *wyewidencjonowania* folderu aplikacji.

### <a name="add-a-checkout-folder-and-pages"></a>Dodaj Folder realizacji transakcji i strony

Teraz utworzysz *wyewidencjonowania* folderu i stron w nim wyświetlany klienta podczas wyewidencjonowania. Te strony spowoduje zaktualizowanie w dalszej części tego samouczka.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **dodać nowy Folder**. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — nowy Folder](checkout-and-payment-with-paypal/_static/image1.png)
2. Nazwa nowego folderu *wyewidencjonowania*.
3. Kliknij prawym przyciskiem myszy *wyewidencjonowania* folder, a następnie wybierz **Dodaj**-&gt;**nowy element**. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — nowy element](checkout-and-payment-with-paypal/_static/image2.png)
4. **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
5. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie w środkowym okienku wybierz **formularza sieci Web ze stroną wzorcową**i nadaj mu nazwę *CheckoutStart.aspx*. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Dodaj okno dialogowe nowego elementu](checkout-and-payment-with-paypal/_static/image3.png)
6. Jak wcześniej, wybierz *Site.Master* pliku jako strony wzorcowej.
7. Dodaj następujące dodatkowe strony do *wyewidencjonowania* folderu powyższe kroki:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Dodanie pliku Web.config

Dodając nowe *Web.config* pliku na *wyewidencjonowania* folder, można ograniczyć dostęp do wszystkich stron zawartych w folderze.

1. Kliknij prawym przyciskiem myszy *wyewidencjonowania* i wybierz polecenie **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie w środkowym okienku wybierz **pliku konfiguracji sieci Web**, zaakceptuj domyślną nazwę *Web.config*, a następnie wybierz **Dodaj**.
3. Zastąp istniejące zawartości w pliku XML *Web.config* pliku następującym kodem:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Zapisz *Web.config* pliku.

*Web.config* pliku Określa, że wszyscy użytkownicy nieznany aplikacji sieci Web musi mieć dostępu do stron zawartych w *wyewidencjonowania* folderu. Jednak jeśli użytkownik został zarejestrowany konta i jest zalogowany, one będą znanych użytkowników i będą mieć dostęp do stron w *wyewidencjonowania* folderu.

Należy pamiętać, że konfiguracja platformy ASP.NET postępuje hierarchii, ważne jest, gdzie każdy *Web.config* pliku stosuje ustawienia konfiguracji do folderu, który jest i do wszystkich katalogów podrzędnych.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Włącz protokół SSL dla projektu

 Secure Sockets Layer (SSL) to protokół określone, aby umożliwić klientom sieci Web do bezpiecznego komunikowania się przy użyciu szyfrowania i serwerów sieci Web. Gdy protokół SSL nie jest używany, dane przesyłane między klientem a serwerem jest otwarty pakiet wykrywanie wszystkim osobom z fizycznego dostępu do sieci. Ponadto kilka wspólnych schematów uwierzytelniania nie są bezpieczne za pośrednictwem zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie oparte na formularzach Wyślij niezaszyfrowane poświadczeń. Do zabezpieczenia te schematy uwierzytelniania muszą używać protokołu SSL. 

1. W **Eksploratora rozwiązań**, kliknij przycisk **WingtipToys** projektu, a następnie naciśnij klawisz **F4** do wyświetlenia **właściwości** okna.
2. Zmień **SSL włączone** do `true`.
3. Kopiuj **adres URL protokołu SSL** , można jej użyć później.   
 Adres URL protokołu SSL będzie `https://localhost:44300/` chyba, że utworzono wcześniej SSL witryny sieci Web (jak pokazano poniżej).   
    ![Właściwości projektu](checkout-and-payment-with-paypal/_static/image4.png)
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys** projekt i kliknij przycisk **właściwości**.
5. Na karcie po lewej stronie kliknij **Web**.
6. Zmień **adres Url projektu** do używania **adres URL protokołu SSL** zapisany wcześniej.   
    ![Właściwości projektu sieci Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Zapisz stronę naciskając **CTRL + S**.
8. Naciśnij klawisz **Ctrl + F5** do uruchomienia aplikacji. Visual Studio będzie wyświetlana opcja pozwala uniknąć ostrzeżenia dotyczące protokołu SSL.
9. Kliknij przycisk **tak** ufać certyfikatowi SSL programu IIS Express i kontynuować.   
    ![Szczegóły certyfikatu SSL programu IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Zostanie wyświetlone ostrzeżenie zabezpieczeń.
10. Kliknij przycisk **tak** do zainstalowania certyfikatu użytkownika localhost.   
    ![Okno dialogowe ostrzeżenia o zabezpieczeniach](checkout-and-payment-with-paypal/_static/image7.png)  
 Wyświetli się okno przeglądarki.

Teraz można łatwo testować aplikację sieci Web lokalnie przy użyciu protokołu SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Dodawanie dostawcy uwierzytelniania OAuth 2.0

Formularze sieci Web ASP.NET zawiera rozszerzone opcje członkostwa i uwierzytelniania. Te ulepszenia obejmują OAuth. OAuth jest otwarte protokół, który umożliwia bezpieczne autoryzacji w metodzie proste i standard z aplikacji sieci web, mobilnych i klasycznych. Szablon formularzy sieci Web ASP.NET używa OAuth do udostępnienia usługi Facebook, Twitter, Google i Microsoft jako dostawcy uwierzytelniania. Chociaż w tym samouczku jest używany tylko Google jako dostawcy uwierzytelniania, można łatwo zmodyfikować kod, aby użyć każdego z dostawców. Kroki w celu wykonania innych dostawców są bardzo podobne do kroków, które będą wyświetlane w tym samouczku.

Oprócz uwierzytelniania samouczka będą też używać do implementowania autoryzacji ról. Tylko użytkownicy dodać do `canEdit` rola będzie mógł zmienić danych (Tworzenie, edytowanie lub usuwanie kontaktów).

> [!NOTE] 
> 
> Aplikacje Windows Live akceptować tylko na żywo adres URL witryny sieci Web pracy, więc nie można używać adresu URL lokalną witrynę sieci Web do testowania logowania.


Poniższe kroki pozwoli dodać dostawcę uwierzytelniania serwisu Google.

1. Otwórz *aplikacji\_Start\Startup.Auth.cs* pliku.
2. Usuń znaki komentarza z `app.UseGoogleAuthentication()` metody, aby metody ma postać następuje: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/). Należy również do logowania z kontem Google developer poczty e-mail (gmail.com). Jeśli nie masz konta Google, wybierz **utworzyć konto** łącza.   
   Następnie zostanie wyświetlony **konsoli deweloperów Google**.   
    ![Konsola deweloperów Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Kliknij przycisk **tworzenia projektu** przycisk, a następnie wprowadź nazwę projektu i identyfikator (należy użyć wartości domyślnych). Następnie kliknij przycisk **wyboru umowy** i **Utwórz** przycisku.  

    ![Google — nowy projekt](checkout-and-payment-with-paypal/_static/image9.png)

   W ciągu kilku sekund zostanie utworzony nowy projekt i przeglądarka wyświetli nową stronę projektów.
5. Na karcie po lewej stronie kliknij **interfejsów API &amp; uwierzytelniania**, a następnie kliknij przycisk **poświadczenia**.
6. Kliknij przycisk **Utwórz nowy identyfikator klienta** w obszarze **OAuth**.   
   **Utworzyć identyfikator klienta** zostanie wyświetlone okno dialogowe.   
    ![Google — Utwórz identyfikator klienta](checkout-and-payment-with-paypal/_static/image10.png)
7. W **utworzyć identyfikator klienta** okna dialogowego, zachowaj ustawienie domyślne **aplikacji sieci Web** typu aplikacja.
8. Ustaw **autoryzowany źródeł JavaScript** do adresu URL protokołu SSL używany we wcześniejszej części tego samouczka (`https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL).   
   Ten adres URL jest punkt początkowy aplikacji. Dla tego przykładu zostanie tylko wprowadź adres URL testu localhost. Można jednak wprowadzić wiele adresów URL dla hosta lokalnego i produkcji.
9. Ustaw **autoryzowany identyfikator URI przekierowania** do następującego: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Ta wartość jest identyfikatorem URI tego OAuth ASP.NET użytkowników do komunikacji z serwerem programu google OAuth. Należy pamiętać, adres URL protokołu SSL używany powyżej ( `https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL).
10. Kliknij przycisk **utworzyć identyfikator klienta** przycisku.
11. W menu po lewej stronie konsoli deweloperów Google, kliknij przycisk **ekranu zgody** element menu, a następnie ustaw nazwie e-mail adres i produktu. Po wypełnieniu formularza, kliknij przycisk **zapisać**.
12. Kliknij przycisk **interfejsów API** element menu, przewiń w dół i kliknij przycisk **poza** znajdujący się obok **interfejsu API Google +**.   
    Akceptowanie tej opcji spowoduje włączenie interfejsu API Google +.
13. Należy również zaktualizować **Microsoft.Owin** pakiet NuGet do wersji 3.0.0.   
    Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** , a następnie wybierz **Zarządzaj pakietami NuGet dla rozwiązania**.  
    Z **Zarządzaj pakietami NuGet** okna, Znajdź i aktualizacji **Microsoft.Owin** pakietu do wersji 3.0.0.
14. W programie Visual Studio, należy zaktualizować `UseGoogleAuthentication` metody *Startup.Auth.cs* strony przez kopiowanie i wklejanie **identyfikator klienta** i **klucz tajny klienta** do metody. **Identyfikator klienta** i **klucz tajny klienta** poniższe wartości są przykłady i nie będzie działać. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Naciśnij klawisz **CTRL + F5** Aby skompilować i uruchomić aplikację. Kliknij przycisk **Zaloguj** łącza.
16. W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.  
    ![Zaloguj się](checkout-and-payment-with-paypal/_static/image11.png)
17. Jeśli musisz wprowadzić swoje poświadczenia, nastąpi przekierowanie do witryny google, w którym należy wprowadzić poświadczenia.  
    ![Google — logowanie](checkout-and-payment-with-paypal/_static/image12.png)
18. Po wprowadzeniu poświadczeń, pojawi się monit Aby udzielić uprawnień do nowo utworzonej aplikacji sieci web.  
    ![Projekt domyślnego konta usługi](checkout-and-payment-with-paypal/_static/image13.png)
19. Kliknij przycisk **zaakceptować**. Teraz nastąpi przekierowanie do **zarejestrować** strony **WingtipToys** aplikacji, w którym można zarejestrować konto Google.  
    ![Zarejestruj konto Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz pozostawić domyślny alias e-mail (to znaczy aktualnie używany do uwierzytelniania). Kliknij przycisk **Zaloguj** zgodnie z powyższym.

### <a name="modifying-login-functionality"></a>Modyfikowanie funkcji logowania

Jak już wspomniano w tym samouczku wiele funkcji rejestracji użytkownika zostało uwzględnione w szablonie formularzy sieci Web ASP.NET domyślnie. Teraz należy zmodyfikować domyślne *Login.aspx* i *Register.aspx* stron do wywołania `MigrateCart` metody. `MigrateCart` Metoda kojarzy nowo zalogowanego użytkownika z anonimowego koszyk. Kojarzenie użytkownika i koszyka, Wingtip Toys przykładowej aplikacji będzie można utrzymać koszyk użytkownika między wizytach.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *konta* folderu.
2. Zmodyfikuj stronę związane z kodem o nazwie *Login.aspx.cs* aby uwzględnić kod wyróżnione kolorem żółtym, aby był on wyświetlany w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Zapisz *Login.aspx.cs* pliku.

Teraz, możesz zignorować to ostrzeżenie, że nie ma żadnych definicji `MigrateCart` metody. Dodasz go nieco dalszej części tego samouczka.

*Login.aspx.cs* plik CodeBehind obsługuje metody logowania. Sprawdzając strony Login.aspx, zobaczysz, że ta strona zawiera przycisk "Zaloguj" że w przypadku kliknij wyzwalaczy `LogIn` obsługi na CodeBehind.

Gdy `Login` metoda *Login.aspx.cs* jest nazywany nowe wystąpienie klasy koszyk o nazwie `usersShoppingCart` jest tworzony. Identyfikator koszyk (GUID) jest pobrać i ustawić `cartId` zmiennej. Następnie `MigrateCart` metoda jest wywoływana z obu przekazywanie `cartId` i nazwa zalogowanego użytkownika do tej metody. Podczas migracji koszyka identyfikator GUID używany do identyfikowania anonimowe koszyk zostanie zastąpiony nazwą tego użytkownika.

Oprócz modyfikowania *Login.aspx.cs* pliku CodeBehind migracji koszyku podczas logowania użytkownika, musisz także zmodyfikować *pliku CodeBehind Register.aspx.cs* migracji koszyka gdy użytkownik tworzy nowe konto i loguje.

1. W *konta* folder, otwórz plik CodeBehind o nazwie *Register.aspx.cs*.
2. Zmodyfikuj plik CodeBehind przez dołączenie kodu do żółty, tak, aby pojawia się w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Zapisz *Register.aspx.cs* pliku. Ponownie, zignorować to ostrzeżenie o `MigrateCart` metody.

Zwróć uwagę, że kod, należy użyć w `CreateUser_Click` program obsługi zdarzeń jest bardzo podobny do kod używany w `LogIn` metody. Gdy użytkownik rejestruje lub loguje się do witryny, wywołanie `MigrateCart` metody zostanie podjęta.

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Teraz, gdy masz proces logowania i rejestracji aktualizacji, można dodać kod, aby przeprowadzić migrację zakupów za pomocą koszyka `MigrateCart` metody.

1. W **Eksploratora rozwiązań**, Znajdź *logiki* folderu i Otwórz *ShoppingCartActions.cs* pliku klasy.
2. Dodaj kod wyróżnione kolorem żółtym istniejący kod w *ShoppingCartActions.cs* plików, dzięki czemu kod w *ShoppingCartActions.cs* pliku wygląda następująco:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` — Metoda korzysta z istniejącej cartId do znalezienia koszyk użytkownika. Następnie kod w pętli wszystkich elementów koszyka zakupów i zastępuje `CartId` właściwości (zgodnie z określonym `CartItem` schematu) o nazwie zalogowanego użytkownika.

### <a name="updating-the-database-connection"></a>Aktualizowanie połączenie z bazą danych

Jeśli korzystasz z tego samouczka przy użyciu **wbudowane** Wingtip Toys przykładowej aplikacji, należy ponownie utworzyć domyślna baza danych członkostwa. Zmieniając domyślnego ciągu połączenia bazy danych członkostwa zostanie utworzony przy następnym uruchomieniu aplikacji.

1. Otwórz *Web.config* pliku w katalogu głównym projektu.
2. Aktualizacja domyślnego ciągu połączenia, aby był on wyświetlany w następujący sposób:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrowanie usługi PayPal

PayPal to platforma rozliczeń opartych na sieci web akceptujący płatności przez sprzedawców online. W tym samouczku obok wyjaśniono, jak zintegrować PayPal Express wyewidencjonowania funkcjonalności aplikacji. Express wyewidencjonowania pozwala klientom korzystanie z usługi PayPal płacisz za elementy dodane do ich koszyk.

### <a name="create-paylpal-test-accounts"></a>Tworzenie kont PaylPal testu

Aby korzystać z usługi PayPal, w środowisku testowym, należy utworzyć i zweryfikować konto dewelopera testu. Konto dewelopera testu użyje do utworzenia kupujący testowe konto oraz konto sprzedawcy testu. Poświadczenia konta dewelopera testu będzie również umożliwiać Wingtip Toys przykładowej aplikacji uzyskiwać dostęp do środowiska testowego PayPal.

1. W przeglądarce przejdź do deweloperów PayPal testowania witryny:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Jeśli nie masz konta dewelopera systemu PayPal, należy utworzyć nowe konto, klikając **Utwórz konto**i wykonując kroki tworzenia konta. Jeśli masz istniejące konto dewelopera PayPal, zaloguj się, klikając **dziennika w**. Konieczne będzie konto dewelopera PayPal, aby przetestować aplikację przykładową Wingtip Toys w dalszej części tego samouczka.
3. Użytkownik po prostu jest zarejestrowany dla konta dewelopera systemu PayPal, konieczne może zweryfikować konta dewelopera systemu PayPal z PayPal. Aby zweryfikować swoje konto, należy zgodnie z krokami, które PayPal wysyłane do swojego konta poczty e-mail. Po upewnieniu się, konto PayPal dewelopera, zaloguj się do testowania witryny dewelopera systemu PayPal.
4. Po zalogowaniu się do witryny dewelopera PayPal z konta dewelopera systemu PayPal, musisz utworzyć konto PayPal nabywców testu, jeśli użytkownik nie jest już jeszcze raz. Aby utworzyć konto testu nabywców kliknij witrynę PayPal **aplikacji** a następnie kliknij pozycję **kont piaskownicy**.   
 **Piaskownicy testowe konta** wyświetlana jest strona.   

    > [!NOTE] 
    > 
    > PayPal deweloperskiej zawiera już konto handlowe testu.

    ![Wyewidencjonowywanie i płatności w systemie PayPal — piaskownicy testowe konta](checkout-and-payment-with-paypal/_static/image15.png)
5. Na stronie piaskownicy testu konta kliknij **Utwórz konto**.
6. Na **Utwórz testowe konto** strony wybierz kupujący testowa wiadomość e-mail z konta i hasło wybranych przez użytkownika.   

    > [!NOTE] 
    > 
    > Konieczne będzie nabywców adresy e-mail i hasło, aby przetestować aplikację przykładową Wingtip Toys na końcu tego samouczka.

    ![Wyewidencjonowywanie i płatności w systemie PayPal — piaskownicy testowe konta](checkout-and-payment-with-paypal/_static/image16.png)
7. Tworzenie konta testowego nabywców, klikając **Utwórz konto** przycisku.  
 **Piaskownicy testowe konta** zostanie wyświetlona strona. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — PaylPal kont](checkout-and-payment-with-paypal/_static/image17.png)
8. Na **piaskownicy testowe konta** kliknij przycisk **pośrednik w przemieszczaniu** konto e-mail.  
    **Profil** i **powiadomień** opcje są wyświetlane.
9. Wybierz **profilu** opcji, a następnie kliknij przycisk **poświadczenia interfejsu API** do wyświetlenia interfejsu API poświadczeń dla konta handlowe testu.
10. Skopiuj poświadczenia TESTUJ interfejs API do Notatnika.

Konieczne będzie wyświetlane poświadczenia klasycznego interfejsu API TEST (nazwa użytkownika, hasło i podpis) wywołań interfejsu API z przykładowej aplikacji Wingtip Toys PayPal środowiska testowego. Poświadczenia zostaną dodane w następnym kroku.

### <a name="add-paypal-class-and-api-credentials"></a>Dodawanie klasy PayPal i poświadczenia interfejsu API

Większość kodu PayPal w jednej klasie zostaną umieszczone. Ta klasa zawiera metody używane do komunikacji z PayPal. Ponadto swoje poświadczenia usługi PayPal zostaną dodane do tej klasy.

1. W przykładowej Wingtip Toys aplikacji w programie Visual Studio, kliknij prawym przyciskiem myszy **logiki** folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. W obszarze **Visual C#** z **zainstalowana** w okienku po lewej stronie, wybierz opcję **kod**.
3. W środkowym okienku wybierz **klasy**. Nazwa ta nowa klasa **PayPalFunctions.cs**.
4. Kliknij przycisk **Dodaj**.  
   Nowy plik klasy jest wyświetlany w edytorze.
5. Zastąp następujący kod w kodzie domyślnym:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Dodaj poświadczenia sprzedawcy interfejsu API (nazwa użytkownika, hasło i podpis), wyświetlane we wcześniejszej części tego samouczka, tak aby można było wprowadzać wywołania funkcji PayPal środowiska testowego.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> W tej przykładowej aplikacji są po prostu dodawania poświadczeń do pliku C# (CS). Jednak w rozwiązaniu zaimplementowany, należy rozważyć szyfrowania poświadczeń w pliku konfiguracji.


Klasa NVPAPICaller zawiera większość funkcji PayPal. Kod w klasie udostępnia metody potrzebne do podejmowania testu kupić od środowiska testowego PayPal. Następujących trzech funkcji PayPal są używane do dokonywanie zakupów:

- `SetExpressCheckout` Funkcja
- `GetExpressCheckoutDetails` Funkcja
- `DoExpressCheckoutPayment` Funkcja

`ShortcutExpressCheckout` — Metoda zbiera szczegóły informacji i produktu zakupu testu z koszyka zakupów i wywołania `SetExpressCheckout` funkcji PayPal. `GetCheckoutDetails` Metody potwierdza szczegóły zakupu i wywołania `GetExpressCheckoutDetails` funkcji PayPal przed dokonaniem zakupu testu. `DoCheckoutPayment` Metoda wykonuje zakupu testów w środowisku testowym przez wywołanie metody `DoExpressCheckoutPayment` funkcji PayPal. Pozostały kod obsługuje metody płatności PayPal i procesu, taką jak kodowanie ciągów i dekodowania ciągów, przetwarzania tablic oraz określania poświadczeń.

> [!NOTE] 
> 
> PayPal umożliwia zawierają szczegóły zakupu opcjonalne na podstawie [specyfikacja interfejsu API PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Rozszerzając kodu w Wingtip Toys przykładowej aplikacji mogą obejmować szczegóły lokalizacji, opisy produktów, podatku, numer usługi klienta, a także wiele innych pól opcjonalnych.


Zwróć uwagę, że adresy URL zwracane i Anuluj, które są określone w **ShortcutExpressCheckout** metody używany numer portu.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Po uruchomieniu programu Visual Web Developer projektu sieci web przy użyciu protokołu SSL często port 44300 jest używany przez serwer sieci web. Jak pokazano powyżej, numer portu jest 44300. Po uruchomieniu aplikacji, można wyświetlić inny numer portu. Potrzeb numer portu określone poprawnie w kodzie tak, aby można było pomyślnie uruchomić Wingtip Toys przykładowej aplikacji na końcu tego samouczka. W następnej sekcji w tym samouczku wyjaśniono, jak pobrać numer portu lokalnego hosta i zaktualizować klasy PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Numer portu LocalHost w klasie PayPal aktualizacji

Wingtip Toys przykładowej aplikacji Zakupy produktów, przechodząc do witryny testowania PayPal i zwracany do lokalnego wystąpienia programu Wingtip Toys przykładowej aplikacji. Aby przypisać PayPal, wróć do poprawny adres URL, należy określić numer portu uruchomionych lokalnie przykładową aplikację w kodzie PayPal wymienionych powyżej.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**WingtipToys**) w **Eksploratora rozwiązań** i wybierz **właściwości**.
2. W kolumnie po lewej stronie wybierz **Web** kartę.
3. Numer portu z pobrać **adres Url projektu** pole.
4. W razie potrzeby zaktualizować `returnURL` i `cancelURL` w klasie PayPal (`NVPAPICaller`) w *PayPalFunctions.cs* plik używany numer portu aplikacji sieci web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Teraz kod, który został dodany będzie zgodna z oczekiwanym portów lokalnych aplikacji sieci Web. PayPal będzie można wrócić do poprawny adres URL na komputerze lokalnym.

### <a name="add-the-paypal-checkout-button"></a>Dodawanie przycisku PayPal wyewidencjonowania

Teraz, podstawowe funkcje PayPal zostały dodane do przykładowej aplikacji, możesz rozpocząć dodawanie znaczników i kodu potrzebnych do wywoływania tych funkcji. Najpierw należy dodać przycisk wyewidencjonowania, który użytkownik zostanie wyświetlony na stronie koszyka zakupów.

1. Otwórz *ShoppingCart.aspx* pliku.
2. Przewiń w dół pliku i Znajdź `<!--Checkout Placeholder -->` komentarza.
3. Zastąp komentarz z `ImageButton` , aby narzutu zostanie zastąpiony w następujący sposób:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. W *ShoppingCart.aspx.cs* plików po `UpdateBtn_Click` Dodaj program obsługi zdarzeń zbliża się koniec pliku, `CheckOutBtn_Click` program obsługi zdarzeń:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Również w *ShoppingCart.aspx.cs* plików, Dodaj odwołanie do `CheckoutBtn`, dzięki czemu przycisk Nowy obraz jest określany w następujący sposób:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Zapisz zmiany w obu *ShoppingCart.aspx* pliku i *ShoppingCart.aspx.cs* pliku.
7. Wybierz z menu **debugowania**-&gt;**kompilacji WingtipToys**.  
   Projekt zostanie odbudowany z nowo dodanego **ImageButton** formantu.

### <a name="send-purchase-details-to-paypal"></a>Szczegóły zakupu wysyłania do systemu PayPal

Po kliknięciu przez użytkownika **wyewidencjonowania** przycisk na stronie koszyka (*ShoppingCart.aspx*), zostaną one rozpocząć proces zakupu. Poniższy kod wywołuje pierwszej funkcji PayPal potrzebne do zakupu produktów.

1. Z *wyewidencjonowania* folder, otwórz plik CodeBehind o nazwie *CheckoutStart.aspx.cs*.   
   Pamiętaj otworzyć plik CodeBehind.
2. Zastąp istniejący kod poniżej:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Po kliknięciu przez użytkownika aplikacja **wyewidencjonowania** przycisk na stronie koszyka, przeglądarka przejdzie do *CheckoutStart.aspx* strony. Gdy *CheckoutStart.aspx* strony obciążeń `ShortcutExpressCheckout` metoda jest wywoływana. W tym momencie użytkownik zostanie przeniesiony do witryny sieci web testowania PayPal. W witrynie PayPal użytkownika wprowadzenia poświadczeń PayPal, przegląda szczegóły zakupu, akceptuje umowy PayPal i zwraca do aplikacji przykładowej Wingtip Toys gdzie `ShortcutExpressCheckout` ukończeniu metody. Gdy `ShortcutExpressCheckout` metody została ukończona, nastąpi przekierowanie użytkownika do *CheckoutReview.aspx* stronę określoną w `ShortcutExpressCheckout` metody. Dzięki temu użytkownikowi Przejrzyj szczegóły kolejności z aplikacji przykładowej Wingtip Toys.

### <a name="review-order-details"></a>Przejrzyj szczegóły kolejności

Po powrocie z PayPal, *CheckoutReview.aspx* Wingtip Toys przykładowej aplikacji, zostanie wyświetlona szczegółów zamówienia. Ta strona umożliwia użytkownikowi przeglądanie szczegółów zamówienia przed zakupem produktów. *CheckoutReview.aspx* strony należy utworzyć w następujący sposób:

1. W *wyewidencjonowania* folder, otwórz stronę o nazwie *CheckoutReview.aspx*.
2. Zastąp istniejący kod znaczników następujące czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Otwórz stronę związane z kodem o nazwie *CheckoutReview.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**Widoku DetailsView** kontroli służy do wyświetlenia szczegółów zamówienia, które zostały zwrócone z PayPal. Ponadto powyższy kod zapisuje szczegółów zamówienia Wingtip Toys bazy danych w postaci `OrderDetail` obiektu. Gdy użytkownik kliknie **całe zamówienie** przycisku, zostanie przekierowany do *CheckoutComplete.aspx* strony.

> [!NOTE] 
> 
> **Porada**
> 
> W znaczniku danego *CheckoutReview.aspx* strony, zwróć uwagę, że `<ItemStyle>` tag jest używany do Zmień styl elementów w obrębie **widoku DetailsView** kontroli w dolnej części strony. Wyświetlając stronę w **widoku Projekt** (wybierając **projekt** w lewym dolnym rogu Visual Studio), wybierając **widoku DetailsView** sterowania i wybierając  **Tag inteligentny** (ikonę strzałki w górnej rogu formantu), będzie mógł wyświetlić **zadania w widoku DetailsView**.
> 
> ![Wyewidencjonowywanie i płatności w systemie PayPal — Edytuj pola](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Wybierając **Edytuj pola**, **pola** zostanie wyświetlone okno dialogowe. W tym oknie dialogowym można łatwo zarządzać visual właściwości, takie jak **ItemStyle**, z **widoku DetailsView** formantu.
> 
> ![Wyewidencjonowywanie i płatności w systemie PayPal — okno dialogowe pola](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Ukończ zakupu

*CheckoutComplete.aspx* strony sprawia, że zakupu z PayPal. Jak wspomniano powyżej, użytkownik musi kliknąć na **całe zamówienie** przycisku, zanim aplikacja zostanie przejdź do *CheckoutComplete.aspx* strony.

1. W *wyewidencjonowania* folder, otwórz stronę o nazwie *CheckoutComplete.aspx*.
2. Zastąp istniejący kod znaczników następujące czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Otwórz stronę związane z kodem o nazwie *CheckoutComplete.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Gdy *CheckoutComplete.aspx* załadowanej strony `DoCheckoutPayment` metoda jest wywoływana. Jak wspomniano wcześniej, `DoCheckoutPayment` metody sfinalizuje zakup ze środowiska testowego PayPal. Po ukończeniu zakupu zlecenia, PayPal *CheckoutComplete.aspx* transakcji płatniczych zostanie wyświetlona strona `ID` kupującemu.

### <a name="handle-cancel-purchase"></a>Obsługa anulowania zakupu

Jeśli użytkownik zdecyduje się na anulowanie zakupu, zostanie skierowany do *CheckoutCancel.aspx* strony, gdy zobaczą, ich kolejność została anulowana.

1. Otwórz stronę o nazwie *CheckoutCancel.aspx* w *wyewidencjonowania* folderu.
2. Zastąp istniejący kod znaczników następujące czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Obsługa błędów zakupu

Błędy podczas procesu zakupu będzie obsługiwany przez *CheckoutError.aspx* strony. Kodem z *CheckoutStart.aspx* strony, *CheckoutReview.aspx* stronę i *CheckoutComplete.aspx* strony każdego przekieruje do  *CheckoutError.aspx* strony w przypadku wystąpienia błędu.

1. Otwórz stronę o nazwie *CheckoutError.aspx* w *wyewidencjonowania* folderu.
2. Zastąp istniejący kod znaczników następujące czynności:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* zostanie wyświetlona strona z szczegóły błędu, gdy wystąpi błąd podczas wyewidencjonowania.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację na temat sposobu zakupu produktów. Należy pamiętać, że będzie działać w PayPal środowiska testowego. Wymieniane bez rzeczywistego pieniędzy.

1. Upewnij się, że wszystkie pliki są zapisywane w programie Visual Studio.
2. Otwórz przeglądarkę sieci Web i przejdź do [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Zaloguj się za pomocą konta dewelopera PayPal, który został utworzony we wcześniejszej części tego samouczka.  
   Piaskownica developer PayPal, musisz zalogować się na [ https://developer.paypal.com ](https://developer.paypal.com/) do testowania wyewidencjonowania express. Dotyczy to tylko piaskownicy PayPal testowania nie do środowiska produkcyjnego PayPal.
4. W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
   Po odbudowania bazy danych, w przeglądarce zostanie otworzyć i Pokaż *Default.aspx* strony.
5. Dodaj do koszyka trzy różne produkty Wybieranie kategorii produktów, takie jak "Samochodów", a następnie klikając polecenie **Dodaj do koszyka** obok każdego produktu.  
   Moduł koszyka zakupów wyświetli produktu, który wybrano.
6. Kliknij przycisk **PayPal** przycisku do wyewidencjonowania. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — koszyka](checkout-and-payment-with-paypal/_static/image20.png)

   Wyewidencjonowywanie wymaga posiadania konta użytkownika dla aplikacji przykładowej Wingtip Toys.
7. Kliknij przycisk **Google** łącza po prawej stronie do logowania się przy użyciu istniejącego konta e-mail gmail.com.  
   Jeśli nie masz konta gmail.com, możesz utworzyć jedną podczas testowania w [www.gmail.com](https://www.gmail.com/). Umożliwia także standardowe konto lokalne, klikając pozycję "Zarejestruj". 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Zaloguj](checkout-and-payment-with-paypal/_static/image21.png)
8. Zaloguj się przy użyciu swojego konta usługi gmail i hasło. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Gmail logowania](checkout-and-payment-with-paypal/_static/image22.png)
9. Kliknij przycisk **Zaloguj** przycisk, aby zarejestrować konto usługi gmail z nazwą użytkownika Wingtip Toys przykładowej aplikacji. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — konta rejestru](checkout-and-payment-with-paypal/_static/image23.png)
10. W witrynie testu PayPal, Dodaj użytkownika **nabywców** e-mail adres i hasło, które utworzono we wcześniejszej części tego samouczka, a następnie kliknij przycisk **dziennika w** przycisku. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — PayPal logowania](checkout-and-payment-with-paypal/_static/image24.png)
11. Zgodę na zasady PayPal i kliknij przycisk **Zgadzam się i Kontynuuj** przycisku.  
    Należy pamiętać, że ta strona jest tylko wyświetlane po raz pierwszy używasz tego konta PayPal. Ponownie należy pamiętać, że jest to konto testu nie pieniędzy są wymieniane. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — PayPal zasad](checkout-and-payment-with-paypal/_static/image25.png)
12. Przejrzeć informacje o kolejności na PayPal testowanie stronę przeglądu środowiska i kliknij przycisk **Kontynuuj**. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Przegląd informacji](checkout-and-payment-with-paypal/_static/image26.png)
13. Na *CheckoutReview.aspx* , sprawdź wartość kolejności i Wyświetl adres wysyłkowy wygenerowany. Następnie kliknij przycisk **całe zamówienie** przycisku. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Przegląd kolejności](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** zostanie wyświetlona strona z identyfikatorem transakcji płatniczych 

    ![Transakcji i płatności z PayPal — pełny wyewidencjonowania](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Przegląd bazy danych

Przeglądając zaktualizowanych danych w bazie danych aplikacji Wingtip Toys po uruchomieniu aplikacji, widać, że aplikacja pomyślnie zarejestrowane zakupu produktów.

Możesz sprawdzić dane zawarte w *Wingtiptoys.mdf* pliku bazy danych przy użyciu **Eksploratora bazy danych** okna (**Eksploratora serwera** okna w programie Visual Studio) tak samo jak wcześniej w tym samouczku.

1. Zamknij okno przeglądarki, jeśli jest nadal otwarty.
2. W programie Visual Studio, wybierz **Pokaż wszystkie pliki** ikony na początku **Eksploratora rozwiązań** pozwala rozwinąć **aplikacji\_danych** folderu.
3. Rozwiń węzeł **aplikacji\_danych** folderu.  
 Musisz wybrać **Pokaż wszystkie pliki** ikonę folderu.
4. Kliknij prawym przyciskiem myszy *Wingtiptoys.mdf* pliku bazy danych i wybierz **Otwórz**.  
    **W Eksploratorze serwera** jest wyświetlany.
5. Rozwiń węzeł **tabel** folderu.
6. Kliknij prawym przyciskiem myszy **zamówień**tabeli i wybierz **Pokaż dane tabeli**.  
 **Zamówień** tabela jest wyświetlana.
7. Przegląd **PaymentTransactionID** kolumny, aby potwierdzić pomyślne transakcje. 

    ![Wyewidencjonowywanie i płatności w systemie PayPal — Przegląd bazy danych](checkout-and-payment-with-paypal/_static/image29.png)
8. Zamknij **zamówień** okna tabeli.
9. W Eksploratorze serwera, kliknij prawym przyciskiem myszy **SzczegółyZamówienia** tabeli i wybierz **Pokaż dane tabeli**.
10. Przegląd `OrderId` i `Username` wartości w **SzczegółyZamówienia** tabeli. Należy pamiętać, że te wartości są takie same `OrderId` i `Username` wartości uwzględnionych **zamówień** tabeli.
11. Zamknij **SzczegółyZamówienia** okna tabeli.
12. Kliknij prawym przyciskiem myszy plik bazy danych Wingtip Toys (*Wingtiptoys.mdf*) i wybierz **bliskie połączenie**.
13. Jeśli nie widzisz **Eksploratora rozwiązań** okna, kliknij przycisk **Eksploratora rozwiązań** w dolnej części **Eksploratora serwera** do wyświetlenia w oknie **Eksploratora rozwiązań**  ponownie.

## <a name="summary"></a>Podsumowanie

W tym samouczku dodać kolejności i schematy szczegółów zamówienia śledzenia zakupu produktów. Można również zintegrować funkcji PayPal Wingtip Toys przykładowej aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przegląd konfiguracji ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, OAuth i bazy danych SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

Ten samouczek zawiera przykładowy kod. Takie przykładowy kod jest dostarczany "w jakim jest" bez jakichkolwiek gwarancji. W związku z tym Microsoft nie gwarantuje dokładność, integralność lub jakości przykładowy kod. Użytkownik zobowiązuje się używać przykładowy kod na własne ryzyko. W żadnym wypadku będzie Microsoft nie ponosi odpowiedzialności do Ciebie w żaden sposób żadnych przykładowy kod, zawartość, między innymi do błędów lub przeoczenia żadnych przykładowy kod, zawartości, lub dowolnego utratę lub uszkodzenie dowolnego rodzaju wynikające z użycia żadnych przykładowy kod. Jest powiadamiany, a zgadzają się wynagradzać, Zapisz i przytrzymaj z odpowiedzialności za straty wszystkie, oświadczenia straty, szkody lub uszkodzenia dowolnego rodzaju tym, bez ograniczenia, spowodowane lub wynikających z materiału, który ogłasza, Microsoft przekazuje, użyj lub polegać na tym, ale nie wyłącznie, poglądy wyrażone w nim.

> [!div class="step-by-step"]
> [Poprzednie](shopping-cart.md)
> [dalej](membership-and-administration.md)
