---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure uwierzytelniania | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Narzędzia Microsoft ASP.NET dla usługi Windows Azure Active Directory pozwala w prosty sposób włączania uwierzytelniania dla aplikacji sieci web hostowanych w witrynach sieci Web Azure Windows...
ms.author: aspnetcontent
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: e3443fb627dc8d7d5011341828556b4c13836170
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812817"
---
<a name="windows-azure-authentication"></a>Windows Azure uwierzytelniania
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET narzędzi dla usługi Windows Azure Active Directory pozwala w prosty sposób włączania uwierzytelniania dla aplikacji sieci web hostowanych na [Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/). Uwierzytelnianie w usłudze Windows Azure służy do uwierzytelniania użytkowników usługi Office 365 z Twojej organizacji, kont firmowych synchronizowanych z lokalnej usługi Active Directory lub użytkownicy utworzeni w własnej domenie niestandardowej usługi Windows Azure Active Directory. Włączanie uwierzytelniania opartego na Windows Azure umożliwia skonfigurowanie aplikacji w celu uwierzytelniania użytkowników za pomocą pojedynczej [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) dzierżawy.
> 
> Narzędzie uwierzytelniania platformy ASP.NET Windows Azure nie jest obsługiwane dla ról sieci web w usłudze w chmurze, ale planujemy to zrobić w przyszłej wersji. [Program Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) jest obsługiwana w rolach sieci web usługi Windows Azure.
> 
> Aby uzyskać szczegółowe informacje dotyczące sposobu konfigurowania synchronizacji między lokalnej usługi Active Directory i dzierżawy usługi Windows Azure Active Directory można znaleźć [usług AD FS 2.0 do wdrażania i zarządzania nimi logowanie jednokrotne](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory jest obecnie dostępna jako [bezpłatne usługi w wersji zapoznawczej](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Wymagania:

- Program Visual Studio 2012 lub [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web Tools Extensions dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) lub [rozszerzeń narzędzi sieci Web dla programu Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for Windows usługi Azure Active Directory — program Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) lub [Microsoft ASP.NET narzędzia dla programu Windows Azure z usługą Active Directory programu Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Tworzenie aplikacji sieci Web platformy ASP.NET za pomocą programu Visual Studio 2012

Można utworzyć dowolnej aplikacji sieci Web za pomocą programu Visual Studio 2012, w tym samouczku korzysta z szablonu platformy ASP.NET MVC w sieci intranet.

1. Tworzenie nowej aplikacji intranetowych 4 platformy ASP.NET MVC i Zaakceptuj wszystkie ustawienia domyślne. (Musi to być In **tra** netto a nie w **warunków** netto projektu).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Włącz uwierzytelnianie w usłudze Azure okna (Jeśli jesteś administratorem globalnym główną)

Jeśli nie masz istniejącej dzierżawy usługi Windows Azure Active Directory (na przykład za pomocą istniejącego konta usługi Office 365) można utworzyć nową dzierżawę, logując się w celu [nowego konta usługi Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. Wybierz z menu projektu **Włącz uwierzytelnianie usługi Azure Windows**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Wprowadź domenę do dzierżawy usługi Windows Azure Active Directory (np. contoso.onmicrosoft.com), a następnie kliknij przycisk **Włącz**:

![](windows-azure-authentication/_static/image3.png)

3. W sieci Web uwierzytelniania okna dialogowego logowania jako administrator dzierżawy usługi Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Włącz Windows Azure, innej niż administrator z główną

Jeśli nie masz uprawnień administratora globalnego dla dzierżawy usługi Windows Azure Active Directory, możesz usunąć zaznaczenie pola wyboru dotyczącego udostępniania aplikacji.

![](windows-azure-authentication/_static/image6.png)

Wyświetli się okno dialogowe **domeny**, **identyfikator podmiotu zabezpieczeń aplikacji** i **adres URL odpowiedzi** które są wymagane do obsługi administracyjnej aplikacji za pomocą usługi Azure Active Directory główną. Musisz podać te informacje do osoby, która ma wystarczające uprawnienia do udostępniania aplikacji. Zobacz[sposób implementowania logowania jednokrotnego z usługą Windows Azure Active Directory — aplikacja ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) szczegółowe informacje na temat ręcznego tworzenia jednostki usługi przy użyciu polecenia cmdlet.  
Gdy aplikacja została pomyślnie aprowizowano, możesz kliknąć **kontynuowania aktualizacji pliku web.config z wybranymi ustawieniami**. Jeśli chcesz kontynuować tworzenie aplikacji podczas oczekiwania na potrzeby inicjowania obsługi administracyjnej i tak się stanie po kliknięciu **Zamknij, aby pamiętać ustawienia w pliku projektu**. Podczas następnego wywołania Włączanie uwierzytelniania w programie Windows Azure i usunąć zaznaczenie pola wyboru inicjowania obsługi administracyjnej, zobaczysz tych samych ustawień i możesz kliknąć pozycję **Kontynuuj**, następnie kliknij przycisk **zastosować te ustawienia w pliku web.config**.

1. Zaczekaj, aż aplikacja jest skonfigurowany na potrzeby uwierzytelniania Azure Windows i obsługiwana za pomocą usługi Windows Azure Active Directory.
2. Po włączeniu uwierzytelnianie w usłudze Windows Azure dla aplikacji, kliknij przycisk **Zamknij:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Naciśnij klawisz F5, aby uruchomić aplikację. Należy uzyskać możesz automatyczne przekierowanie do strony logowania. Korzystanie z poświadczeń użytkownika katalogu główną, aby zalogować się do aplikacji...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Ponieważ aplikacja jest obecnie używany certyfikat testowy z podpisem własnym, zostanie wyświetlone ostrzeżenie z przeglądarki, że certyfikat nie został wystawiony przez zaufany urząd certyfikacji.

    To ostrzeżenie można bezpiecznie zignorować podczas rozwoju lokalnego, klikając **Kontynuuj przeglądanie tej witryny sieci Web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Możesz teraz pomyślnie zalogowali się do aplikacji za pomocą uwierzytelnianie w usłudze Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Włączanie usługi Windows Azure uwierzytelniania wprowadza następujące zmiany do swojej aplikacji:

- Przed fałszerstwem żądań przed między lokacji ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) klasy ( *aplikacji\_Start\AntiXsrfConfig.cs* ) jest dodawany do projektu.
- Pakiety NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` zostanie dodany do projektu.
- Ustawienia programu Windows Identity Foundation w aplikacji zostaną skonfigurowane w taki sposób, aby zaakceptować tokeny zabezpieczające od dzierżawy usługi Windows Azure Active Directory. Kliknij poniżej, aby wyświetlić rozszerzony widok zmiany wprowadzone do obrazu *Web.config* pliku.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Jednostki usługi dla aplikacji w dzierżawie usługi Windows Azure Active Directory będzie gotowa.
- Protokół HTTPS jest włączona.

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji na platformie Windows Azure

Aby uzyskać pełne instrukcje, zobacz [wdrażanie aplikacji sieci Web ASP.NET do witryny sieci Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Aby opublikować aplikację przy użyciu uwierzytelnianie w usłudze Windows Azure do witryny sieci Web platformy Azure:

1. Kliknij prawym przyciskiem myszy aplikację i wybierz **publikowania:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. W oknie dialogowym Publikowanie w sieci Web należy pobrać i zaimportować profil publikowania dla witryny sieci Web platformy Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Połączenia** karcie pokazuje **docelowy adres URL** (publiczny dostępny adres URL dla aplikacji). Kliknij przycisk **Waliduj połączenie** Aby przetestować swoje połączenie:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Jeśli publikujesz do tej witryny sieci Web Azure wcześniej należy wziąć pod uwagę sprawdzanie **Usuń dodatkowe pliki w lokalizacji docelowej** ustawienie, aby upewnić się, aplikacja publikuje nie pozostawia żadnych śladów. Zwróć uwagę **Włącz uwierzytelnianie usługi Azure Windows** wybranych jest pole wyboru.  

    ![](windows-azure-authentication/_static/image10.png)
5. Opcjonalnie: Na **Podgląd** kliknij kartę **Uruchom Podgląd** Aby wyświetlić pliki, wdrożone.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Kliknij przycisk **publikowania.**

    Zostanie wyświetlony monit włączyć uwierzytelnianie w usłudze Windows Azure dla hosta docelowego. Kliknij przycisk **Włącz** aby kontynuować:

    ![](windows-azure-authentication/_static/image11.png)
7. Wprowadź swoje poświadczenia administratora dla dzierżawy usługi Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Po pomyślnym opublikowaniu aplikacji opublikowanej witryny sieci web zostanie otwarta w przeglądarce.

    > [!NOTE]
    > Może upłynąć do pięciu minut (zwykle musi mniej) dla aplikacji do w pełni aprowizowane za pomocą usługi Windows Azure Active Directory po włączeniu uwierzytelnianie w usłudze Windows Azure dla hosta docelowego. Przy pierwszym uruchomieniu aplikacji, jeśli zostanie wyświetlony błąd ACS50001: nie można odnaleźć jednostki uzależnionej o nazwie [obszaru], a następnie zaczekaj kilka minut i spróbuj uruchomić ponownie aplikację.
9. Po wyświetleniu monitu zaloguj się jako użytkownik w Twoim katalogu:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Masz teraz pomyślnie zalogowano się do subskrypcji platformy Azure hostowanej aplikacji przy użyciu uwierzytelnianie w usłudze Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Znane problemy

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autoryzacja oparta na rolach nie powiedzie się, podczas uwierzytelniania Windows Azure < o:p >< / o:p >

Uwierzytelnianie w usłudze Windows Azure nie zapewnia obecnie oświadczeń wymagane role, dzięki czemu można przeprowadzić autoryzacji opartej na rolach. Rola użytkownika uwierzytelnionego musi zostać ręcznie pobrany z Windows Azure Active Directory. < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Przechodzenie do aplikacji za pomocą uwierzytelnianie w usłudze Windows Azure powoduje błąd "ACS20016 domeny zalogowanego użytkownika (live.com) nie odpowiada żadnym allowed domeny tej usługi STS" < o:p >< / o:p >

Jeśli już zalogowano się do Account firmy Microsoft (np. hotmail.com, live.com, outlook.com), a następnie spróbujesz uzyskać dostęp do aplikacji, które ma włączone uwierzytelnianie w usłudze Windows Azure może uzyskać odpowiedź 400 błąd, ponieważ domeny Account firmy Microsoft nie jest rozpoznawane przez usługi Windows Azure Active Directory. Aby zalogować się do aplikacji, wyloguj się z Account Microsoft najpierw. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Logując się do aplikacji przy użyciu Windows Azure uwierzytelnianie jest włączone i X509CertificateValidationMode innego niż None powoduje błędy sprawdzania poprawności certyfikatu dla certyfikatu accounts.accesscontrol.windows.net < o:p >< / o:p >

Sprawdzanie poprawności certyfikatu nie jest wymagane i powinny pozostać wyłączone. Odcisk palca certyfikatu wystawcy jest sprawdzana WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Podczas próby włączyć uwierzytelnianie w usłudze Windows Azure okna dialogowego uwierzytelniania sieci Web pokazuje błąd "ACS20016: domeny zalogowanego użytkownika (contoso.onmicrosoft.com) jest niezgodna z dowolnej dozwolonych domeny tej usługi STS." < o:p >< / o:p >

Może zostać wyświetlony ten błąd, gdy użytkownik wcześniej pomyślnie zalogował się przy użyciu innego konta usługi Windows Azure Active Directory z w ramach tego samego procesu programu Visual Studio. Wyloguj się z określonego konta, lub uruchom ponownie program Visual Studio. Jeśli wcześniej zarejestrowane w i wybrana została opcja "Wylogowuj mnie", wówczas może być konieczne wyczyszczenie plików cookie przeglądarki. < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Żądanie nie jest prawidłowy komunikat protokołu WS-Federation < o:p >< / o:p >

Może to nastąpić, jeśli już zalogowano się za pomocą niektórych identyfikator firmy Microsoft do jednej z usług platformy Azure. Użyj prywatnego okna przeglądarki InPrivate w programie Internet Explorer lub Incognito w przeglądarce Chrome, takich jak lub usuń zaznaczenie wszystkich plików cookie. < o:p >< / o:p >

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Microsoft ASP.NET narzędzia dla programu Windows Azure Active Directory — program Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) — Vittorio Bertocci
- [Windows funkcji platformy Azure: tożsamość](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows usługa Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: opracowywanie aplikacji dla swojej organizacji](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: opracowywanie aplikacji dla wielu organizacji](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Jak zaimplementować logowanie jednokrotne z usługą Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Logowanie jednokrotne przy użyciu Windows Azure Active Directory: Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) — Vittorio Bertocci
- [Usług AD FS 2.0 do wdrażania i zarządzania nimi logowanie jednokrotne](https://technet.microsoft.com/library/jj205462.aspx)
