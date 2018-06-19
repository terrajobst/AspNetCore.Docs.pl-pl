---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure Authentication | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Narzędzia Microsoft ASP.NET dla usługi Windows Azure Active Directory upraszcza do włączenia uwierzytelniania dla aplikacji sieci web hostowanych w systemie Windows Azure Web Sites...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 09cb37ceb0132958a48f5f3a5d52dc46c6f0a78d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874827"
---
<a name="windows-azure-authentication"></a>Windows Azure Authentication
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET narzędzi dla systemu Windows Azure Active Directory upraszcza włączyć uwierzytelnianie dla aplikacji sieci web znajdującej się na [Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/). Uwierzytelnianie systemu Windows Azure można użyć do uwierzytelniania użytkowników usługi Office 365 z organizacji, użytkownicy utworzeni w własne niestandardowe domeny systemu Windows Azure Active Directory lub kont firmowych synchronizowane z lokalnej usługi Active Directory. Włączenie uwierzytelniania systemu Windows Azure konfiguruje aplikację do uwierzytelniania użytkowników za pomocą pojedynczej [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) dzierżawy.
> 
> Narzędzie uwierzytelniania platformy ASP.NET systemu Windows Azure nie jest obsługiwane dla ról sieci web w usłudze w chmurze, ale planujemy to zrobić w przyszłej wersji. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) jest obsługiwana w role sieci web systemu Windows Azure.
> 
> Aby uzyskać szczegółowe informacje dotyczące konfigurowania synchronizacji między lokalnymi usługi Active Directory i dzierżawy usługi Windows Azure Active Directory zobacz [pomocą usług AD FS 2.0 do wdrażania i zarządzania logowanie jednokrotne](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory jest obecnie dostępna jako [wolny w wersji zapoznawczej usługi](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Wymagania:

- Program Visual Studio 2012 lub [programu Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Rozszerzeń dla programu Visual Studio 2012 narzędzi sieci Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) lub [rozszerzeń narzędzi sieci Web dla programu Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Narzędzia platformy Microsoft ASP.NET dla systemu Windows Azure z usługą Active Directory programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) lub [narzędzi platformy Microsoft ASP.NET dla systemu Windows Azure Active Directory — Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Tworzenie aplikacji sieci Web platformy ASP.NET z programu Visual Studio 2012

Można utworzyć żadnej aplikacji sieci Web programu Visual Studio 2012, w tym samouczku używana szablonu intranetowego ASP.NET MVC.

1. Utwórz nową aplikację sieci Intranet 4 ASP.NET MVC i Zaakceptuj wszystkie ustawienia domyślne. (Musi być w **licz** sieci, a nie w **rowadź** net projektu).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Włączanie uwierzytelniania Azure (Jeśli jesteś administratorem globalnym cechą)

Jeśli nie masz istniejącej dzierżawy usługi Windows Azure Active Directory (na przykład przy użyciu istniejącego konta usługi Office 365) można utworzyć nową dzierżawę, logując się [nowego konta usługi Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. Wybierz z menu Projekt **włączenia usługi Windows Azure Authentication**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Wprowadź domenę dla dzierżawy usługi Windows Azure Active Directory (np. contoso.onmicrosoft.com), a następnie kliknij przycisk **włączyć**:

![](windows-azure-authentication/_static/image3.png)

3. W sieci Web uwierzytelniania okna dialogowego Zaloguj się jako administrator dzierżawy usługi Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Włącz Windows Azure z systemem innym niż administrator programu cechą

Jeśli nie masz uprawnień administratora globalnego dla dzierżawy usługi Windows Azure Active Directory, można usunąć zaznaczenie pola wyboru do obsługi aplikacji.

![](windows-azure-authentication/_static/image6.png)

Wyświetli się okno dialogowe **domeny**, **identyfikator podmiotu zabezpieczeń aplikacji** i **adres URL odpowiedzi** które są wymagane do obsługi aplikacji w usłudze Azure Active Directory cechą. Należy podać te informacje osobom, które ma wystarczające uprawnienia do udostępnienia aplikacji. Zobacz[implementowania rejestracji jednokrotnej z systemu Windows Azure Active Directory — aplikacji ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) szczegółowe informacje dotyczące sposobu używania polecenia cmdlet ręcznie utworzyć nazwy głównej usługi.  
Po pomyślnie zainicjowano aplikacji, możesz kliknąć **zaktualizować plik web.config z wybranymi ustawieniami w dalszym ciągu**. Jeśli chcesz kontynuować tworzenie aplikacji podczas oczekiwania na potrzeby inicjowania obsługi administracyjnej wystąpi, możesz kliknąć **Zamknij, aby Zapamiętaj ustawienia w pliku projektu**. Podczas następnego wywołania Włącz z uwierzytelniania systemu Windows Azure i usunąć zaznaczenie pola wyboru inicjowania obsługi administracyjnej, zobaczysz tych samych ustawień i możesz kliknąć **Kontynuuj**, następnie kliknij przycisk **zastosować te ustawienia w pliku web.config**.

1. Zaczekaj, aż aplikacja jest skonfigurowana do uwierzytelniania systemu Windows Azure i obsługiwane za pomocą usługi Windows Azure Active Directory.
2. Po włączeniu uwierzytelniania systemu Windows Azure dla aplikacji, kliknij przycisk **Zamknij:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Naciśnij klawisz F5, aby uruchomić aplikację. Należy następuje automatyczne przekierowanie do strony logowania. Korzystanie z poświadczeń użytkownika cechą katalogu, aby zalogować się do aplikacji.  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Ponieważ aplikacja jest obecnie używany certyfikat testowy podpisem zostanie wyświetlone ostrzeżenie z przeglądarki, że certyfikat nie został wystawiony przez zaufany urząd certyfikacji.

    To ostrzeżenie można bezpiecznie zignorować podczas tworzenia lokalnego, klikając **Kontynuuj przeglądanie tej witryny sieci Web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Możesz teraz pomyślnie zalogowali się do aplikacji przy użyciu uwierzytelniania systemu Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Włączenie systemu Windows Azure authentication wprowadza następujące zmiany do aplikacji:

- Żądania Międzywitrynowego przeciwko między lokacji ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) klasy ( *aplikacji\_Start\AntiXsrfConfig.cs* ) zostanie dodany do projektu.
- Pakiety NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` zostanie dodany do projektu.
- Ustawienia systemu Windows Identity Foundation w aplikacji zostaną skonfigurowane do akceptowania tokeny zabezpieczające z dzierżawy usługi Windows Azure Active Directory. Kliknij przycisk poniżej, aby wyświetlić widoku rozwiniętego zmiany wprowadzone do obrazu *Web.config* pliku.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Nazwy głównej usługi dla aplikacji w dzierżawie usługi Windows Azure Active Directory zostanie zainicjowana.
- HTTPS jest włączone.

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji w systemie Windows Azure

Aby uzyskać pełne instrukcje, zobacz [wdrażanie aplikacji sieci Web ASP.NET do witryny sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Aby opublikować aplikację przy użyciu uwierzytelniania systemu Windows Azure do witryny sieci Web platformy Azure:

1. Kliknij prawym przyciskiem myszy aplikację, a następnie wybierz **publikowania:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. W oknie dialogowym Publikowanie w sieci Web pobieranie i importowanie profilu publikowania dla witryny sieci Web platformy Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Połączenia** karcie pokazuje **docelowy adres URL** (publicznego URL połączonej aplikacji). Kliknij przycisk **sprawdzania poprawności połączenia** do testowania połączenia:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Po opublikowaniu do tej witryny sieci Web Azure wcześniej należy wziąć pod uwagę sprawdzanie **Usuń dodatkowe pliki w miejscu docelowym** ustawienie w celu zapewnienia aplikacji publikuje prawidłowo. Powiadomienie **włączenia usługi Windows Azure Authentication** pole wyboru jest wybranych.  

    ![](windows-azure-authentication/_static/image10.png)
5. Opcjonalnie: Na **Podgląd** kliknij kartę **Uruchom Podgląd** Aby wyświetlić pliki, wdrożone.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Kliknij przycisk **publikowania.**

    Pojawi się monit, aby włączyć uwierzytelnianie systemu Windows Azure dla hosta docelowego. Kliknij przycisk **włączyć** aby kontynuować:

    ![](windows-azure-authentication/_static/image11.png)
7. Wprowadź swoje poświadczenia administratora dla dzierżawy usługi Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Gdy aplikacja została pomyślnie opublikowana, opublikowanej witryny sieci web zostanie otwarta w przeglądarce.

    > [!NOTE]
    > Może upłynąć do pięciu minut (zazwyczaj musi mniej) dla aplikacji w pełni obsługiwane za pomocą usługi Windows Azure Active Directory po włączeniu uwierzytelnianie systemu Windows Azure dla hosta docelowego. Przy pierwszym uruchomieniu aplikacji Jeśli wystąpi błąd ACS50001: nie znaleziono jednostki uzależnionej o nazwie [obszaru], zaczekaj kilka minut i spróbuj uruchomić ponownie aplikację.
9. Po wyświetleniu monitu zaloguj się jako użytkownik w katalogu:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Teraz pomyślnie zarejestrowany do platformy Azure hostowanej aplikacji przy użyciu uwierzytelniania systemu Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Znane problemy

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autoryzacji opartej na rolach nie powiedzie się, używając uwierzytelniania systemu Windows Azure < o: p >< / o: p >

Uwierzytelnianie systemu Windows Azure nie jest aktualnie dostępny oświadczenia wymagane rolę, dzięki czemu można przeprowadzić autoryzacji opartej na rolach. Rola uwierzytelnionego użytkownika musi zostać ręcznie pobrany z usługi Windows Azure Active Directory < o: p >< / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Przeglądanie do aplikacji przy użyciu uwierzytelniania systemu Windows Azure powoduje błąd "ACS20016 domena zalogowanego użytkownika (live.com) nie odpowiada żadnym dozwolone domeny tego STS" < o: p >< / o: p >

Jeśli użytkownik jest już zalogowany z Account Microsoft (np. hotmail.com, live.com, outlook.com), a próba uzyskania dostępu do aplikacji, które ma włączone uwierzytelnianie systemu Windows Azure może uzyskać odpowiedzi błąd 400, ponieważ domeny Account Microsoft nie jest rozpoznawany przez Windows Azure Active Directory. Aby zalogować się do aplikacji, wyloguj się z Account Microsoft najpierw. < o: p >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Logowanie do aplikacji przy użyciu uwierzytelniania systemu Windows Azure, włączona i X509CertificateValidationMode innych niż Brak powoduje błędy sprawdzania poprawności certyfikatów dla certyfikatu accounts.accesscontrol.windows.net < o: p >< / o: p >

Sprawdzanie poprawności certyfikatu nie jest wymagany i należy pozostawić wyłączone. Odcisk palca wystawcy certyfikatu jest zweryfikowany przez WSFederationAuthenticationModule. < o: p >< / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Podczas próby włączenia uwierzytelniania systemu Windows Azure dialog uwierzytelniania sieci Web zawiera kod błędu "ACS20016: domeny zalogowanego użytkownika (contoso.onmicrosoft.com) jest niezgodna z dowolnej dozwolonych domeny tej usługi STS." < o: p >< / o: p >

Może występować ten błąd, gdy użytkownik wcześniej pomyślnie zalogował się przy użyciu innego konta usługi Windows Azure Active Directory z w ramach tego samego procesu programu Visual Studio. Wyloguj się z określonego konta lub ponownie program Visual Studio. Jeśli wcześniej zalogowany i wybraniu opcji "Zachowaj wylogowuj mnie", należy wyczyścić pliki cookie przeglądarki. < o: p >< / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Żądanie nie jest prawidłową komunikatu protokołu WS-Federation < o: p >< / o: p >

Może to nastąpić, jeśli użytkownik jest już zalogowany niektórych identyfikator firmy Microsoft do jednej z usług Azure. Użyj prywatnego okna przeglądarki, takich jak funkcja InPrivate w programie Internet Explorer lub Incognito w przeglądarce Chrome lub wyczyść wszystkie pliki cookie. <o:p></o:p>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Narzędzia platformy Microsoft ASP.NET dla systemu Windows Azure z usługą Active Directory programu Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) — Vittorio Bertocci
- [Windows Azure funkcji: tożsamości](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: opracowywanie aplikacji dla Twojej organizacji](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: opracowywanie aplikacji dla wielu organizacji](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Implementowania rejestracji jednokrotnej z systemu Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Logowanie jednokrotne z systemem Windows Azure Active Directory: nowości](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) — Vittorio Bertocci
- [Użyj usługi AD FS 2.0 do wdrażania i zarządzania logowanie jednokrotne](https://technet.microsoft.com/library/jj205462.aspx)
