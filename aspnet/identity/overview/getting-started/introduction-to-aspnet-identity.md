---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Wprowadzenie do produktu ASP.NET Identity | Dokumentacja firmy Microsoft
author: jongalloway
description: Systemu członkostwa programu ASP.NET została wprowadzona w systemach kopii programu ASP.NET 2.0 w wersji 2005 i od czasu, a następnie typicall aplikacji sieci web sposoby miały miejsce wiele zmian...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 39a6f0195c407403b7bd7e2f1eb5b52c822b52a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402499"
---
<a name="introduction-to-aspnet-identity"></a>Wprowadzenie do produktu ASP.NET Identity
====================
przez [Galloway'em Jon](https://github.com/jongalloway), [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> Systemu członkostwa programu ASP.NET została wprowadzona w systemach kopii programu ASP.NET 2.0 w wersji 2005 i od czasu, a następnie w sposób, w aplikacji sieci web zazwyczaj obsługiwać uwierzytelnianie i autoryzacja było wiele zmian. ASP.NET Identity jest nowy wygląd, na jakie systemu członkostwa powinny być podczas kompilowania nowoczesnych aplikacji dla sieci web, telefonu lub tabletu.
> 
> W tym artykule został napisany przez autorem jest Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jan Galloway'em ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra i Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Tło: Członkostwo w programie ASP.NET:

### <a name="aspnet-membership"></a>Członkostwo ASP.NET

[Członkostwo ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) zaprojektowano tak, aby rozwiązać wymagania dotyczące członkostwa w lokacji, które są wspólne w 2005 uwierzytelniania formularzy i bazy danych programu SQL Server dla nazwy użytkownika, hasła i dane profilu. Obecnie istnieje znacznie szerszy tablicą opcji przechowywania danych dla aplikacji sieci web i większość deweloperów chcesz włączyć w swoich witrynach dostawców tożsamości społecznościowych na użytek funkcji uwierzytelniania i autoryzacji. Ograniczenia dotyczące projektowania członkostwa ASP.NET utrudniają to przejście:

- Schemat bazy danych została zaprojektowana dla programu SQL Server i nie można go zmienić. Można dodać informacji o profilu, ale dodatkowe dane są pakowane w innej tabeli, co utrudnia na dostęp za pomocą jakichkolwiek środków, z wyjątkiem za pośrednictwem interfejsu API dostawcy profilu.
- System dostawcy umożliwia zmianę zapasowy magazyn danych, ale system zaprojektowany pod kątem założeń, które są odpowiednie w przypadku relacyjnej bazy danych. Można napisać dostawcy do przechowywania informacji o członkostwie w mechanizm magazynu nierelacyjnych, takich jak tabele magazynu platformy Azure, ale następnie trzeba obejść projektowania relacyjnych, pisząc duże ilości kodu i wiele `System.NotImplementedException` wyjątków dla metod, które nie mają zastosowanie do baz danych NoSQL.
- Ponieważ funkcje w/log Wyloguj opiera się na uwierzytelnianie formularzy, nie można użyć systemu członkostwa [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN obejmuje składniki oprogramowania pośredniczącego uwierzytelniania, takie jak Obsługa dziennika dodatków przy użyciu dostawców tożsamości zewnętrznych (takich jak Accounts firmy Microsoft, Facebook, Google i Twitter) i dziennika dodatków przy użyciu konta organizacji z usługi Active Directory lokalnych lub Usługa Azure Active Directory. OWIN obejmuje również obsługę protokołu OAuth 2.0 JWT i mechanizmu CORS.

### <a name="aspnet-simple-membership"></a>Członkostwo ASP.NET prosty

[Proste członkostwa ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) został opracowany jako system członkostwa ASP.NET Web Pages. Został wydany przy użyciu programu WebMatrix i Visual Studio 2010 SP1. Celem członkostwa prostego był ułatwia dodawanie funkcji przynależności do aplikacji stron sieci Web.

Członkostwa prostego ułatwiają dostosowywanie informacji o profilu użytkownika, ale nadal udostępnia inne problemy z członkostwa ASP.NET i ma pewne ograniczenia:

- Był trudny do utrwalenia danych systemu członkostwa w magazynie nierelacyjnych.
- Nie można go używać z oprogramowaniem OWIN.
- Nie działa dobrze z istniejącego dostawcy członkostwa ASP.NET, a nie jest rozszerzalny.

### <a name="aspnet-universal-providers"></a>Standardowi dostawcy ASP.NET

[Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) zostały zaprojektowane, aby umożliwić utrwalanie informacji o członkostwie w systemie Microsoft Azure SQL Database i mogą również działać przy użyciu programu SQL Server Compact. Dostawców uniwersalnych zostały zbudowane Entity Framework Code First, co oznacza, że dostawców uniwersalnych może służyć do utrwalenia danych w magazynie, wszystkie objęte EF. Za pomocą dostawców uniwersalnych schemat bazy danych został oczyszczony z bardzo dużo także.

Dostawców uniwersalnych są tworzone w infrastrukturze członkostwa ASP.NET, więc one nadal wykonać te same ograniczenia jako dostawca SqlMembership. Oznacza to, że zostały zaprojektowane dla relacyjnych baz danych i trudno Dostosuj informacje o profilu i użytkownika. Ci dostawcy nadal można używać uwierzytelniania formularzy dla funkcji logowania i Wyloguj.

## <a name="aspnet-identity"></a>ASP.NET Identity

Jako członkostwa wątku na platformie ASP.NET powstał w ciągu lat zespół programu ASP.NET ma umożliwił zdobycie z opinii klientów.

Przy założeniu, że użytkownicy będą logować się, wprowadzając nazwę użytkownika i hasło, które zarejestrowanych w swojej aplikacji nie jest już prawidłowy. Stała się bardziej społecznościowych sieci web. Użytkownicy wchodzą w interakcje ze sobą w czasie rzeczywistym za pośrednictwem kanałów społecznościowych, takich jak Facebook, Twitter i innych społecznościowych witryn sieci web. Deweloperzy chcesz, aby użytkownicy mogli zalogować się przy użyciu ich tożsamości społecznościowych, dzięki czemu mają bogate doświadczenie w swoich witrynach sieci web. System członkostwa nowoczesnych należy włączyć na podstawie przekierowania dziennika dodatki plug-in do dostawców uwierzytelniania, takich jak Facebook, Twitter i innych.

Jak ewoluował programowania dla sieci web, tak samo wzorce programowania dla sieci web. Jednostki testowania kodu aplikacji stały się podstawowe znaczenie dla deweloperów aplikacji. W 2008 ASP.NET dodaje nowej struktury, na podstawie wzorca Model-View-Controller (MVC), w części, aby pomóc deweloperom w tworzeniu jednostki sprawdzalnego działa zgodnie aplikacji ASP.NET. Deweloperzy, którzy chcieli jednostki testowania ich logiki aplikacji, które również chce, aby można było to zrobić za pomocą systemu członkostwa.

Biorąc pod uwagę te zmiany podczas tworzenia aplikacji sieci web ASP.NET Identity został opracowany przy użyciu następujących celów:

- **Jeden system produktu ASP.NET Identity**

    - ASP.NET Identity może służyć w przypadku wszystkich platform ASP.NET, takich jak ASP.NET MVC, formularze sieci Web, stron sieci Web, internetowy interfejs API i SignalR.
    - ASP.NET Identity może służyć podczas tworzenia aplikacji sieci web, telefonu, magazynu lub hybrydowego.
- **Łatwość podłączania dane profilu użytkownika**

    - Masz kontrolę nad schemat użytkownika oraz informacje o profilu. Na przykład można łatwo włączyć system do przechowywania daty urodzenia wprowadzone przez użytkowników, po zarejestrowaniu konta w aplikacji.

- **Kontrola stanu trwałego**

    - Domyślnie system produktu ASP.NET Identity przechowuje wszystkie informacje o użytkowniku w bazie danych. Do wdrożenia całości mechanizmu stanu trwałego produktu ASP.NET Identity korzysta z programu Entity Framework Code First.
    - Ponieważ możesz kontrolować schematu bazy danych, typowych zadań, takich jak zmiana nazwy tabel lub zmiana typu danych kluczy podstawowych jest prosty sposób.
    - Można łatwo podłączyć mechanizmów innego magazynu, takich jak SharePoint, usługę Azure Storage Table Service, bazy danych NoSQL, itp., bez konieczności throw `System.NotImplementedExceptions` wyjątków.
- **Jednostka testowalności**

    - ASP.NET Identity sprawia, że aplikacja sieci web więcej jednostek sprawdzalnego działa zgodnie. Można pisać testy jednostkowe dla części aplikacji, które używają produktu ASP.NET Identity.
- **Dostawca ról**

    - Brak dostawcy ról, które pozwala ograniczyć dostęp do poszczególnych części aplikacji przez role. Możesz łatwo tworzyć role, takie jak "Admin" i dodać użytkowników do ról.
- **Na podstawie oświadczeń**

    - ASP.NET Identity obsługuje uwierzytelniania opartego na oświadczeniach, gdy tożsamość użytkownika jest reprezentowana jako zestaw oświadczeń. Oświadczenia umożliwiają deweloperom można o wiele bardziej ekspresyjnego do opisywania tożsamości użytkownika, niż jest dozwolone role. Członkostwo w roli jest po prostu atrybut typu wartość logiczna (składowej lub nie jest członkiem), oświadczenia mogą zawierać bogate informacje o tożsamości i członkostwa użytkownika.
- **Dostawców społecznościowych logowania**

    - Możesz łatwo dodać społecznościowych dodatków dziennika, takich jak Account Microsoft, Facebook, Twitter, Google i innych użytkowników do aplikacji i przechowywać dane specyficzne dla użytkownika w aplikacji.
- **Azure Active Directory**

    - Można również dodać funkcję z logowania za pomocą usługi Azure Active Directory i przechowywać dane specyficzne dla użytkownika w aplikacji. Aby uzyskać więcej informacji, zobacz [kont organizacyjnych](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) w tworzeniu projektów sieci Web platformy ASP.NET w programie Visual Studio 2013
- **Integracja z OWIN**

    - Uwierzytelnianie ASP.NET teraz opiera się na oprogramowania pośredniczącego OWIN, które mogą być używane na dowolnego hosta, na podstawie OWIN. ASP.NET Identity nie ma żadnych zależności na System.Web. Jest w pełni zgodne framework OWIN i mogą być używane w dowolnej aplikacji OWIN hostowanej.
    - ASP.NET Identity korzysta z uwierzytelniania OWIN w/log Wyloguj użytkowników w witrynie sieci web. Oznacza to, że zamiast korzystać z uwierzytelniania formularzy, aby wygenerować plik cookie, aplikacja używa OWIN CookieAuthentication w tym.
- **Pakiet NuGet**

    - ASP.NET Identity jest dystrybuowany jako pakiet NuGet, który jest zainstalowany w szablonach platformy ASP.NET MVC i formularzy sieci Web interfejsu API sieci Web, które są dostarczane z programem Visual Studio 2013. Ten pakiet NuGet można pobrać z galerii pakietów NuGet.
    - Zwalnianie produktu ASP.NET Identity jako NuGet pakietu ułatwia zespół programu ASP.NET do iteracji na nowe funkcje i poprawki błędów i dostarczaj je deweloperom zwinne.

## <a name="getting-started-with-aspnet-identity"></a>Wprowadzenie do produktu ASP.NET Identity

ASP.NET Identity jest używany w szablonach projektu programu Visual Studio 2013 dla platformy ASP.NET MVC i formularzy sieci Web, internetowy interfejs API SPA. W ramach tego przewodnika firma Microsoft pokazują, jak szablony projektów umożliwiają produktu ASP.NET Identity dodawania funkcjonalności do rejestracji, zaloguj się i wylogowania użytkownika.

ASP.NET Identity jest implementowany przy użyciu poniższej procedury. Celem tego artykułu jest zapewnienie wysokiego poziomu omówienie produktu ASP.NET Identity; można wykonać krok po kroku lub tylko odczytać szczegóły. Aby uzyskać bardziej szczegółowe instrukcje na temat tworzenia aplikacji za pomocą tożsamości ASP.NET, w tym za pomocą nowego interfejsu API, aby dodać użytkowników, role i informacje o profilu Zobacz sekcji Następne kroki na końcu tego artykułu.

1. Tworzenie aplikacji ASP.NET MVC za pomocą indywidualnych kont. Możesz użyć produktu ASP.NET Identity w ASP.NET MVC, Web Forms, internetowy interfejs API itp SignalR. W tym artykule Zaczniemy z aplikacją ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Utworzony projekt zawiera następujące trzy pakiety dla produktu ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Ten pakiet zawiera implementację programu Entity Framework tożsamości ASP.NET, która zostanie utrzymany danych produktu ASP.NET Identity i schematów do programu SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Ten pakiet zawiera interfejsy podstawowe dla produktu ASP.NET Identity. Ten pakiet można napisać implementację dla produktu ASP.NET Identity, że elementy docelowe różnych stanów trwałych magazyny, takie jak Azure Table Storage, NoSQL baz danych itp.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Ten pakiet zawiera funkcje, które służy do dodatku w aplikacjach ASP.NET uwierzytelniania OWIN w produkcie ASP.NET Identity. To jest używana podczas dodawania dziennika funkcji do aplikacji i wywołać oprogramowanie pośredniczące uwierzytelniania plików Cookie OWIN do generowania pliku cookie.
3. Tworzenie użytkownika.  
   Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** link, aby utworzyć użytkownika. Na poniższej ilustracji przedstawiono na stronie rejestru, który gromadzi informacje o nazwę użytkownika i hasło.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Kiedy użytkownik kliknie **zarejestrować** przycisku `Register` akcji kontrolera konto tworzy użytkownika, wywołując Identity interfejsu API ASP.NET, jak wyróżniono poniżej:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Zaloguj się.  
   Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany za `SignInAsync` metody.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   Wyróżniony kod powyżej `SignInAsync` metoda generuje [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Ponieważ produktu ASP.NET Identity i uwierzytelniania plików Cookie OWIN systemu opartego na oświadczeniach, struktura wymaga aplikację, aby wygenerować ClaimsIdentity dla użytkownika. Tożsamość ClaimsIdentity zawierają informacje dotyczące wszystkich oświadczeń użytkownika, takich jak role, jakie należy użytkownik. Można również dodać więcej oświadczenia dla użytkownika na tym etapie.  
  
   Wyróżniony kod poniżej `SignInAsync` metoda loguje użytkownika za pomocą słowniku z OWIN i wywoływania `SignIn` i przekazując ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Wyloguj się.  
   Klikając **wylogować** linku wywołuje operację wylogowania w kontrolerze konta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Wyróżniony kod powyżej przedstawiono OWIN `AuthenticationManager.SignOut` metody. To jest odpowiednikiem [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodę używaną przez [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modułu w formularzach sieci Web.

## <a name="components-of-aspnet-identity"></a>Składniki produktu ASP.NET Identity

Na poniższym diagramie przedstawiono składniki systemu produktu ASP.NET Identity (kliknij [to](introduction-to-aspnet-identity/_static/image3.png) lub na diagramie Aby powiększyć ją). Pakiety w kolorze zielonym tworzą system tożsamości ASP.NET. Inne pakiety są zależności, które są potrzebne do korzystania z systemu tożsamości ASP.NET w aplikacji ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Poniżej przedstawiono krótki opis pakiety NuGet nie są wymienione wcześniej:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Oprogramowanie pośredniczące, który umożliwia aplikacji na używanie plików cookie na podstawie uwierzytelniania, podobnie jak ASP. Uwierzytelnianie formularzy w sieci.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework jest technologii dostępu do danych zalecane przez firmę Microsoft dla relacyjnych baz danych.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrowanie z członkostwa do produktu ASP.NET Identity

Mamy nadzieję, że wkrótce zapewnić wskazówki dotyczące migrowania istniejących aplikacji, które używają członkostwa ASP.NET lub członkostwa prostego do nowego systemu tożsamości ASP.NET.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Interfejs API platformy ASP.NET Identity jest samouczku można dodać informacji o profilu do bazy danych użytkowników i sposób uwierzytelniania za pomocą serwis Google czy Facebook.
- [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 W tym samouczku pokazano, jak dodać użytkowników i ról za pomocą interfejsu API tożsamości.
- [Indywidualne konta użytkowników](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) podczas tworzenia projektów sieci Web platformy ASP.NET, w programie Visual Studio 2013
- [Konta organizacyjne](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) podczas tworzenia projektów sieci Web platformy ASP.NET, w programie Visual Studio 2013
- [Dostosowywanie informacje o profilu w produkcie ASP.NET Identity w szablonach VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Więcej informacji można uzyskać od dostawców sieci społecznościowych, używane w szablonach projektu programu VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Przykładowa aplikacja pokazuje, jak dodać podstawowe role i pomoc dla użytkowników i jak to zrobić, ról i zarządzanie użytkownikami.
