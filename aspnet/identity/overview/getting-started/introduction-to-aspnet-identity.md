---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Wprowadzenie do platformy ASP.NET Identity | Dokumentacja firmy Microsoft
author: jongalloway
description: "Systemu członkostwa programu ASP.NET została wprowadzona w programie kopii programu ASP.NET 2.0 w 2005 r. i od, a następnie zaszły dużej liczby zmian w typicall aplikacji sieci web sposoby..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 7c7dcb7903b0d0772acc560161ff39c6869c599a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-identity"></a>Wprowadzenie do tożsamości platformy ASP.NET
====================
przez [Galloway Jan](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

> Systemu członkostwa programu ASP.NET została wprowadzona przy użyciu zwrotnego ASP.NET 2.0 w 2005, ponieważ było wiele zmian w sposób aplikacji sieci web zwykle obsługi uwierzytelniania i autoryzacji, a następnie. ASP.NET Identity jest nowy wygląd, na jakie systemu członkostwa powinny być podczas tworzenia nowoczesnych aplikacji sieci web, telefon lub tablet.
> 
> W tym artykule zapisał Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jan Galloway ([@jongalloway](https://twitter.com/jongalloway)), Dykstra Tomasz i Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Tło: Członkostwo w programie ASP.NET

### <a name="aspnet-membership"></a>Członkostwo ASP.NET

[Członkostwo ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) została zaprojektowana w celu rozwiązania wymagania członkostwa lokacji, które były często używane w 2005 r., uwierzytelnianie formularzy i bazy danych programu SQL Server dla nazwy użytkownika, hasła i dane profilu. Obecnie jest tablicą szerszych wiele opcji przechowywania danych aplikacji sieci web, a większość deweloperów chcesz włączyć ich lokacji do używania dostawcy tożsamości społecznościowych dla funkcji uwierzytelniania i autoryzacji. Ograniczenia dotyczące projektowania członkostwa ASP.NET utrudnić ten proces przejścia:

- Schemat bazy danych została zaprojektowana dla programu SQL Server i nie można go zmienić. Można dodać informacji o profilu, ale dodatkowe dane są pakowane w innej tabeli, która utrudnia dostępu w jakikolwiek sposób z wyjątkiem za pośrednictwem interfejsu API dostawcy profilu.
- Dostawca systemu umożliwia zmianę zapasowego magazynu danych, ale system jest zaprojektowana dla założenia odpowiednie dla relacyjnej bazy danych. Można napisać dostawcy do przechowywania informacji o członkostwie w mechanizm nierelacyjnych magazynu, takich jak tabele magazynu Azure, ale następnie trzeba będzie obejść relacyjne projektowania pisząc dużej ilości kodu i wiele `System.NotImplementedException` wyjątki dla metod, które nie mają zastosowanie do bazy danych NoSQL.
- Ponieważ funkcja dziennika lub dziennika — brak jest oparty na uwierzytelnianie formularzy, nie można użyć systemu członkostwa [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN zawiera składniki oprogramowania pośredniczącego uwierzytelniania, w tym obsługę dodatków dziennika przy użyciu dostawcy tożsamości zewnętrznych (na przykład Accounts firmy Microsoft, Facebook, Google, Twitter) i dziennika dodatków przy użyciu konta organizacyjne z lokalnej usługi Active Directory lub Usługa Azure Active Directory. OWIN obejmuje również obsługę protokołu OAuth 2.0 JWT i CORS.

### <a name="aspnet-simple-membership"></a>Członkostwo proste ASP.NET

[Proste członkostwa ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) został opracowany jako system członkostwa ASP.NET Web Pages. Został wydany z programu WebMatrix i Visual Studio 2010 z dodatkiem SP1. Celem członkostwa prostego było ułatwia dodawanie funkcjonalności członkostwa do aplikacji stron sieci Web.

Członkostwa prostego ułatwić Dostosuj informacje o profilu użytkownika, ale nadal udostępnia inne problemy z członkostwa ASP.NET i ma następujące ograniczenia:

- Był trudny do utrwalenia danych systemu członkostwa w magazynie nierelacyjnych.
- Nie można go używać z oprogramowaniem OWIN.
- Ta funkcja nie działa prawidłowo w przypadku istniejących dostawców członkostwa ASP.NET, a nie jest rozszerzony.

### <a name="aspnet-universal-providers"></a>Standardowi dostawcy ASP.NET

[Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) opracowano pod pozwala zachować informacje o członkostwie w usłudze Microsoft Azure SQL Database i one również współpracować z programu SQL Server Compact. Dostawców uniwersalnych zostały zbudowane na Entity Framework Code First, co oznacza, że dostawców uniwersalnych może służyć do utrwalenia danych w magazynie, wszystkie obsługiwane przez EF. Z dostawców uniwersalnych schemat bazy danych został oczyszczony wielu również.

Dostawców uniwersalnych są wbudowane w infrastrukturze członkostwa ASP.NET, więc one nadal wykonać te same ograniczenia co SqlMembership dostawcy. Oznacza to, że zostały zaprojektowane tak, relacyjnych baz danych i trudno Dostosuj informacje o profilu i użytkownika. Tych dostawców nadal można używać uwierzytelniania formularzy do obsługi funkcji logowania i wyloguj się.

## <a name="aspnet-identity"></a>ASP.NET Identity

Jako członkostwa wątku w programie ASP.NET powstał całościowo, zespół ASP.NET dowiedziała się znacznie od informacji uzyskanych od klientów.

Zakładając, że użytkownicy będą logować, wprowadzając nazwę użytkownika i hasło, które w zarejestrowani w swojej aplikacji nie jest już prawidłowy. Witryna sieci web stał się bardziej społecznościowych. Użytkownicy interakcji są ze sobą w czasie rzeczywistym za pomocą kanałów społecznościowych, takich jak Facebook, Twitter i innych społecznościowych witryn sieci web. Deweloperzy mają użytkownicy mogli Zaloguj się przy użyciu tożsamości społecznościowych, dzięki czemu mogą oni mieć bogate środowisko w swoich witrynach sieci web. System członkostwa nowoczesnych należy włączyć na podstawie przekierowania dziennika dodatków do dostawców uwierzytelniania, takich jak Facebook, Twitter i inne.

Jak ewoluował projektowanie witryn sieci web, tak samo wzorce projektowanie witryn sieci web. Jednostka testowania kodu aplikacji stał się podstawowy problem dotyczący dla deweloperów aplikacji. 2008 ASP.NET dodać nowej struktury oparte na wzorcu Model-widok-kontroler (MVC) w części, aby pomóc deweloperom tworzenie jednostki konstruowanie aplikacji ASP.NET. Deweloperzy, którzy chcieli jednostki testu ich logiki aplikacji również chcieli można to zrobić przy użyciu systemu członkostwa.

Biorąc pod uwagę te zmiany w tworzenie aplikacji sieci web ASP.NET Identity został opracowany z następujących celów:

- **Jeden system tożsamości platformy ASP.NET**

    - ASP.NET Identity można biorąc pod uwagę następujące platformy ASP.NET, takich jak ASP.NET MVC, Web Forms stron sieci Web, interfejsu API sieci Web i SignalR.
    - Tożsamości platformy ASP.NET można użyć podczas tworzenia aplikacji sieci web, telefon, magazynu lub hybrydowego.
- **Łatwość podłączania dane profilu użytkownika**

    - Masz kontrolę nad schemat użytkownika oraz informacje o profilu. Na przykład można łatwo włączyć systemu do przechowywania daty urodzenia wprowadzone przez użytkowników, po zarejestrowaniu konta w aplikacji.

- **Formant trwałości**

    - Domyślnie system tożsamości ASP.NET przechowuje wszystkie informacje o użytkowniku w bazie danych. ASP.NET Identity Entity Framework Code First używa do wdrożenia całości mechanizmu stanu trwałego.
    - Ponieważ kontrolować schematu bazy danych, typowych zadań, takich jak zmiana nazwy tabeli lub zmiana typu danych kluczy podstawowych jest proste zrobić.
    - Ułatwia Podłącz mechanizmów innego magazynu, takich jak SharePoint, z tabeli magazynu Azure, bazy danych NoSQL, itp., bez konieczności throw `System.NotImplementedExceptions` wyjątków.
- **Testowania jednostki**

    - ASP.NET Identity sprawia, że aplikacja sieci web jednostki więcej testować. Można pisać testy jednostkowe dla części aplikacji korzystających z tożsamości ASP.NET.
- **Dostawca roli**

    - Brak dostawcy roli, co umożliwia ograniczenie dostępu do części aplikacji przez role. Można łatwo tworzyć role, takie jak "Admin" i dodać użytkowników do ról.
- **Oświadczeń**

    - ASP.NET Identity obsługuje uwierzytelnianie oparte na oświadczeniach, gdy tożsamość użytkownika jest reprezentowany jako zestaw oświadczeń. Oświadczenia umożliwiają deweloperom można znacznie bardziej obszerne w opisujące tożsamości użytkownika, niż pozwala rola. Członkostwo roli jest tylko wartość logiczną (elementu członkowskiego lub nie jest członkiem), oświadczenia mogą obejmować zaawansowanych informacji o tożsamości użytkownika i członkostwa.
- **Dostawców logowania społecznościowych**

    - Możesz łatwo dodać społecznościowych dodatków dziennika takich jak Account Microsoft, Facebook, Twitter, Google i inne osoby do aplikacji i przechowywać dane specyficzne dla użytkownika w aplikacji.
- **Azure Active Directory**

    - Można również dodaje funkcji obsługi logowania za pomocą usługi Azure Active Directory i przechowywać dane specyficzne dla użytkownika w aplikacji. Aby uzyskać więcej informacji, zobacz [konta organizacyjne](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) w tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013
- **Integracja OWIN**

    - Uwierzytelnianie ASP.NET jest teraz oparte na oprogramowanie pośredniczące OWIN służący na żadnym hoście standardem OWIN. Tożsamość platformy ASP.NET nie ma żadnych zależności na System.Web. Jest całkowicie zgodny framework OWIN i mogą być używane w dowolnej aplikacji hostowanej OWIN.
    - ASP.NET Identity korzysta z uwierzytelniania OWIN dla dziennika lub dziennika — Brak użytkowników w witrynie sieci web. Oznacza to, że zamiast do wygenerowania pliku cookie uwierzytelniania formularzy, aplikacja używa OWIN CookieAuthentication w tym celu.
- **Pakiet NuGet**

    - ASP.NET Identity jest dystrybuowany jako pakietu NuGet, który jest instalowany w szablonach ASP.NET MVC i formularzy sieci Web interfejsu API sieci Web, które są dostarczane z programu Visual Studio 2013. Ten pakiet NuGet można pobrać z galerii NuGet.
    - Zwalnianie ASP.NET Identity jako NuGet pakietu ułatwia zespół ASP.NET iterację nowe funkcje i poprawki błędów i dostarczanie tych dla deweloperów w sposób elastyczne.

## <a name="getting-started-with-aspnet-identity"></a>Wprowadzenie do korzystania z tożsamości platformy ASP.NET

ASP.NET Identity jest używana w szablonach projektu programu Visual Studio 2013 dla platformy ASP.NET MVC i formularzy sieci Web, interfejs API sieci Web SPA. W tym przewodniku możemy zilustrować, jak szablony projektów za pomocą tożsamości ASP.NET dodaje funkcjonalność się zarejestrować, zaloguj się i wylogowania użytkownika.

ASP.NET Identity jest implementowane za pomocą poniższej procedury. Celem tego artykułu jest zapewnienie wysokiego poziomu omówienie ASP.NET Identity; Możesz wykonać krok po kroku lub tylko odczytać szczegółów. Bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji przy użyciu tożsamości ASP.NET, w tym przy użyciu nowego interfejsu API, aby dodać użytkowników ról i informacje o profilu, zobacz sekcję następne kroki na końcu tego artykułu.

1. Tworzenie aplikacji platformy ASP.NET MVC z indywidualnych kont. Możesz użyć ASP.NET Identity w ASP.NET MVC, formularzy sieci Web, Web API SignalR itp. W tym artykule Rozpoczniemy z aplikacji platformy ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Utworzony projekt zawiera następujące trzy pakiety dla tożsamości ASP.NET.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 Ten pakiet zawiera implementacji programu Entity Framework tożsamości platformy ASP.NET, w którym będzie umieszczony ASP.NET Identity danych i schemat z programem SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 Ten pakiet zawiera interfejsów podstawowych dla tożsamości ASP.NET. Ten pakiet można zapisać implementację ASP.NET Identity czy trwałości różnych celów przechowuje takie jak magazyn tabel Azure, NoSQL bazy danych itp.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 Ten pakiet zawiera funkcje, które służy do dodatku uwierzytelniania OWIN o tożsamości platformy ASP.NET w aplikacji ASP.NET. To jest używany podczas dodawania dziennika funkcji do aplikacji i wywołanie oprogramowanie pośredniczące uwierzytelniania plików Cookie OWIN do wygenerowania pliku cookie.
3. Tworzenie użytkownika.  
 Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącze, aby utworzyć użytkownika. Na poniższej ilustracji przedstawiono strony rejestru, które zbiera informacje o nazwę użytkownika i hasło.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 Po kliknięciu przez użytkownika **zarejestrować** przycisku `Register` akcji kontrolera konto tworzy użytkownika przez wywołanie interfejsu API tożsamości platformy ASP.NET, wyróżniono poniżej:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Zaloguj się.  
 Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany za `SignInAsync` metody.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 Wyróżniony kod powyżej w `SignInAsync` metoda generuje [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Ponieważ ASP.NET Identity i uwierzytelniania plików Cookie OWIN systemu opartego na oświadczeniach, framework wymaga aplikacji do wygenerowania ClaimsIdentity dla użytkownika. Identyfikator oświadczenia ma informacje o wszystkich oświadczenia dla użytkownika, takich jak role, jakie użytkownik należy do. Na tym etapie można również dodać więcej oświadczenia dla użytkownika.  
  
 Wyróżniony kod poniżej w `SignInAsync` metody loguje użytkownika przy użyciu elementu AuthenticationManager z OWIN i wywoływania `SignIn` i przekazując ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Wyloguj.  
 Kliknięcie przycisku **wylogować** łącze wywołuje operację wylogowania w kontrolerze konta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 Zaznaczony kod powyżej przedstawiono OWIN `AuthenticationManager.SignOut` metody. To jest odpowiednikiem [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodę używaną przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu w formularzach sieci Web.

## <a name="components-of-aspnet-identity"></a>Składniki tożsamości platformy ASP.NET

Na poniższym diagramie przedstawiono składniki systemu tożsamości platformy ASP.NET (kliknij [to](introduction-to-aspnet-identity/_static/image3.png) lub na diagramie, aby je powiększyć). Pakiety na zielono tworzą system tożsamości ASP.NET. Wszystkie pakiety są zależności, które są niezbędne do używania systemu tożsamości platformy ASP.NET w aplikacji ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Poniżej znajduje się krótki opis pakietów NuGet nie wymienione wcześniej:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Oprogramowanie pośredniczące, który umożliwia aplikacji na używanie plików cookie na podstawie uwierzytelniania, podobnie jak ASP. Uwierzytelnianie formularzy w sieci.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework jest technologii dostępu do danych zalecane przez firmę Microsoft relacyjnych baz danych.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrowanie z członkostwa do tożsamości platformy ASP.NET

Mamy nadzieję szybko zawierają wskazówki na temat migracji istniejące aplikacje wykorzystujące do nowego systemu ASP.NET Identity członkostwa ASP.NET lub członkostwa prostego.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Samouczek używa interfejsu API programu ASP.NET Identity można dodać informacji o profilu w bazie danych użytkownika oraz sposobu uwierzytelniania w usłudze Google i Facebook.
- [Tworzenie aplikacji platformy ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 W tym samouczku przedstawiono sposób dodawania użytkowników i ról za pomocą interfejsu API tożsamości.
- [Indywidualne konta użytkowników](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) podczas tworzenia sieci Web ASP.NET projektów programu Visual Studio 2013
- [Konta organizacyjne](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) podczas tworzenia sieci Web ASP.NET projektów programu Visual Studio 2013
- [Dostosowywanie informacji o profilu w produkcie ASP.NET Identity w szablonach VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Uzyskaj więcej informacji od dostawców sieci społecznościowych, używane w szablonach projektu programu VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Przykładowa aplikacja obsługująca przedstawiono sposób dodawania ról podstawowych i obsługa użytkowników i ról i zarządzanie użytkownikami.
