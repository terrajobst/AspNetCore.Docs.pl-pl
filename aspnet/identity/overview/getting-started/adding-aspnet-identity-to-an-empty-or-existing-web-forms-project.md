---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Dodawanie projektu formularzy ASP.NET Identity pusta lub istniejącej sieci Web | Dokumentacja firmy Microsoft
author: raquelsa
description: W tym samouczku przedstawiono sposób dodawania tożsamości platformy ASP.NET (nowego systemu członkostwa programu ASP.NET) do aplikacji ASP.NET. Podczas tworzenia nowych formularzy sieci Web lub MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8961e596f0d6cc4810e2439be1ec2915bddb8c78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Dodawanie ASP.NET Identity pusta lub istniejącej sieci Web Forms projektu
====================
przez [Raquel Soares De Almeida](https://github.com/raquelsa)

> W tym samouczku przedstawiono sposób dodawania [ASP.NET Identity](introduction-to-aspnet-identity.md) (nowego systemu członkostwa programu ASP.NET) do aplikacji ASP.NET.
> 
> Podczas tworzenia nowego projektu formularzy sieci Web lub MVC w Visual Studio 2013 RTM z indywidualnych kont, Visual Studio zainstaluje wszystkie wymagane pakiety i dodać wszystkie niezbędne klasy dla Ciebie. W tym samouczku przedstawiają kroki, aby dodać obsługę tożsamości ASP.NET do istniejącego projektu formularzy sieci Web lub nowy pusty projekt. I będzie konspektu wszystkich pakietów NuGet, które należy zainstalować i klasy, które należy dodać. I będą przekazywane przykładowych formularzy sieci Web dla nowych użytkowników do rejestrowania i zalogowanie się podczas wyróżniania wszystkich interfejsów API punktu wejścia głównego do zarządzania użytkownikami i uwierzytelniania. W tym przykładzie będzie używać ASP.NET Identity domyślna Implementacja magazynu danych SQL, który jest oparty na platformie Entity Framework. Ten samouczek bazy danych SQL użyjemy LocalDB.
> 
> W tym samouczku zapisał Raquel Soares De Almeida i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Wprowadzenie tożsamości platformy ASP.NET

1. Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Kliknij przycisk **nowy projekt** od początku strony, lub można użyć menu i wybierz **pliku**, a następnie **nowy projekt**.
3. Wybierz **Visual C# i** n okienka po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwa projektu "WebFormsIdentity", a następnie kliknij przycisk **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Powiadomienie **Zmień uwierzytelnianie** przycisk jest wyłączony i nie obsługuje uwierzytelniania jest dostępne w tym szablonie. Szablony sieci Web Forms, MVC i interfejsu API sieci Web umożliwiają wybierz metodę uwierzytelniania. Aby uzyskać więcej informacji, zobacz [omówienie uwierzytelniania](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Dodawanie pakietów tożsamości do aplikacji

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole tekstowe wyszukiwania, wpisz "*Identity.E*". Kliknij przycisk Instaluj tego pakietu.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Należy pamiętać, że ten pakiet zostanie zainstalowany pakietów zależności: EntityFramework i Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Dodawanie formularzy sieci Web na rejestrowanie użytkowników

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj**, a następnie **formularza sieci Web**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. W **Określ nazwę dla elementu** okno dialogowe, Nazwa nowego formularza sieci web **zarejestrować**, a następnie kliknij przycisk **OK**
3. Zastąp kod znaczników w wygenerowanym *Register.aspx* pliku przy użyciu poniższego kodu. Zmiany kodu zostały wyróżnione.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Jest to po prostu uproszczonej wersji *Register.aspx* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET. Kod znaczników powyżej dodaje pola formularza i przycisk, aby zarejestrować nowego użytkownika.
4. Otwórz *Register.aspx.cs* plików i Zastąp zawartość pliku następującym kodem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Powyższy kod jest uproszczone *Register.aspx.cs* pliku, który jest tworzony podczas tworzenia nowego projektu formularzy sieci Web ASP.NET.
    > 2. *IdentityUser* klasy jest domyślną implementację EntityFramework *IUser* interfejsu. *IUser* interfejs jest minimalny interfejs użytkownika programu ASP.NET Identity Core.
    > 3. *Magazynie UserStore* klasy jest domyślna implementacja EntityFramework magazynu użytkowników. Ta klasa implementuje interfejsy minimalnego platformy ASP.NET Core tożsamości: *elementy IUserStore*, *IUserLoginStore*, *IUserClaimStore* i *IUserRoleStore* .
    > 4. *Interfejs UserManager* użytkownik udostępnia klasy powiązane interfejsy API, który automatycznie zapisze zmiany *magazynie UserStore*.
    > 5. *IdentityResult* klasy reprezentuje wynik operacji tożsamości.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj**, **Dodaj Folder ASP.NET** , a następnie **aplikacji\_danych**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otwórz *Web.config* plik i dodać wpis ciągu połączenia dla bazy danych, firma Microsoft będzie używany do przechowywania informacji o użytkowniku. Bazy danych zostanie utworzone w czasie wykonywania przez EntityFramework jednostek tożsamości. Parametry połączenia są podobne do utworzony podczas tworzenia nowego projektu formularzy sieci Web. Zaznaczony kod znaczników, należy dodać:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Dla programu Visual Studio 2015 lub nowszego, Zastąp `(localdb)\v11.0` z `(localdb)\MSSQLLocalDB` w ciągu połączenia.
    
7. Kliknij prawym przyciskiem myszy plik *Register.aspx* w projekcie i wybierz **Ustaw jako stronę startową**. Naciśnij klawisze Ctrl + F5, aby skompilować i uruchomić aplikację sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity obsługę weryfikacji i w tym przykładzie może sprawdzać to zachowanie domyślne w użytkownika i hasła moduły weryfikacji, które pochodzą z pakietu podstawowej tożsamości. Domyślny moduł weryfikacji dla użytkownika (`UserValidator`) ma właściwość `AllowOnlyAlphanumericUserNames` która ma ustawioną wartość domyślną `true`. Domyślny moduł weryfikacji dla hasła (`MinimumLengthValidator`) zapewnia, że hasło ma co najmniej 6 znaków. Te moduły weryfikacji właściwości znajdują się na `UserManager` może zostać zastąpiona, jeśli chcesz użyć niestandardowego sprawdzania poprawności,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Weryfikowanie bazy danych LocalDb tożsamości i tabele generowane przez program Entity Framework

1. W **widoku** menu, kliknij przycisk **Eksploratora serwera**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozwiń węzeł **parametru DefaultConnection (WebFormsIdentity)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurowanie aplikacji na potrzeby uwierzytelniania OWIN

W tym momencie tylko Dodaliśmy obsługę tworzenia użytkowników. Teraz zamierzamy pokazują, jak można dodać uwierzytelniania logowania użytkownika. Tożsamość platformy ASP.NET używa oprogramowania pośredniczącego uwierzytelniania OWIN Microsoft uwierzytelniania formularzy. Uwierzytelniania plików Cookie OWIN plik cookie i oświadczenia mogą być używane przez wszystkie framework hostowanych na mechanizm uwierzytelniania przy użyciu [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) lub usług IIS. Z tym modelem te same pakiety uwierzytelniania można na wiele platform, na przykład ASP.NET MVC i formularzy sieci Web. Aby uzyskać więcej informacji w projekcie Katana i jak uruchomić oprogramowanie pośredniczące Zobacz o niesprecyzowanym hosta [wprowadzenie projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalowanie pakietów uwierzytelniania do aplikacji

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole tekstowe wyszukiwania, wpisz "*Identity.Owin*". Kliknij przycisk Instaluj tego pakietu.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Wyszukaj pakiet ***Microsoft.Owin.Host.SystemWeb*** i zainstaluj go.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** pakiet zawiera zbiór OWIN rozszerzenie klasy do zarządzania i konfiguracji oprogramowania pośredniczącego uwierzytelniania OWIN do użycia przez pakiety ASP.NET Identity Core.  
    > **Microsoft.Owin.Host.SystemWeb** pakiet zawiera serwer OWIN, który umożliwia aplikacjom na podstawie OWIN w środowisku usług IIS przy użyciu żądania potoku ASP.NET. Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące OWIN w usługach IIS zintegrowane potoku](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Dodawanie klasy konfiguracji uwierzytelniania i uruchamiania OWIN

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, kliknij przycisk **Dodaj**, a następnie **Dodaj nowy element**. W oknie dialogowym pole tekstowe wyszukiwania, wpisz "*owin*". Nazwa klasy "*uruchamiania*" i kliknij przycisk **Dodaj**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. W pliku Startup.cs Dodaj wyróżniony kod przedstawione poniżej, aby skonfigurować uwierzytelniania plików cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Ta klasa zawiera `OwinStartup` atrybut służący do określania klasy początkowej OWIN. Każda aplikacja OWIN ma klasę uruchamiania, której możesz określić składniki do potoku aplikacji. Zobacz [OWIN uruchamiania klasy wykrywania](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) uzyskać więcej informacji dotyczących tego modelu.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Dodawanie formularzy sieci Web rejestracji i logowania użytkowników

1. Otwórz *Register.cs* i Dodaj następujący kod, który będzie zalogować użytkownika podczas rejestracji zakończy się pomyślnie. Zmiany są wyróżnione poniżej.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Ponieważ ASP.NET Identity i uwierzytelniania plików Cookie OWIN systemu oświadczeń, framework wymaga Deweloper aplikacji do wygenerowania [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) dla użytkownika. Identyfikator oświadczenia ma informacje o wszystkich oświadczeń dla użytkownika, takich jak role, jakie użytkownik należy do. Na tym etapie można również dodać więcej oświadczenia dla użytkownika.
    > - Można zalogować użytkownika przy użyciu elementu AuthenticationManager z OWIN i wywoływania `SignIn` i przekazując ClaimsIdentity, jak pokazano powyżej. Ten kod będzie zalogować użytkownika i generowanie również pliku cookie. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) używane przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt kliknięcia **Dodaj**, a następnie **formularza sieci Web**. Nazwa formularza sieci web **logowania**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Zastąp zawartość *Login.aspx* pliku następującym kodem:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Zastąp zawartość *Login.aspx.cs* pliku następującym kodem:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Teraz sprawdza stan bieżącego użytkownika i podejmuje działania na podstawie jego `Context.User.Identity.IsAuthenticated` stanu.  
    >     **Wyświetlić zarejestrowane w nazwie użytkownika** : program Microsoft ASP.NET Identity Framework został dodany metody rozszerzenia w [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) pozwala to uzyskać `UserName` i `UserId` dla zalogowanego użytkownika. Te metody rozszerzenia są definiowane w `Microsoft.AspNet.Identity.Core` zestawu. Te metody rozszerzenia są zastępuje [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metoda logowania:   
    >     `This` metoda zastępuje poprzedniej `CreateUser_Click` metody w tym próbki i teraz loguje użytkownika po pomyślnym utworzeniu użytkownika.   
    >  Microsoft OWIN Framework został dodany metody rozszerzenia w `System.Web.HttpContext` umożliwiająca odwołać się do `IOwinContext`. Te metody rozszerzenia są definiowane w `Microsoft.Owin.Host.SystemWeb` zestawu. `OwinContext` Klasy ujawnia `IAuthenticationManager` właściwość, która reprezentuje funkcje oprogramowania pośredniczącego uwierzytelniania dostępne dla bieżącego żądania.  
    >  Można zalogować użytkownika przy użyciu `AuthenticationManager` OWIN i wywoływania `SignIn` i przekazując `ClaimsIdentity` zgodnie z powyższym.   
    >  Ponieważ ASP.NET Identity i uwierzytelniania plików Cookie OWIN są systemu opartego na oświadczeniach, framework wymaga aplikacji do wygenerowania `ClaimsIdentity` dla użytkownika.   
    >  `ClaimsIdentity` Informacje na temat wszystkich oświadczeń dla użytkownika, takich jak role, jakie należy użytkownik. Możesz także dodać więcej oświadczenia dla użytkownika na tym etapie  
    >  Ten kod będzie zalogować użytkownika i generowanie również pliku cookie. To wywołanie jest odpowiednikiem [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) używane przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
    > - `SignOut` Metoda:   
    >  Pobiera odwołanie do `AuthenticationManager` z OWIN i wywołania `SignOut`. To jest odpowiednikiem [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodę używaną przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu.
5. Naciśnij klawisz **Ctrl + F5** Aby skompilować i uruchomić aplikację sieci web. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Uwaga: W tym momencie nowy użytkownik jest utworzony i zalogowany.
6. Polecenie **Wyloguj się** przycisku. Nastąpi przekierowanie do strony logowania formularza.
7. Wprowadź Nieprawidłowa nazwa użytkownika lub hasło i kliknij **Zaloguj** przycisku.   
   `UserManager.Find` Metoda zwróci wartość null i komunikat o błędzie: " *Nieprawidłowa nazwa użytkownika lub hasło* " będą wyświetlane.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
