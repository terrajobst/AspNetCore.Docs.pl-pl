---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Tworzenie aplikacji ASP.NET w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Narzędzia Microsoft ASP.NET dla usługi Azure Active Directory upraszcza do włączenia uwierzytelniania dla aplikacji sieci web hostowanej na platformie Azure. Można użyć Azure Authenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 9b2dc05089126fd5f4c1b0a0bd85b8a39f3041dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838501"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Tworzenie aplikacji ASP.NET w usłudze Azure Active Directory
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

Narzędzia Microsoft ASP.NET dla usługi Azure Active Directory upraszcza włączania uwierzytelniania dla aplikacji sieci web hostowanych na [Azure](https://www.windowsazure.com/home/features/web-sites/). Azure Authentication służy do uwierzytelniania użytkowników usługi Office 365 z organizacji, użytkownicy utworzeni w własne niestandardowe domeny usługi Azure Active Directory lub kont firmowych synchronizowane z lokalnej usługi Active Directory. Włączenie uwierzytelniania systemu Windows Azure konfiguruje aplikację do uwierzytelniania użytkowników za pomocą pojedynczej [usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) dzierżawy.

Ten samouczek przedstawia sposób tworzenia aplikacji ASP.NET, która jest skonfigurowana dla logowania z [usługi Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Także przedstawiono sposób wywołania interfejsu API programu Graph, aby uzyskać informacje o aktualnie zalogowanego użytkownika oraz sposobu wdrażania aplikacji na platformie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Program Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) lub [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) — wymagana jest aktualizacja 3 lub nowszej.
3. Konto platformy Azure. [Kliknij tutaj](https://azure.microsoft.com/pricing/free-trial/) bezpłatnej wersji próbnej, jeśli nie masz już konto.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Dodaj rolę administratora globalnego do usługi Active Directory

1. Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com/).
2. Wszystkie konta platformy Azure zawierają **katalog domyślny** — kliknij go, a następnie kliknij przycisk **użytkowników** kartę w górnej części strony (zobacz obraz poniżej).
3. Kliknij przycisk Dodaj użytkownika.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Utwórz nowego użytkownika z **administratora globalnego** roli. Kliknij przycisk **użytkowników** z menu u góry, a następnie kliknij przycisk **Dodaj użytkownika** przycisk paska poleceń.
5. W **Dodaj użytkownika** okna dialogowego, wprowadź nazwę dla nowego użytkownika, a następnie kliknij strzałkę w prawo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Wprowadź nazwę użytkownika i rolę ustaw **administratora globalnego**. Administratorzy globalni wymagają alternatywnego adresu e-mail na potrzeby odzyskiwania hasła. Po zakończeniu kliknij strzałkę w prawo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na następnej stronie okna dialogowego, kliknij przycisk **Utwórz**. Hasło tymczasowe, zostanie utworzona dla nowego użytkownika i wyświetlane w oknie dialogowym.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Zapisz hasło konieczna będzie zmiana hasła po pierwszego logowania. Na poniższej ilustracji przedstawiono nowe konto administratora. Aby zalogować się do aplikacji, nie na koncie Microsoft, które są także wyświetlane na tej stronie, należy użyć usługi Azure Active Directory.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Tworzenie aplikacji ASP.NET

Następujące kroki użyj [programu Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)i wymaga [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. W programie Visual Studio, kliknij przycisk **pliku** , a następnie **nowy projekt**. Na **nowy projekt** okno dialogowe, wybierz Visual C# sieci Web projektu z menu po lewej stronie i kliknij przycisk **OK**. Można również usunąć zaznaczenie pola wyboru **Dodaj usługę Application Insights do projektu** Jeśli nie chcesz funkcjonalności aplikacji.
2. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**, a następnie kliknij przycisk **Zmień uwierzytelnianie**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Na **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **konta organizacyjne**. Te opcje można automatycznie zarejestrować aplikację w usłudze Azure AD, a także automatycznie skonfigurować aplikację do integracji z usługą Azure AD. Nie trzeba używać **Zmień uwierzytelnianie** okna dialogowego do rejestracji i konfiguracji aplikacji, ale ułatwia. Jeśli na przykład używasz programu Visual Studio 2012, można nadal ręcznie zarejestrować aplikację w portalu zarządzania Azure i zaktualizuj jego konfigurację do integracji z usługą Azure AD.  
   W menu rozwijanego wybierz **Cloud - jednej organizacji** i **jednokrotne, Czytaj dane katalogu**. Wprowadź domenę katalogu usługi Azure AD, na przykład (w obrazach poniżej) *aricka0yahoo.onmicrosoft.com*, a następnie kliknij przycisk **OK**. Możesz też uzyskać nazwę domeny na karcie domen katalog domyślny, w portalu azure (zobacz następny obraz w dół).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Na poniższej ilustracji przedstawiono nazwę domeny w portalu Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Opcjonalnie można skonfigurować identyfikator URI aplikacji, która zostanie zarejestrowana w usłudze Azure AD, klikając **więcej opcji**. Identyfikator URI aplikacji jest unikatowym identyfikatorem dla aplikacji, która jest zarejestrowana w usłudze Azure AD i używane przez aplikację do identyfikacji podczas komunikacji z usługą Azure AD. Aby uzyskać więcej informacji na temat identyfikator URI aplikacji i innych właściwości zarejestrowanych aplikacji, zobacz [w tym temacie](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Kliknij pole wyboru poniżej pola Identyfikator URI aplikacji, można również zastąpić istniejący rejestracji w usłudze Azure AD, który korzysta z takim samym Identyfikatorem URI aplikacji.
4. Po kliknięciu przycisku **OK**, wyświetli się okno dialogowe logowania i konieczne będzie zalogowanie się przy użyciu konta administratora globalnego (nie konta Microsoft skojarzonego z subskrypcją). Jeśli wcześniej utworzono nowe konto administratora, będzie trzeba zmienić hasło, a następnie zaloguj się ponownie przy użyciu nowego hasła.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Po pomyślnie uwierzytelniono, **nowy projekt ASP.NET** okna dialogowego zostaną wyświetlone wybór uwierzytelniania (**organizacyjnej** ) i katalog, w której będzie nowa aplikacja zarejestrowany (*aricka0yahoo.onmicrosoft.com* na poniższej ilustracji). Poniżej tych informacji, zaznacz pole wyboru **Hostuj w chmurze**. Jeśli to pole wyboru jest zaznaczone, projekt zostanie zainicjowana jako aplikacja sieci web platformy Azure i będzie można później łatwo publikowania. Kliknij przycisk **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure konfigurowania witryny sieci Web** wyświetli się okno dialogowe, przy użyciu nazwy lokacji wygenerowany automatycznie i regionu. Należy również zauważyć konta, które jest aktualnie zalogowany do w oknie dialogowym. Aby upewnić się, że to konto jest dołączona subskrypcji platformy Azure, zwykle konta Microsoft.

    > [!NOTE]
    > Ten projekt wymaga bazy danych. Należy wybrać jedną z istniejących baz danych lub Utwórz nową. Bazy danych jest wymagana, ponieważ projekt już korzysta z plików lokalnej bazy danych do przechowywania niewielką ilość danych konfiguracji uwierzytelniania. Podczas wdrażania aplikacji do witryny sieci Web platformy Azure tej bazy danych nie jest dostarczana z wdrożeniem, należy wybrać jedną, która jest dostępna w chmurze. Kliknij przycisk **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Projekt zostanie utworzony i opcje aplikacji sieci web i opcje uwierzytelniania, na których zostanie automatycznie skonfigurowany z projektem. Po zakończeniu tego procesu uruchamianie projektu lokalnie, naciskając **^ F5**. Będą musieli logować się za pomocą konta organizacyjnego. Podaj nazwę użytkownika i hasło dla konta utworzonego wcześniej, a następnie kliknij przycisk **Zaloguj**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Po pomyślnego logowania witryny ASP.NET wykaże, że uwierzytelniono za pomocą wyświetlania nazwy użytkownika w prawym górnym rogu strony.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Jeśli zostanie wyświetlony błąd:  
   Wartość nie może być zerowa ani pusta. Nazwa parametru: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   zobacz [debugowania](#dbg) sekcji na końcu samouczka.

## <a name="basics-of-the-graph-api"></a>Podstawy interfejs API Graph

[Interfejsu API programu Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) jest interfejs programistyczny używanych do wykonywania CRUD i inne operacje na obiektach w katalogu usługi Azure AD. Jeśli wybrano opcję konta organizacyjnego do uwierzytelniania podczas tworzenia nowego projektu programu Visual Studio 2013, aplikacja już zostanie skonfigurowana do wywołania interfejsu API programu Graph. W tej sekcji krótko opisano sposób działania interfejsu API programu Graph.

1. W uruchomionej aplikacji, kliknij nazwę użytkownika zalogowanego na początku prawej części strony. Spowoduje to przejście do strony profilu użytkownika, czyli akcji w kontrolerze Home. Można zauważyć, że tabela zawiera informacje użytkownika o konta administratora, które wcześniej utworzony. Te informacje są przechowywane w katalogu i interfejsu API programu Graph jest wywoływana, aby pobrać te informacje podczas ładowania strony.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wróć do programu Visual Studio i rozwiń **kontrolerów** , a następnie otwórz folder **HomeController.cs** pliku. Zobaczysz **UserProfile()** akcji, który zawiera kod, aby pobrać token, a następnie wywołać interfejsu API programu Graph. Ten kod jest zduplikowany poniżej: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Aby wywołać interfejsu API programu Graph, należy najpierw pobrać tokenu. Po pobraniu tokenu jego wartość ciągu musi być przypisany w nagłówku autoryzacji dla wszystkich kolejnych żądań interfejsu API programu Graph. Większość kodu powyżej obsługuje dane do usługi Azure AD w celu pobrania tokenu uwierzytelniania, przy użyciu tokenu do wywoływania interfejsu API programu Graph i następnie Przekształcanie odpowiedzi, dzięki czemu mogą być przedstawiane w widoku.

    Najbardziej odpowiedniej części omówienie jest następujący wyróżniony wiersz: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Ten wiersz reprezentuje nazwę użytkownika, który został zdeserializowany z odpowiedzi JSON i są prezentowane w widoku.

    Można wywołać interfejsu API programu Graph, za pomocą elementu HttpClient i obsługiwać nieprzetworzone dane, samodzielnie, ale łatwiej metodą jest użycie [biblioteki klienta wykresu, który jest dostępny za pośrednictwem pakietu NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Biblioteka klienta obsługuje pierwotne żądania HTTP i transformacja dane zwrotne dla Ciebie i ułatwia do pracy z interfejsu API programu Graph w środowisku .NET. Zobacz powiązane przykłady kodu interfejsu API programu Graph w [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Poniższe kroki pokazują jak wdrożyć aplikację na platformie Azure. We wcześniejszych krokach zostanie połączony nowego projektu z aplikacją sieci web na platformie Azure, więc jest gotowy do opublikowania w kilku kroków.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**. **Publikowanie w sieci Web** wyświetli się okno dialogowe z każdego ustawienia już skonfigurowane. Polecenie **dalej** przycisk, aby przejść do **ustawienia** strony. Może pojawić się prośba do uwierzytelniania; Upewnij się, że należy przeprowadzić uwierzytelnianie przy użyciu konta subskrypcji platformy Azure (zazwyczaj kontem Microsoft), a nie konta organizacyjnego utworzony wcześniej.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Sprawdź **Włączanie uwierzytelniania organizacyjnego** opcji. W **domeny** wprowadź domeny dla katalogu. Z **poziom dostępu** listy rozwijanej, wybierz pozycję **jednokrotne, Czytaj dane katalogu**. Można zauważyć, że poprzednie bazy danych, możesz użyć znajduje się już w **baz danych** sekcji. Kliknij przycisk **publikowania**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio będzie rozpocząć wdrażanie witryny sieci Web, a następnie pojawi się nowe okno przeglądarki. Może pojawić się prośba do uwierzytelniania katalogu jeszcze raz. Po uwierzytelniono, użytkownik będzie przekierowany do nowo opublikowana witryny sieci Web na platformie Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli zostanie wyświetlony następujący błąd:   
 Wartość nie może być zerowa ani pusta. Nazwa parametru: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Zastąp kod w *Views\Shared\\_LoginPartial.cshtml* pliku następującym kodem:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Po uruchomieniu aplikacji, jeśli zalogowanego użytkownika zawiera "Użytkownika o wartości Null", wyloguj się i zaloguj się ponownie przy użyciu utworzonego wcześniej konta usługi Active Directory.

Znakomity samouczkiem, aby wykonać to Rick Rainey [nowości: witryny sieci Web platformy Azure i organizacji uwierzytelnianie przy użyciu usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Więcej informacji

- [Szczegółowe informacje na temat: Witryn sieci Web Azure i organizacyjne uwierzytelniania za pomocą usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Przegląd interfejsu API Graph usługi Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenariusze uwierzytelniania w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Przykłady kodu usługi Azure AD w witrynie GitHub](https://github.com/AzureADSamples)
