---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Dodawanie produktu ASP.NET Identity do pustego lub istniejącego sieci Web Forms projektu | Dokumentacja firmy Microsoft
author: raquelsa
description: W tym samouczku przedstawiono sposób dodawania produktu ASP.NET Identity (nowy system członkostwa programu ASP.NET) do aplikacji ASP.NET. Podczas tworzenia nowych formularzy sieci Web lub MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236513"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Dodawanie systemu ASP.NET Identity do pustego lub istniejącego projektu Web Forms


> W tym samouczku przedstawiono sposób dodawania [produktu ASP.NET Identity](introduction-to-aspnet-identity.md) (nowy system członkostwa programu ASP.NET) do aplikacji ASP.NET.
> 
> Po utworzeniu nowego projektu formularzy sieci Web lub MVC w programie Visual Studio 2017 RTM za pomocą indywidualnych kont programu Visual Studio zainstalujesz wymagane pakiety i Dodaj wszystkie klasy niezbędne dla Ciebie. Ten samouczek przedstawia kroki Aby dodać obsługę produktu ASP.NET Identity do istniejącego projektu formularzy sieci Web lub nowy pusty projekt. Czy mogę opisują wszystkich pakietów NuGet, które należy zainstalować i klas, które należy dodać. Będzie go za pośrednictwem formularzy sieci Web przykładowej rejestrowania nowych użytkowników i logowania podczas wyróżniania wszystkie interfejsy API punkt wejścia głównego do zarządzania użytkownikami i uwierzytelniania. W tym przykładzie będzie używać w implementacji domyślnej produktu ASP.NET Identity magazyn danych SQL, która jest oparta na platformie Entity Framework. Ten samouczek, firma Microsoft będzie używać LocalDB bazy danych SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Wprowadzenie do produktu ASP.NET Identity

1. Rozpocznij od instalowania i uruchamiania [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Wybierz **nowy projekt** od samego początku strony, lub można użyć menu i wybrać **pliku**, a następnie **nowy projekt**.
3. W okienku po lewej stronie rozwiń **Visual C#** , a następnie wybierz **Web**, następnie **aplikacji sieci Web ASP.NET (.Net Framework)**. Nazwij swój projekt "WebFormsIdentity", a następnie wybierz pozycję **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **Pusty**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Zwróć uwagę **Zmień uwierzytelnianie** przycisk jest wyłączony i nie obsługuje uwierzytelniania znajduje się w tym szablonie. Szablony formularzy sieci Web, MVC i interfejs API sieci Web umożliwiają wybranie podejścia do uwierzytelniania.

## <a name="add-identity-packages-to-your-app"></a>Dodaj pakiety tożsamości do aplikacji

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. Wyszukiwanie i instalowanie **Microsoft.AspNet.Identity.EntityFramework** pakietu. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Należy zwrócić uwagę na to, że ten pakiet zainstaluje pakiety zależności: **EntityFramework** i **tożsamość Microsoft ASP.NET Core**.

## <a name="add-a-web-form-to-register-users"></a>Dodaj formularz sieci web w celu zarejestrowania użytkowników

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie **formularz sieci Web**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. W **Określ nazwę dla elementu** okno dialogowe, Nazwa nowego formularza sieci web **zarejestrować**, a następnie wybierz pozycję **OK**
3. Zastąp kod znaczników w wygenerowanym *Register.aspx* plik zawierający poniższy kod. Zmiany kodu są wyróżnione. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Jest to po prostu uproszczoną wersję *Register.aspx* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET. Kod znaczników powyżej umożliwia dodanie pola formularza i przycisk, aby zarejestrować nowego użytkownika.
4. Otwórz *Register.aspx.cs* i Zastąp zawartość pliku następującym kodem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Powyższy kod jest uproszczoną wersją *Register.aspx.cs* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET.
    > 2. *IdentityUser* klasa jest domyślna implementacja EntityFramework *IUser* interfejsu. *IUser* interfejs jest minimalny interfejs użytkownika programu ASP.NET Identity Core.
    > 3. *Magazynie UserStore* klasa jest domyślna implementacja EntityFramework magazynu na użytkownika. Ta klasa implementuje interfejsy minimalny platformy ASP.NET Core Identity: *Elementy IUserStore*, *IUserLoginStore*, *IUserClaimStore* i *IUserRoleStore*.
    > 4. *Menedżera UserManager* klasa udostępnia związane z użytkownikami interfejsy API, który automatycznie zapisze zmiany *magazynie UserStore*.
    > 5. *IdentityResult* klasa reprezentuje wynik operacji tożsamości.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, **Dodaj Folder ASP.NET** i następnie **aplikacji\_danych**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otwórz *Web.config* pliku i Dodaj wpis ciągu połączenia dla bazy danych, firma Microsoft będzie używany do przechowywania informacji o użytkowniku. Baza danych zostanie utworzona w czasie wykonywania przez EntityFramework jednostek tożsamości. Parametry połączenia są podobne do tworzony automatycznie podczas tworzenia nowego projektu formularzy sieci Web. Wyróżniony kod znaczników, należy dodać:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Dla programu Visual Studio 2015 lub nowszego, Zastąp `(localdb)\v11.0` z `(localdb)\MSSQLLocalDB` w ciągu połączenia.
    
7. Kliknij prawym przyciskiem myszy plik *Register.aspx* w projekcie i wybierz **Ustaw jako strona startowa**. Naciśnij klawisze Ctrl + F5, aby skompilować i uruchomić aplikację sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie wybierz pozycję **zarejestrować**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > Tożsamości ASP.NET zapewnia obsługę sprawdzania poprawności i w tym przykładzie domyślne zachowanie na użytkownika i hasła można sprawdzić poprawności, które pochodzą z pakiet podstawowego tożsamości. Domyślny moduł weryfikacji dla użytkownika (`UserValidator`) ma właściwość `AllowOnlyAlphanumericUserNames` zawierający wartość domyślna równa `true`. Domyślny moduł weryfikacji dla hasła (`MinimumLengthValidator`) zapewnia, że hasło zawiera co najmniej 6 znaków. Te moduły są właściwościami na `UserManager` , może zostać zastąpiona, jeśli chcesz mieć niestandardowego sprawdzania poprawności,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Sprawdź bazy danych LocalDb tożsamości i tabele generowane przez program Entity Framework

1. W **widoku** menu, wybierz opcję **Eksploratora serwera**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozwiń **DefaultConnection (WebFormsIdentity)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** , a następnie wybierz **Pokaż dane tabeli**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Konfigurowanie aplikacji uwierzytelniania OWIN

W tym momencie tylko Dodaliśmy obsługę tworzenia użytkowników. Teraz zamierzamy pokazują, jak możemy dodać uwierzytelnianie, aby zalogować użytkownika. Tożsamości ASP.NET używa oprogramowania pośredniczącego uwierzytelniania OWIN firmy Microsoft dla uwierzytelniania formularzy. Uwierzytelniania plików Cookie OWIN plik cookie i oświadczeń mechanizmu uwierzytelniania zależności, które mogą być używane przez dowolnej architektury w serwisie [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) lub IIS. W tym modelu te same pakiety uwierzytelniania może służyć przez wielu struktur, w tym ASP.NET MVC i formularzy sieci Web. Aby uzyskać więcej informacji na temat projektu Katana oraz sposobu uruchamiania oprogramowania pośredniczącego Zobacz niezależny od hosta [wprowadzenie do projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Zainstaluj pakiety uwierzytelniania do aplikacji

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. Wyszukiwanie i instalowanie ***Microsoft.AspNet.Identity.Owin*** pakietu. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Wyszukiwanie i instalowanie ***Microsoft.Owin.Host.SystemWeb*** pakietu.

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** pakiet zawiera zestaw klas rozszerzeń OWIN do zarządzania i konfigurowania oprogramowania pośredniczącego uwierzytelniania OWIN do użycia przez pakiety platformy ASP.NET Core Identity.
    > **Microsoft.Owin.Host.SystemWeb** pakiet zawiera serwer OWIN, który umożliwia aplikacji OWIN do uruchamiania w usługach IIS przy użyciu potoku żądania programu ASP.NET. Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące OWIN w usługach IIS zintegrowany potok](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Dodawanie klas konfiguracji uruchamiania i uwierzytelniania OWIN

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj**, a następnie **Dodaj nowy element**. W oknie dialogowym pole tekstowe wyszukiwania wpisz "*owin*". Nadaj nazwę klasy "*uruchamiania*" i wybierz **Dodaj**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. W pliku Startup.cs Dodaj wyróżniony kod przedstawione poniżej, aby skonfigurować uwierzytelniania plików cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Ta klasa zawiera `OwinStartup` atrybutu w celu określenia klasy początkowej OWIN. Każda aplikacja OWIN ma klasę uruchamiania, w których określane są składniki do potoku aplikacji. Zobacz [wykrywanie klasy początkowej OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Aby uzyskać więcej informacji na ten model.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Dodawanie formularzy sieci web rejestracji i logowania użytkowników

1. Otwórz *Register.aspx.cs* pliku i Dodaj następujący kod które loguje się użytkownik, po pomyślnym zakończeniu rejestracji.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Ponieważ produktu ASP.NET Identity i uwierzytelniania plików Cookie OWIN są oświadczenia na podstawie systemu, struktura wymaga deweloperem aplikacji, aby wygenerować [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) dla użytkownika. Tożsamość ClaimsIdentity zawierają informacje dotyczące wszystkich oświadczeń dla użytkownika, takich jak role, jakie należy użytkownik. Można również dodać więcej oświadczenia dla użytkownika na tym etapie.
    > - Użytkownik loguje użytkownika za pomocą słowniku z OWIN i wywoływania `SignIn` i przekazując ClaimsIdentity, jak pokazano powyżej. Ten kod będzie zalogować użytkownika i generowanie pliku cookie, jak również. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) posługują się [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj**, a następnie **formularz sieci Web**. Nazwa formularza sieci web **logowania**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Zastąp zawartość *Login.aspx* pliku następującym kodem:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Zastąp zawartość *Login.aspx.cs* pliku następującym kodem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Teraz sprawdza, czy stan bieżącego użytkownika i wykonuje akcję na podstawie jego `Context.User.Identity.IsAuthenticated` stanu.
    >     **Wyświetlanie rejestrowane w polu Nazwa użytkownika** : Struktury tożsamość Microsoft ASP.NET został dodany metody rozszerzenia w [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) pozwala uzyskać `UserName` i `UserId` dla zalogowanego użytkownika. Te metody rozszerzenia są zdefiniowane w `Microsoft.AspNet.Identity.Core` zestawu. Te metody rozszerzenia są zastępuje [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Logowanie za pomocą metody: `This` metoda zastępuje poprzedni `CreateUser_Click` metody, w tym przykładowych i teraz loguje się użytkownik, po pomyślnym utworzeniu użytkownika.   
    >   Microsoft OWIN Framework został dodany metody rozszerzenia w `System.Web.HttpContext` pozwala to uzyskać odwołanie do `IOwinContext`. Te metody rozszerzenia są zdefiniowane w `Microsoft.Owin.Host.SystemWeb` zestawu. `OwinContext` Klasy ujawnia `IAuthenticationManager` właściwość, która reprezentuje funkcje oprogramowania pośredniczącego uwierzytelniania dostępne dla bieżącego żądania. Użytkownik loguje użytkownika za pomocą `AuthenticationManager` z OWIN i wywoływania `SignIn` i przekazując `ClaimsIdentity` jak pokazano powyżej. Ponieważ produktu ASP.NET Identity i uwierzytelniania plików Cookie OWIN są systemu opartego na oświadczeniach, struktura wymaga aplikację, aby wygenerować `ClaimsIdentity` dla użytkownika. `ClaimsIdentity` Zawiera informacje o wszystkich oświadczeń użytkownika, takich jak role, jakie należy użytkownik. Można również dodać więcej oświadczenia dla użytkownika na tym etapie, ten kod będzie zalogować użytkownika i generowanie pliku cookie, jak również. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) posługują się [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
    > - `SignOut` Metoda: Pobiera odwołanie do `AuthenticationManager` z OWIN i wywołania `SignOut`. To jest odpowiednikiem [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodę używaną przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
5. Naciśnij klawisz **klawiszy Ctrl + F5** do kompilowania i uruchamiania aplikacji sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie wybierz pozycję **zarejestrować**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Uwaga: W tym momencie nowy użytkownik jest utworzony i zalogowany.
6. Wybierz **Wyloguj** przycisku. Nastąpi przekierowanie do strony logowania formularza.
7. Nieprawidłowa nazwa użytkownika lub hasło należy wprowadzić, a następnie wybierz pozycję **Zaloguj** przycisku. 
   `UserManager.Find` Metoda zwróci wartość null i komunikat o błędzie: " *Nieprawidłowa nazwa użytkownika lub hasło* "będą wyświetlane.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
