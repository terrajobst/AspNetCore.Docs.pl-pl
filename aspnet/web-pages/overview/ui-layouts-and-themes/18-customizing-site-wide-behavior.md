---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: "Dostosowywanie zachowania całej lokacji, dla strony sieci Web platformy ASP.NET (Razor) witryn | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym rozdziale opisano sposób wprowadzić ustawienia w całej witryny sieci Web lub cały folder, a nie tylko strony."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Dostosowywanie zachowania całej witryny dla witryny sieci Web ASP.NET (Razor) stron
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób wprowadzania ustawień lokacji po stronie dla stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak do uruchomienia kodu, która umożliwia zestaw wartości (globalne wartości lub ustawień pomocnika) dla wszystkich stron w witrynie.
> - Jak uruchamiać kod, który można ustawić wartości dla wszystkich stron w folderze.
> - Jak uruchomić kod przed i po stronie obciążeń.
> - Jak wysłać błędy do strony błędu centralnej.
> - Jak dodawanie uwierzytelniania do wszystkich stron w folderze.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Program WebMatrix 3
> - Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 3 i Visual Studio 2013 (lub programu Visual Studio Express 2013 for Web), chyba że użytkownik nie można użyć bibliotekę pomocników platformy ASP.NET sieci Web.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Dodawanie kodu uruchomienia witryny sieci Web dla strony sieci Web ASP.NET

Dla wielu kod napisany w języku ASP.NET Web Pages pojedynczej strony może zawierać całego kodu, która jest wymagana dla tej strony. Na przykład jeśli strona wysyła wiadomość e-mail, istnieje możliwość umieścić cały kod dla tej operacji na jednej stronie. Może to obejmować kod do zainicjowania ustawienia do wysyłania wiadomości e-mail (to znaczy, że dla serwera SMTP) i wysyłania wiadomości e-mail.

W niektórych sytuacjach może być do uruchomienia kodu przed uruchomieniem dowolnej strony w witrynie. Jest to przydatne do ustawiania wartości, których można użyć w dowolnym miejscu w lokacji (nazywane *wartości globalnej*.) Na przykład niektóre pomocników wymagają podania wartości, takich jak ustawienia poczty e-mail lub klucze konta. Może być przydatne, aby zachować te ustawienia globalne wartości.

Można to zrobić, tworząc stronę o nazwie  *\_AppStart.cshtml* w katalogu głównym witryny. Jeśli ta strona istnieje, działa po raz pierwszy dowolnej strony w witrynie jest wymagane. W związku z tym jest dobrym miejscem do uruchomienia kodu do ustawiania wartości globalnej. (Ponieważ  *\_AppStart.cshtml* prefiksie podkreślenia ASP.NET nie będzie wysyłać strony do przeglądarki, nawet jeśli użytkownicy zażądają go bezpośrednio.)

Na poniższym diagramie przedstawiono sposób  *\_AppStart.cshtml* strony działa. Gdy nadejdzie żądanie strony, a jeśli jest to pierwsze żądanie do dowolnej strony w witrynie platformy ASP.NET najpierw sprawdza, czy czy  *\_AppStart.cshtml* strona istnieje. Jeśli tak, żadnego kodu w  *\_AppStart.cshtml* strony działa, a następnie uruchomi żądanej strony.

![[Obraz]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Ustawianie wartości globalne dla witryny sieci Web

1. W folderze głównym witryny sieci Web programu WebMatrix, Utwórz plik o nazwie  *\_AppStart.cshtml*. Plik musi być w katalogu głównym witryny.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Ten kod przechowuje wartość w `AppState` słownik, który jest automatycznie dostępne dla wszystkich stron w witrynie. Zwróć uwagę, że  *\_AppStart.cshtml* plik nie ma w niej żadnych znaczników. Strona będzie uruchomić kod i Przekierowanie do strony, która pierwotnie zażądano.

    > [!NOTE]
    > Należy zachować ostrożność podczas umieść kod  *\_AppStart.cshtml* pliku. Jeśli wystąpią błędy w kodzie w  *\_AppStart.cshtml* plików, nie uruchamia się witryny sieci Web.
3. W folderze głównym, Utwórz nową stronę o nazwie *AppName.cshtml*.
4. Zastąp domyślną znaczników i kodu z następujących czynności: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Ten kod wyodrębnianie wartości z `AppState` obiektu, który ustawia w  *\_AppStart.cshtml* strony.
5. Uruchom *AppName.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Zostaje wyświetlona strona wartości globalnej. 

    ![[Obraz]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Ustawienie wartości dla wątków

Dobre wykorzystanie dla  *\_AppStart.cshtml* plik jest można ustawić wartości dla wątków, które używają w witrynie i mają być zainicjowana. Typowymi przykładami są ustawienia poczty e-mail dla `WebMail` pomocnika i klucze prywatne i publiczne `ReCaptcha` pomocnika. W takich przypadkach można ustawić wartości raz w  *\_AppStart.cshtml* , a następnie ich jest już ustawiony dla wszystkich stron w witrynie.

W tej procedurze przedstawiono sposób ustawiania `WebMail` ustawienia globalnie. (Aby uzyskać więcej informacji o korzystaniu z `WebMail` pomocnika, zobacz [dodawanie wiadomości E-mail do witryny ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie zostało dodane.
2. Jeśli nie masz jeszcze  *\_AppStart.cshtml* plików, w folderze głównym witryny sieci Web Utwórz plik o nazwie  *\_AppStart.cshtml*.
3. Dodaj następujące `WebMail` ustawienia  *\_AppStart.cshtml* pliku: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Zmodyfikuj następujące e-mail powiązane ustawienia w kodzie:

    - Ustaw `your-SMTP-host` na nazwę serwera SMTP, który ma dostęp do.
    - Ustaw `your-user-name-here` do nazwy użytkownika dla konta serwera SMTP.
    - Ustaw `your-account-password` hasło dla konta serwera SMTP.
    - Ustaw `your-email-address-here` na adres e-mail. Jest to komunikat jest wysyłany z adres e-mail. (Niektóre dostawców poczty e-mail nie umożliwiają określenie innej `From` adresów, a następnie użyje swoją nazwę użytkownika jako `From` adresu.)

    Aby uzyskać więcej informacji na temat ustawień SMTP, zobacz [Konfigurowanie ustawień poczty E-mail](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) w artykule [wysyłania wiadomości E-mail z witryn stron sieci Web platformy ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) i [problemy z wysłaniem wiadomości E-mail](https://go.microsoft.com/fwlink/?LinkId=253001#email)w [ASP.NET Web Pages przewodnik rozwiązywania problemów (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
- Zapisz  *\_AppStart.cshtml* plik i zamknij go.
- W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *TestEmail.cshtml*.
- Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- Uruchom *TestEmail.cshtml* strony w przeglądarce.
- Wypełnij pola, aby wysłać do siebie wiadomość e-mail, a następnie kliknij przycisk **wysyłania**.
- Sprawdź, upewnij się, że zaakceptowania wiadomości e-mail.

Ważnym elementem w tym przykładzie jest to, że ustawienia, które zwykle nie należy zmieniać — takie jak nazwa serwera SMTP i poświadczenia e-mail — są ustawiane w  *\_AppStart.cshtml* pliku. Dzięki temu nie trzeba ustawić je ponownie w każdej stronie gdzie wysłać wiadomości e-mail. (Mimo że, jeśli z jakiegoś powodu musisz zmienić te ustawienia, można ustawić je oddzielnie na stronie.) Na stronie można ustawić tylko wartości, które zwykle zmieniać zawsze, takich jak adresat i treść wiadomości e-mail.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Uruchomienia kodu przed i po nich pliki w folderze

Tak samo, jak można użyć  *\_AppStart.cshtml* do pisania kodu przed uruchomieniem strony w witrynie, można napisać kod, który jest uruchamiany przed (i po) dowolnej strony w określonym folderze Uruchom. Jest to przydatne dla elementów, jak ustawienie tej samej strony układu dla wszystkich stron w folderze lub Sprawdzanie, które użytkownik jest zalogowany przed uruchomieniem strony w folderze.

W przypadku stron w szczególności folderów, można utworzyć kod w pliku o nazwie  *\_PageStart.cshtml*. Na poniższym diagramie przedstawiono sposób  *\_PageStart.cshtml* strony działa. Gdy nadejdzie żądanie strony, aplikacja ASP.NET sprawdza najpierw dla  *\_AppStart.cshtml* i uruchamia się, że strony. Następnie aplikacja ASP.NET sprawdza, czy istnieje  *\_PageStart.cshtml* strony, a jeśli tak, który uruchamia. Następnie uruchomieniu żądanej strony.

Wewnątrz  *\_PageStart.cshtml* strony, można określić miejsce podczas przetwarzania ma żądanej strony do uruchomienia, umieszczając w niej `RunPage` metody. Dzięki temu można uruchomić kod przed uruchomieniem żądanej strony, a następnie ponownie po nim. Jeśli nie podasz `RunPage`, całego kodu w  *\_PageStart.cshtml* działa, a następnie żądanej strony jest uruchamiana automatycznie.

![[Obraz]](18-customizing-site-wide-behavior/_static/image3.jpg)

Program ASP.NET pozwala utworzyć hierarchię  *\_PageStart.cshtml* plików. Możesz też zaznaczyć  *\_PageStart.cshtml* pliku w katalogu głównym witryny i wszelkich podfolderów. Po zażądaniu strony  *\_PageStart.cshtml* pliku przebiegów najwyższy poziom (znajdujący się najbliżej katalogu głównego witryny), a następnie  *\_PageStart.cshtml* pliku w ciągu następnych podfolder, i tak dalej dół struktury podfolderów, dopóki żądanie dotrze folder zawierający żądanej strony. Po wszystkich odpowiednich  *\_PageStart.cshtml* plików został uruchomiony program jest uruchomiony żądanej strony.

Na przykład może być następujących kombinacji  *\_PageStart.cshtml* plików i *Default.cshtml* pliku:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Po uruchomieniu */myfolder/default.cshtml*, pojawi się następujące:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Wykonywanie kodu inicjowania dla wszystkich stron w folderze

Dobre wykorzystanie dla  *\_PageStart.cshtml* plików jest zainicjowanie tej samej strony układu dla wszystkich plików w jednym folderze.

1. W folderze głównym, Utwórz nowy folder o nazwie *InitPages*.
2. W *InitPages* folderu witryny sieci Web, Utwórz plik o nazwie  *\_PageStart.cshtml* i zastąp go następującym łańcuchem domyślne znaczników i kodu: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. W katalogu głównym witryny sieci Web, Utwórz folder o nazwie *Shared*.
4. W *Shared* folderu, Utwórz plik o nazwie  *\_Layout1.cshtml* i zastąp go następującym łańcuchem domyślne znaczników i kodu: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. W *InitPages* folderu, Utwórz plik o nazwie *Content1.cshtml* i Zastąp istniejącą zawartość następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. W *InitPages* folderu, Utwórz plik o nazwie *Content2.cshtml* i Zastąp znaczników domyślne następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Uruchom *Content1.cshtml* w przeglądarce. 

    ![[Obraz]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Gdy *Content1.cshtml* strony działa,  *\_PageStart.cshtml* plików zestawów `Layout` , a także ustawia `PageData["MyBackground"]` na kolor. W *Content1.cshtml*, układu i koloru są stosowane.
8. Wyświetl *Content2.cshtml* w przeglądarce. 

    Układ jest taki sam, ponieważ obie strony używają tej samej strony układu i kolor, jak zainicjować w  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Przy użyciu \_PageStart.cshtml do obsługi błędów

Użyj innego dobrego dla  *\_PageStart.cshtml* jest utworzenie sposób obsługi błędów programowania (wyjątkami), które mogą wystąpić w żadnym pliku *.cshtml* strony w folderze. W tym przykładzie przedstawiono jeden ze sposobów.

1. W folderze głównym, Utwórz folder o nazwie *InitCatch*.
2. W *InitCatch* folderu witryny sieci Web, Utwórz plik o nazwie  *\_PageStart.cshtml* i Zastąp istniejące znaczników i kodu następującym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    W tym kodzie próby jawnie systemem żądanej strony, wywołując `RunPage` metody w ramach `try` bloku. Jeśli wystąpią błędy programowania w żądanej strony, kodu wewnątrz `catch` blokowanie działa. W takim przypadku kod przekierowuje do strony (*Error.cshtml*) i przekazuje nazwę pliku, w którym wystąpił błąd jako część adresu URL. (Utworzysz strony wkrótce.)
3. W *InitCatch* folderu witryny sieci Web, Utwórz plik o nazwie *Exception.cshtml* i Zastąp istniejące znaczników i kodu następującym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Do celów tego przykładu wykonywanych na tej stronie jest celowo tworzenia błąd podejmując próbę otwarcia pliku bazy danych, który nie istnieje.
4. W folderze głównym, Utwórz plik o nazwie *Error.cshtml* i Zastąp istniejące znaczników i kodu następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Na tej stronie wyrażenia `@Request["source"]` pobiera wartość spoza adres URL i wyświetla je.
5. Na pasku narzędzi, kliknij przycisk **zapisać**.
6. Uruchom *Exception.cshtml* w przeglądarce. 

    ![[Obraz]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Ponieważ w występuje błąd *Exception.cshtml*,  *\_PageStart.cshtml* strona przekierowuje do *Error.cshtml* pliku, który wyświetla komunikat.

    Aby uzyskać więcej informacji o wyjątkach, zobacz [wprowadzenie do platformy ASP.NET Web Pages programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Przy użyciu \_PageStart.cshtml, aby ograniczyć dostęp do folderu

Można również użyć  *\_PageStart.cshtml* plik, aby ograniczyć dostęp do wszystkich plików w folderze.

1. W programie WebMatrix, tworzenia nowej witryny sieci Web przy użyciu **szablonu z witryny** opcji.
2. Wybierz z dostępnych szablonów **witryny początkowej**.
3. W folderze głównym, Utwórz folder o nazwie *AuthenticatedContent*.
4. W *AuthenticatedContent* folderu, Utwórz plik o nazwie  *\_PageStart.cshtml* i Zastąp istniejące znaczników i kodu następującym kodem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Kod uruchamiany zapobiegając wszystkie pliki w folderze pamięci podręcznej. (Jest to wymagane w przypadku scenariuszy, takich jak komputery publiczne, w którym nie ma buforowanych stron jednego użytkownika mają być dostępne dla następnego użytkownika.) Następnie kod określa, czy użytkownik zalogował się do lokacji zanim można wyświetlić dowolnej strony w folderze. Jeśli użytkownik nie jest zalogowany, kod przekierowuje do strony logowania. Strona logowania użytkownika można było powrócić do strony, który pierwotnie odebrał żądanie, jeśli wartość ciągu kwerendy o nazwie `ReturnUrl`.
5. Utwórz nową stronę w *AuthenticatedContent* folder o nazwie *Page.cshtml*.
6. Zastąp znaczników domyślne następujących czynności:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Uruchom *Page.cshtml* w przeglądarce. Kod przekieruje Cię do strony logowania. Należy zarejestrować przed zalogowaniem się. Po został zarejestrowany i zalogowany, możesz przejść do strony i wyświetlić jej zawartość.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Wprowadzenie do programowania przy użyciu składni Razor strony sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=251587)
