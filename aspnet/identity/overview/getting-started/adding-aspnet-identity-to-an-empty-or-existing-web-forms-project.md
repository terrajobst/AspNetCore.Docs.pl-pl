---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Dodawanie produktu ASP.NET Identity do pustego lub istniejącego sieci Web Forms projektu | Dokumentacja firmy Microsoft
author: raquelsa
description: W tym samouczku przedstawiono sposób dodawania produktu ASP.NET Identity (nowy system członkostwa programu ASP.NET) do aplikacji ASP.NET. Podczas tworzenia nowych formularzy sieci Web lub MVC...
ms.author: riande
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 229d6fef5aa9c2384b6d92ec3e3ed7316b69afe0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755777"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Dodawanie produktu ASP.NET Identity do pustego lub istniejącego sieci Web Forms projektu
====================
przez [Raquel Soares De Almeida](https://github.com/raquelsa)

> W tym samouczku przedstawiono sposób dodawania [produktu ASP.NET Identity](introduction-to-aspnet-identity.md) (nowy system członkostwa programu ASP.NET) do aplikacji ASP.NET.
> 
> Po utworzeniu nowego projektu formularzy sieci Web lub MVC w programie Visual Studio 2013 RTM za pomocą indywidualnych kont programu Visual Studio zainstalujesz wymagane pakiety i Dodaj wszystkie klasy niezbędne dla Ciebie. Ten samouczek przedstawia kroki Aby dodać obsługę produktu ASP.NET Identity do istniejącego projektu formularzy sieci Web lub nowy pusty projekt. Czy mogę opisują wszystkich pakietów NuGet, które należy zainstalować i klas, które należy dodać. Będzie go za pośrednictwem formularzy sieci Web przykładowej rejestrowania nowych użytkowników i logowania podczas wyróżniania wszystkie interfejsy API punkt wejścia głównego do zarządzania użytkownikami i uwierzytelniania. W tym przykładzie będzie używać w implementacji domyślnej produktu ASP.NET Identity magazyn danych SQL, która jest oparta na platformie Entity Framework. Ten samouczek, firma Microsoft będzie używać LocalDB bazy danych SQL.
> 
> Ten samouczek został napisany przez Raquel Soares De Almeida i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Wprowadzenie do produktu ASP.NET Identity

1. Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Kliknij przycisk **nowy projekt** od samego początku strony, lub można użyć menu i wybrać **pliku**, a następnie **nowy projekt**.
3. Wybierz **Visual C# i** n okienka po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt "WebFormsIdentity", a następnie kliknij przycisk **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Zwróć uwagę **Zmień uwierzytelnianie** przycisk jest wyłączony i nie obsługuje uwierzytelniania znajduje się w tym szablonie. Szablony formularzy sieci Web, MVC i interfejs API sieci Web umożliwiają wybranie podejścia do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [omówienie uwierzytelniania](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Trwa dodawanie pakietów tożsamości do aplikacji

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole tekstowe wyszukiwania wpisz "*Identity.E*". Kliknij przycisk Instaluj, aby uzyskać tego pakietu.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Należy pamiętać, że ten pakiet zainstaluje pakiety zależności: EntityFramework i tożsamość Microsoft ASP.NET w wersji podstawowej.

## <a name="adding-web-forms-to-register-users"></a>Dodawanie formularzy sieci Web w celu zarejestrowania użytkowników

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj**, a następnie **formularz sieci Web**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. W **Określ nazwę dla elementu** okno dialogowe, Nazwa nowego formularza sieci web **zarejestrować**, a następnie kliknij przycisk **OK**
3. Zastąp kod znaczników w wygenerowanym *Register.aspx* plik zawierający poniższy kod. Zmiany kodu są wyróżnione.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Jest to po prostu uproszczoną wersję *Register.aspx* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET. Kod znaczników powyżej umożliwia dodanie pola formularza i przycisk, aby zarejestrować nowego użytkownika.
4. Otwórz *Register.aspx.cs* i Zastąp zawartość pliku następującym kodem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Powyższy kod jest uproszczoną wersją *Register.aspx.cs* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET.
    > 2. *IdentityUser* klasa jest domyślna implementacja EntityFramework *IUser* interfejsu. *IUser* interfejs jest minimalny interfejs użytkownika programu ASP.NET Identity Core.
    > 3. *Magazynie UserStore* klasa jest domyślna implementacja EntityFramework magazynu na użytkownika. Ta klasa implementuje interfejsy minimalny platformy ASP.NET Core Identity: *elementy IUserStore*, *IUserLoginStore*, *IUserClaimStore* i *IUserRoleStore* .
    > 4. *Menedżera UserManager* klasa udostępnia związane z użytkownikami interfejsy API, który automatycznie zapisze zmiany *magazynie UserStore*.
    > 5. *IdentityResult* klasa reprezentuje wynik operacji tożsamości.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj**, **Dodaj Folder ASP.NET** i następnie **aplikacji\_danych**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otwórz *Web.config* pliku i Dodaj wpis ciągu połączenia dla bazy danych, firma Microsoft będzie używany do przechowywania informacji o użytkowniku. Baza danych zostanie utworzona w czasie wykonywania przez EntityFramework jednostek tożsamości. Parametry połączenia są podobne do tworzony automatycznie podczas tworzenia nowego projektu formularzy sieci Web. Wyróżniony kod znaczników, należy dodać:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Dla programu Visual Studio 2015 lub nowszego, Zastąp `(localdb)\v11.0` z `(localdb)\MSSQLLocalDB` w ciągu połączenia.
    
7. Kliknij prawym przyciskiem myszy plik *Register.aspx* w projekcie i wybierz **Ustaw jako strona startowa**. Naciśnij klawisze Ctrl + F5, aby skompilować i uruchomić aplikację sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > Tożsamości ASP.NET zapewnia obsługę sprawdzania poprawności i w tym przykładzie domyślne zachowanie na użytkownika i hasła można sprawdzić poprawności, które pochodzą z pakiet podstawowego tożsamości. Domyślny moduł weryfikacji dla użytkownika (`UserValidator`) ma właściwość `AllowOnlyAlphanumericUserNames` zawierający wartość domyślna równa `true`. Domyślny moduł weryfikacji dla hasła (`MinimumLengthValidator`) zapewnia, że hasło zawiera co najmniej 6 znaków. Te moduły są właściwościami na `UserManager` , może zostać zastąpiona, jeśli chcesz mieć niestandardowego sprawdzania poprawności,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Weryfikowanie bazy danych LocalDb tożsamości i tabele generowane przez program Entity Framework

1. W **widoku** menu, kliknij przycisk **Eksploratora serwera**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozwiń **DefaultConnection (WebFormsIdentity)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurowanie aplikacji uwierzytelniania OWIN

W tym momencie tylko Dodaliśmy obsługę tworzenia użytkowników. Teraz zamierzamy pokazują, jak możemy dodać uwierzytelnianie, aby zalogować użytkownika. Tożsamości ASP.NET używa oprogramowania pośredniczącego uwierzytelniania OWIN firmy Microsoft dla uwierzytelniania formularzy. Uwierzytelniania plików Cookie OWIN plik cookie i oświadczeń mechanizmu uwierzytelniania zależności, które mogą być używane przez dowolnej architektury w serwisie [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) lub IIS. W tym modelu te same pakiety uwierzytelniania może służyć przez wielu struktur, w tym ASP.NET MVC i formularzy sieci Web. Aby uzyskać więcej informacji na temat projektu Katana oraz sposobu uruchamiania oprogramowania pośredniczącego Zobacz niezależny od hosta [wprowadzenie do projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalowanie pakietów uwierzytelniania do aplikacji

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole tekstowe wyszukiwania wpisz "*Identity.Owin*". Kliknij przycisk Instaluj, aby uzyskać tego pakietu.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Wyszukaj pakiet ***Microsoft.Owin.Host.SystemWeb*** i zainstaluj go.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** pakiet zawiera zestaw klas rozszerzeń OWIN do zarządzania i konfigurowania oprogramowania pośredniczącego uwierzytelniania OWIN do użycia przez pakiety platformy ASP.NET Core Identity.  
    > **Microsoft.Owin.Host.SystemWeb** pakiet zawiera serwer OWIN, który umożliwia aplikacji OWIN do uruchamiania w usługach IIS przy użyciu potoku żądania programu ASP.NET. Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące OWIN w usługach IIS zintegrowany potok](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Dodawanie początkowa OWIN i klas konfiguracji uwierzytelniania

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, kliknij przycisk **Dodaj**, a następnie **Dodaj nowy element**. W oknie dialogowym pole tekstowe wyszukiwania wpisz "*owin*". Nadaj nazwę klasy "*uruchamiania*" i kliknij przycisk **Dodaj**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. W pliku Startup.cs Dodaj wyróżniony kod przedstawione poniżej, aby skonfigurować uwierzytelniania plików cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Ta klasa zawiera `OwinStartup` atrybutu w celu określenia klasy początkowej OWIN. Każda aplikacja OWIN ma klasę uruchamiania, w których określane są składniki do potoku aplikacji. Zobacz [wykrywanie klasy początkowej OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Aby uzyskać więcej informacji na ten model.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Dodawanie formularzy sieci Web rejestracji i logowania użytkowników

1. Otwórz *Register.cs* pliku i Dodaj następujący kod, który będzie zalogować użytkownika, po pomyślnym zakończeniu rejestracji. Zmiany są wyróżnione poniżej.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Ponieważ produktu ASP.NET Identity i uwierzytelniania plików Cookie OWIN są oświadczenia na podstawie systemu, struktura wymaga deweloperem aplikacji, aby wygenerować [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) dla użytkownika. Tożsamość ClaimsIdentity zawierają informacje dotyczące wszystkich oświadczeń dla użytkownika, takich jak role, jakie należy użytkownik. Można również dodać więcej oświadczenia dla użytkownika na tym etapie.
    > - Użytkownik loguje użytkownika za pomocą słowniku z OWIN i wywoływania `SignIn` i przekazując ClaimsIdentity, jak pokazano powyżej. Ten kod będzie zalogować użytkownika i generowanie pliku cookie, jak również. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) posługują się [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy kliknij Twojego projektu **Dodaj**, a następnie **formularz sieci Web**. Nazwa formularza sieci web **logowania**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Zastąp zawartość *Login.aspx* pliku następującym kodem:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Zastąp zawartość *Login.aspx.cs* pliku następującym kodem:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Teraz sprawdza, czy stan bieżącego użytkownika i wykonuje akcję na podstawie jego `Context.User.Identity.IsAuthenticated` stanu.  
    >     **Wyświetl zarejestrowane w imieniu użytkownika** : Microsoft ASP.NET Identity Framework został dodany metody rozszerzenia w [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) pozwala uzyskać `UserName` i `UserId` dla zalogowany użytkownik. Te metody rozszerzenia są zdefiniowane w `Microsoft.AspNet.Identity.Core` zestawu. Te metody rozszerzenia są zastępuje [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Logowanie za pomocą metody:   
    >     `This` metoda zastępuje poprzedni `CreateUser_Click` metody, w tym przykładowych i teraz loguje się użytkownik, po pomyślnym utworzeniu użytkownika.   
    >  Microsoft OWIN Framework został dodany metody rozszerzenia w `System.Web.HttpContext` pozwala to uzyskać odwołanie do `IOwinContext`. Te metody rozszerzenia są zdefiniowane w `Microsoft.Owin.Host.SystemWeb` zestawu. `OwinContext` Klasy ujawnia `IAuthenticationManager` właściwość, która reprezentuje funkcje oprogramowania pośredniczącego uwierzytelniania dostępne dla bieżącego żądania.  
    >  Użytkownik loguje użytkownika za pomocą `AuthenticationManager` z OWIN i wywoływania `SignIn` i przekazując `ClaimsIdentity` jak pokazano powyżej.   
    >  Ponieważ produktu ASP.NET Identity i uwierzytelniania plików Cookie OWIN są systemu opartego na oświadczeniach, struktura wymaga aplikację, aby wygenerować `ClaimsIdentity` dla użytkownika.   
    >  `ClaimsIdentity` Zawiera informacje o wszystkich oświadczeń użytkownika, takich jak role, jakie należy użytkownik. Możesz również dodać więcej oświadczenia dla użytkownika na tym etapie  
    >  Ten kod będzie zalogować użytkownika i generowanie pliku cookie, jak również. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) posługują się [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
    > - `SignOut` Metoda:   
    >  Pobiera odwołanie do `AuthenticationManager` z OWIN i wywołania `SignOut`. To jest odpowiednikiem [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodę używaną przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
5. Naciśnij klawisz **klawiszy Ctrl + F5** do kompilowania i uruchamiania aplikacji sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Uwaga: W tym momencie nowy użytkownik jest utworzony i zalogowany.
6. Kliknij pozycję **Wyloguj** przycisku. Nastąpi przekierowanie do strony logowania formularza.
7. Wprowadź Nieprawidłowa nazwa użytkownika lub hasło i kliknij przycisk na **Zaloguj** przycisku.   
   `UserManager.Find` Metoda zwróci wartość null i komunikat o błędzie: " *Nieprawidłowa nazwa użytkownika lub hasło* " będą wyświetlane.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
