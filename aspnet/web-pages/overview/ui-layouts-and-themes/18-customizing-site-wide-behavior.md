---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Dostosowywanie zachowania dla całej witryny dla ASP.NET Web Pages (Razor) witryn | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale wyjaśniono, jak wprowadzić ustawienia całej witryny sieci Web lub cały folder, a nie tylko strony.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 3930c1cceb86e4105463323e0896d7491af5ae56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391935"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Dostosowywanie zachowania dla całej witryny dla witrynach ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak określić ustawienia lokacji po stronie dla stron w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak uruchomić kod umożliwiający zestaw wartości (wartości globalnych lub ustawienia pomocnika) dla wszystkich stron w witrynie.
> - Jak uruchomić kod, który umożliwia ustawienie wartości na wszystkich stronach w folderze.
> - Jak uruchomić kod przed i po stronie ładuje.
> - Jak wysłać błędy do strony błędu centralnej.
> - Jak dodać uwierzytelnianie do wszystkich stron w folderze.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Program WebMatrix 3
> - Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)
>   
> 
> W tym samouczku współpracuje również z 3 stron sieci Web platformy ASP.NET i programu Visual Studio 2013 (lub Visual Studio Express 2013 for Web), chyba że użytkownik nie można użyć bibliotekę pomocników platformy ASP.NET sieci Web.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Dodawanie kodu do uruchomienia witryny sieci Web dla stron sieci Web platformy ASP.NET

Przez większość kodu napisanego w języku ASP.NET Web Pages pojedynczej strony może zawierać kod, który jest wymagany dla tej strony. Na przykład jeśli strona wysyła wiadomość e-mail, istnieje możliwość umieść cały kod dla tej operacji na jednej stronie. Może to obejmować kod do zainicjowania ustawienia wysyłania poczty e-mail (oznacza to, że dla serwera SMTP) i wysyłania wiadomości e-mail.

W niektórych sytuacjach może być do uruchomienia kodu przed uruchomieniem dowolnej stronie w witrynie. Jest to przydatne do ustawiania wartości, które mogą być używane w dowolnym miejscu w witrynie (nazywane *wartości globalnych*.) Na przykład niektóre pomocników wymagają Podaj wartości, takich jak ustawienia poczty e-mail lub kluczy konta. Może być przydatna, aby zachować te ustawienia globalne wartości.

Można to zrobić przez utworzenie strony o nazwie  *\_AppStart.cshtml* w katalogu głównym witryny. Jeśli ta strona istnieje, jest uruchamiany przy pierwszym żądaniu dowolnej strony w witrynie. Dlatego jest dobrym miejscem do uruchomienia kodu, aby ustawić wartości globalnych. (Ponieważ  *\_AppStart.cshtml* ma prefiks podkreślenia ASP.NET nie będzie wysyłać strony w przeglądarce, nawet jeśli użytkownicy zażądają go bezpośrednio.)

Na poniższym diagramie przedstawiono sposób, w jaki  *\_AppStart.cshtml* stronie działa. Gdy nadejdzie żądanie dla strony i jeśli jest to pierwsze żądanie dla każdej strony w witrynie ASP.NET najpierw sprawdza, czy czy  *\_AppStart.cshtml* strona istnieje. Jeśli tak, dowolny kod w  *\_AppStart.cshtml* strony uruchomienia, a następnie uruchamia żądanej strony.

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Ustawienie wartości globalnych dla witryny sieci Web

1. W folderze głównym witryny sieci Web programu WebMatrix, Utwórz plik o nazwie  *\_AppStart.cshtml*. Plik musi być w katalogu głównym witryny.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Ten kod przechowuje wartość w `AppState` słownik, który jest automatycznie dostępny dla wszystkich stron w witrynie. Należy zauważyć, że  *\_AppStart.cshtml* plik nie ma żadnych znaczników w nim. Strona będzie uruchomić kod i Przekierowanie do strony, która pierwotnie żądaną.

    > [!NOTE]
    > Należy zachować ostrożność w umieść kod  *\_AppStart.cshtml* pliku. Jeśli wystąpią błędy w kodzie w  *\_AppStart.cshtml* pliku, witryny sieci Web nie będą naliczane.
3. W folderze głównym, Utwórz nową stronę o nazwie *AppName.cshtml*.
4. Zastąp domyślną znaczników i kodu następujące czynności: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Ten kod wyodrębnianie wartości z `AppState` obiektu, który zostało ustawiony w  *\_AppStart.cshtml* strony.
5. Uruchom *AppName.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Strony wyświetli wartości globalnej. 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Ustawianie wartości dla wątków

Dobrze Wykorzystaj dla  *\_AppStart.cshtml* plik jest do ustawiania wartości dla wątków, które są używane w serwisie i które mają zostać zainicjowany. Typowym przykładem są ustawienia poczty e-mail dla `WebMail` pomocnika i klucze prywatne i publiczne `ReCaptcha` pomocnika. W takich przypadkach można ustawić wartości jeden raz w  *\_AppStart.cshtml* i następnie są już ustawione dla wszystkich stron w Twojej lokacji.

Tej procedury przedstawiono sposób ustawiania `WebMail` ustawienia globalnie. (Aby uzyskać więcej informacji o korzystaniu z `WebMail` pomocnika, zobacz [dodawanie do poczty E-mail witrynie ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie został dodany.
2. Jeśli nie masz jeszcze  *\_AppStart.cshtml* plików, w folderze głównym witryny sieci Web Utwórz plik o nazwie  *\_AppStart.cshtml*.
3. Dodaj następujący kod `WebMail` ustawienia  *\_AppStart.cshtml* pliku: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Zmodyfikuj następujące e-mail powiązane ustawienia w kodzie:

   - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
   - Ustaw `your-user-name-here` nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `your-account-password` hasło dla konta serwera SMTP.
   - Ustaw `your-email-address-here` na adres e-mail. To jest adres e-mail, który komunikat jest wysyłany z. (Niektórzy dostawcy poczty e-mail nie pozwalają określić inną `From` adresem, a następnie użyje nazwy użytkownika jako `From` adresu.)

     Aby uzyskać więcej informacji na temat ustawień protokołu SMTP, zobacz [konfigurowania ustawień poczty E-mail](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) w artykule [wysyłania wiadomości E-mail z witryny ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) i [problemy związane z wysłaniem wiadomości E-mail](https://go.microsoft.com/fwlink/?LinkId=253001#email)w [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Zapisz  *\_AppStart.cshtml* plik i zamknij go.
5. W folderze głównym witryny sieci Web, należy utworzyć nową stronę o nazwie *TestEmail.cshtml*.
6. Zastąp istniejącą zawartość następujących czynności: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Uruchom *TestEmail.cshtml* strony w przeglądarce.
8. Wypełnij pola, aby wysłać do siebie wiadomość e-mail, a następnie kliknij przycisk **wysyłania**.
9. Sprawdź pocztę e-mail, aby upewnić się, że trafiła do Ciebie wiadomość.

Ważną częścią w tym przykładzie jest to, że ustawienia, które zwykle nie została zmieniona — takie jak nazwa serwera SMTP i poświadczenia wiadomości e-mail — są ustawiane w  *\_AppStart.cshtml* pliku. Dzięki temu nie trzeba ich ustawiać ponownie na każdej stronie gdzie wysłać wiadomości e-mail. (Chociaż Jeśli z jakiegoś powodu, musisz zmienić te ustawienia, można ustawić je oddzielnie na stronie.) Na stronie można ustawić tylko wartości, które zazwyczaj zmienia się za każdym razem, takie jak odbiorcy oraz treść wiadomości e-mail.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Uruchamianie kodu przed i po nim pliki w folderze

Podobnie można użyć  *\_AppStart.cshtml* przed uruchomieniem stron witryny, należy napisać kod, można napisać kod, który jest uruchamiany przed (i po nim) dowolnej strony w określonym folderze Uruchom. Jest to przydatne w przypadku elementów, np. ustawienie tej samej strony układu dla wszystkich stron w folderze lub Sprawdzanie, który użytkownik jest zalogowany przed uruchomieniem strony w folderze.

W przypadku stron w szczególności folderów, możesz utworzyć kod w pliku o nazwie  *\_PageStart.cshtml*. Na poniższym diagramie przedstawiono sposób, w jaki  *\_PageStart.cshtml* stronie działa. Gdy nadejdzie żądanie dla strony, ASP.NET najpierw sprawdza  *\_AppStart.cshtml* strony i że uruchomieniu. Następnie aplikacja ASP.NET sprawdza, czy istnieje  *\_PageStart.cshtml* strony, a jeśli tak, który uruchamia. Następnie działa żądanej strony.

Wewnątrz  *\_PageStart.cshtml* strony, można określić miejsce podczas przetwarzania żądanej strony do uruchomienia, umieszczając `RunPage` metody. Dzięki temu można uruchomić kod przed uruchomieniem żądanej strony i następnie ponownie po nim. Jeśli nie podasz `RunPage`, cały kod w  *\_PageStart.cshtml* przebiegów i Żądana strona jest uruchamiana automatycznie.

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

Program ASP.NET pozwala utworzyć hierarchię  *\_PageStart.cshtml* plików. Możesz umieścić  *\_PageStart.cshtml* pliku w katalogu głównym witryny i dowolnego podfolderu. Po żądaniu strony  *\_PageStart.cshtml* pliku działa najważniejsze poziomu (najbardziej zbliżona katalogu głównego witryny), a następnie  *\_PageStart.cshtml* pliku w ciągu następnych podfolder, i tak dalej na dół struktury podfolderów, dopóki żądanie dociera folder, który zawiera żądanej strony. Po wszystkich odpowiednich  *\_PageStart.cshtml* pliki zostały uruchomione, jest uruchamiane w żądanej strony.

Na przykład może zawierać następujących kombinacji  *\_PageStart.cshtml* plików i *Default.cshtml* pliku:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Po uruchomieniu */myfolder/default.cshtml*, zostaną wyświetlone następujące czynności:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Uruchamianie kodu inicjowania dla wszystkich stron w folderze

Dobrze Wykorzystaj dla  *\_PageStart.cshtml* pliki, które jest zainicjowanie tej samej strony układu dla wszystkich plików w jednym folderze.

1. W folderze głównym, Utwórz nowy folder o nazwie *InitPages*.
2. W *InitPages* folderu witryny sieci Web, Utwórz plik o nazwie  *\_PageStart.cshtml* i zastąp go następującym domyślne znaczników i kodu: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. W katalogu głównym witryny sieci Web utwórz folder o nazwie *Shared*.
4. W *Shared* folderze utwórz plik o nazwie  *\_Layout1.cshtml* i zastąp go następującym domyślne znaczników i kodu: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. W *InitPages* folderze utwórz plik o nazwie *Content1.cshtml* i Zastąp istniejącą zawartość następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. W *InitPages* folderze utwórz plik o nazwie *Content2.cshtml* i Zastąp znaczniki domyślne następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Uruchom *Content1.cshtml* w przeglądarce. 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Gdy *Content1.cshtml* stronie przebiegów,  *\_PageStart.cshtml* plików zestawów `Layout` , a także ustawia `PageData["MyBackground"]` na kolor. W *Content1.cshtml*, układ i kolory są stosowane.
8. Wyświetlanie *Content2.cshtml* w przeglądarce. 

    Układ jest taki sam, ponieważ obie strony używają tej samej strony układu i kolorów, jak zainicjować w  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Za pomocą \_PageStart.cshtml do obsługi błędów

Dobre innym na użytek  *\_PageStart.cshtml* pliku polega na utworzeniu sposób obsługi błędy programowania (wyjątki), które mogą występować w dowolnym *.cshtml* strony w folderze. W tym przykładzie przedstawiono jeden ze sposobów.

1. W folderze głównym Utwórz folder o nazwie *InitCatch*.
2. W *InitCatch* folderu witryny sieci Web, Utwórz plik o nazwie  *\_PageStart.cshtml* i Zastąp istniejące znaczników i kodu poniższym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    W tym kodzie spróbujesz jawnie uruchomiony żądanej strony, wywołując `RunPage` metody w ramach `try` bloku. Jeśli wystąpią błędy programowania w żądanej strony, kod wewnątrz `catch` block przebiegów. W tym przypadku kod wykonuje przekierowanie do strony (*Error.cshtml*) i przekazuje nazwę pliku, w którym wystąpił błąd jako część adresu URL. (Utworzysz stronę wkrótce.)
3. W *InitCatch* folderu witryny sieci Web, Utwórz plik o nazwie *Exception.cshtml* i Zastąp istniejące znaczników i kodu poniższym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Do celów tego przykładu wykonujesz na tej stronie celowo tworzy błąd, spróbuj ponownie otworzyć plik bazy danych, który nie istnieje.
4. W folderze głównym, Utwórz plik o nazwie *Error.cshtml* i Zastąp istniejące znaczników i kodu poniższym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Na tej stronie wyrażenia `@Request["source"]` pobiera wartość z adresu URL i wyświetla je.
5. Na pasku narzędzi kliknij **Zapisz**.
6. Uruchom *Exception.cshtml* w przeglądarce. 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Ponieważ wystąpienia błędu w *Exception.cshtml*,  *\_PageStart.cshtml* strona przekierowuje do *Error.cshtml* pliku, który wyświetla komunikat.

    Aby uzyskać więcej informacji na temat wyjątków, zobacz [wprowadzenie do platformy ASP.NET Web Pages programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Za pomocą \_PageStart.cshtml, aby ograniczyć dostęp do folderów

Można również użyć  *\_PageStart.cshtml* plik, aby ograniczyć dostęp do wszystkich plików w folderze.

1. W programie WebMatrix, utworzyć nową witrynę sieci Web, używając **lokacji za pomocą szablonu** opcji.
2. Wybierz z dostępnych szablonów **witryny początkowej**.
3. W folderze głównym Utwórz folder o nazwie *AuthenticatedContent*.
4. W *AuthenticatedContent* folderze utwórz plik o nazwie  *\_PageStart.cshtml* i Zastąp istniejące znaczników i kodu poniższym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Zaczyna się kod, uniemożliwiając wszystkie pliki w folderze pamięci podręcznej. (Jest to wymagane dla scenariuszy, takich jak komputery publiczne, których nie chcesz buforowanych stron jednego użytkownika mają być dostępne dla następnego użytkownika.) Następnie kod określa, czy użytkownik zalogował się na lokacji zanim można wyświetlić dowolnej strony w folderze. Jeśli użytkownik nie jest zalogowany, kod wykonuje przekierowanie do strony logowania. Na stronie logowania może zwrócić użytkownika do strony, która pierwotnie żądaną Jeśli dołączysz wartość ciągu kwerendy o nazwie `ReturnUrl`.
5. Utwórz nową stronę w *AuthenticatedContent* folder o nazwie *Page.cshtml*.
6. Zastąp następujące znaczniki domyślne:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Uruchom *Page.cshtml* w przeglądarce. Kod wykonuje przekierowanie do strony logowania. Musisz się zarejestrować, przed zalogowaniem się. Po został zarejestrowany i zalogować, można przejść do strony i wyświetlić jego zawartość.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Wprowadzenie do programowania z użyciem składni Razor strony sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=251587)
