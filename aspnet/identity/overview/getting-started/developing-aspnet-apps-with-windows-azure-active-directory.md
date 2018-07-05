---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Tworzenie aplikacji ASP.NET w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Narzędzia Microsoft ASP.NET dla usługi Azure Active Directory pozwala w prosty sposób włączania uwierzytelniania dla aplikacji sieci web hostowanych na platformie Azure. Można użyć typ Uwier Azure...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 296c897887ea55a2e786d0229b0766e608e4edfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388283"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Tworzenie aplikacji ASP.NET w usłudze Azure Active Directory
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

Microsoft ASP.NET narzędzi dla usługi Azure Active Directory upraszcza włączania uwierzytelniania dla aplikacji sieci web hostowanych na [Azure](https://www.windowsazure.com/home/features/web-sites/). Uwierzytelnianie w usłudze Azure służy do uwierzytelniania użytkowników usługi Office 365 z Twojej organizacji, kont firmowych synchronizowanych z lokalnej usługi Active Directory lub użytkownicy utworzeni w własnej domenie niestandardowej usługi Azure Active Directory. Włączanie uwierzytelniania opartego na Windows Azure umożliwia skonfigurowanie aplikacji w celu uwierzytelniania użytkowników za pomocą pojedynczej [usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) dzierżawy.

Ten samouczek przedstawia sposób tworzenia aplikacji ASP.NET, który skonfigurowano na potrzeby logowania jednokrotnego przy użyciu [usługi Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Dowiedz się także, jak do wywołania interfejsu API programu Graph, aby uzyskać informacje o aktualnie zalogowanego użytkownika oraz jak wdrożyć aplikację na platformie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) lub [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 lub nowszy jest wymagany.
3. Konto platformy Azure. [Kliknij tutaj,](https://azure.microsoft.com/pricing/free-trial/) bezpłatnej wersji próbnej, jeśli nie masz jeszcze konta usługi.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Dodaj administratora globalnego do usługi Active Directory

1. Zaloguj się do [portalu zarządzania systemu Azure](https://manage.windowsazure.com/).
2. Wszystkie konta platformy Azure zawierają **katalog domyślny** — kliknij je, a następnie kliknij przycisk **użytkowników** kartę w górnej części strony (zobacz poniższy obraz).
3. Kliknij przycisk Dodaj użytkownika.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Tworzenie nowego użytkownika za pomocą **administratora globalnego** roli. Kliknij przycisk **użytkowników** z górnego menu, a następnie kliknij **Dodaj użytkownika** przycisk na pasku poleceń.
5. W **Dodaj użytkownika** okno dialogowe, wprowadź nazwę dla nowego użytkownika, a następnie kliknij strzałkę w prawo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Wprowadź nazwę użytkownika i ustaw rolę **administratora globalnego**. Administratorzy globalni wymagają alternatywny adres e-mail na potrzeby odzyskiwania hasła. Po zakończeniu kliknij strzałkę w prawo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na następnej stronie okna dialogowego kliknij **Utwórz**. Hasło tymczasowe zostanie utworzona dla nowego użytkownika i wyświetlane w oknie dialogowym.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   To hasło będzie wymagane, aby zmienić hasło po zakończeniu pierwszego logowania. Na poniższej ilustracji przedstawiono nowego konta administratora. Azure Active Directory należy użyć, aby zalogować się do swojej aplikacji, a nie na koncie Microsoft, które są także wyświetlane na tej stronie.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Tworzenie aplikacji ASP.NET

Następujące kroki użycia [Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)i wymaga [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. W programie Visual Studio, kliknij przycisk **pliku** i następnie **nowy projekt**. Na **nowy projekt** okno dialogowe, wybierz w Visual C# sieci Web projektu z menu po lewej stronie i kliknij przycisk **OK**. Możesz również chcieć Usuń zaznaczenie pola wyboru **Dodaj usługę Application Insights do projektu** Jeśli nie chcesz, aby dla aplikacji funkcji.
2. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**, a następnie kliknij przycisk **Zmień uwierzytelnianie**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Na **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **kont organizacyjnych**. Te opcje można automatycznie zarejestrować aplikację w usłudze Azure AD, a także automatycznie skonfigurować aplikację do integracji z usługą Azure AD. Nie trzeba stosować **Zmień uwierzytelnianie** okna dialogowego do rejestracji i konfiguracji aplikacji, ale sprawia, że łatwiej. Jeśli na przykład używasz programu Visual Studio 2012, można nadal ręcznie zarejestrować aplikację w portalu zarządzania systemu Azure i zaktualizuj jego konfigurację, aby zintegrować z usługą Azure AD.  
   W menu rozwijanych wybierz **chmura — pojedyncza organizacja** i **logowanie jednokrotne, Czytaj dane katalogu**. Wprowadź domenę do katalogu usługi Azure AD, na przykład (w poniższe obrazy) *aricka0yahoo.onmicrosoft.com*, a następnie kliknij przycisk **OK**. Możesz też uzyskać nazwy domeny na karcie domen katalog domyślny, w witrynie azure portal (patrz następny obraz w dół).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Na poniższej ilustracji przedstawiono nazwy domeny w witrynie Azure portal.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Opcjonalnie można skonfigurować identyfikator URI aplikacji, który ma zostać zarejestrowany w usłudze Azure AD, klikając **więcej opcji**. Identyfikator URI Identyfikatora aplikacji jest unikatowym identyfikatorem dla aplikacji, która jest zarejestrowana w usłudze Azure AD i używanych przez aplikację do zidentyfikowania się podczas komunikacji z usługą Azure AD. Aby uzyskać więcej informacji na temat identyfikator URI Identyfikatora aplikacji i innych właściwości zarejestrowanych aplikacji, zobacz [w tym temacie](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Klikając pole wyboru poniżej pola Identyfikator URI Identyfikatora aplikacji, możesz również zastąpić istniejącej rejestracji w usłudze Azure AD, która używa tego samego Identyfikatora URI aplikacji.
4. Po kliknięciu przycisku **OK**, zostanie wyświetlone okno dialogowe logowania i konieczne będzie zalogowanie się przy użyciu konta administratora globalnego (nie konta Microsoft skojarzonego z Twoją subskrypcją). Jeśli wcześniej utworzono nowe konto administratora, będzie konieczne zmiany hasła, a następnie zaloguj ponownie przy użyciu nowego hasła.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Po użytkownik został pomyślnie uwierzytelniony, **nowy projekt ASP.NET** okna dialogowego będą wyświetlane dowolnie uwierzytelniania (**organizacji** ) i katalog, w której nowa aplikacja będzie zarejestrowana (*aricka0yahoo.onmicrosoft.com* na poniższej ilustracji). Poniżej tych informacji, zaznacz pola wyboru **Hostuj w chmurze**. Jeśli to pole wyboru jest zaznaczone, projekt zostanie udostępniony jako aplikację internetową platformy Azure i będą mogły łatwo później publikowania. Kliknij przycisk **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure konfigurowania witryny sieci Web** zostanie wyświetlone okno dialogowe, przy użyciu nazwy lokacji wygenerowany automatycznie i regionu. Należy również zauważyć konta, które jest aktualnie zalogowany do w oknie dialogowym. Chcesz upewnić się, że to konto jest taki, który Twoja subskrypcja platformy Azure jest dołączona, zazwyczaj konta Microsoft.

    > [!NOTE]
    > Ten projekt wymaga bazy danych. Należy wybrać jedną z istniejących baz danych lub Utwórz nową. Bazy danych jest wymagana, ponieważ projekt już korzysta z lokalnego pliku bazy danych do przechowywania niewielką ilość danych konfiguracji uwierzytelniania. Podczas wdrażania aplikacji w witrynie sieci Web platformy Azure, ta baza danych nie jest spakowane przy użyciu wdrażania, dlatego należy wybrać jeden, który jest dostępny w chmurze. Kliknij przycisk **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Projekt zostanie utworzony i opcje aplikacji sieci web i opcje uwierzytelniania, na których zostaną automatycznie skonfigurowane z projektem. Po zakończeniu tego procesu należy uruchomić aplikację lokalnie, naciskając klawisz **^ F5**. Będą musieli logować się za pomocą konta swojej organizacji. Podaj nazwę użytkownika i hasło dla konta utworzonego wcześniej, a następnie kliknij przycisk **Zaloguj**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Po pomyślnym zalogowaniu zostaną wyświetlone witryny ASP.NET, że uwierzytelniono, wyświetlając nazwę użytkownika w prawym górnym rogu strony.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Jeśli zostanie wyświetlony błąd:  
   Wartość nie może być zerowa lub pusta. Nazwa parametru: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   zobacz [debugowania](#dbg) sekcji na końcu tego samouczka.

## <a name="basics-of-the-graph-api"></a>Podstawy interfejsu API programu Graph

[Interfejs API programu Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) jest interfejsu programowego, używanych do wykonywania operacji CRUD i inne operacje na obiektach w katalogu usługi Azure AD. Jeśli wybierzesz opcję konta organizacyjnego do uwierzytelniania podczas tworzenia nowego projektu w programie Visual Studio 2013, aplikację już być skonfigurowane do wywołania interfejsu API programu Graph. W tej sekcji krótko dowiesz się, jak działa interfejs API programu Graph.

1. W uruchomionej aplikacji, kliknij nazwę użytkownika zalogowanego w prawym górnym rogu strony. Spowoduje to przejście do strony profilu użytkownika, czyli akcję na kontrolerze Home. Można zauważyć, że tabela zawiera informacje użytkownika o konta administratora, które została utworzona wcześniej. Te informacje są przechowywane w katalogu i interfejsu API programu Graph jest wywoływana, aby pobrać te informacje podczas ładowania strony.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wróć do programu Visual Studio, a następnie rozwiń węzeł **kontrolerów** folder, a następnie otwórz **HomeController.cs** pliku. Zobaczysz **UserProfile()** akcję, która zawiera kod, aby pobrać tokenu, a następnie wywołać interfejs API programu Graph. Ten kod jest zduplikowana poniżej: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Aby wywołać interfejs API programu Graph, należy najpierw pobrać tokenu. Po pobraniu tokenu jego wartość ciągu musi być przypisany w nagłówku autoryzacji dla wszystkich kolejnych żądań interfejsu API programu Graph. Większość kodu powyżej obsłuży proces uwierzytelniania w usłudze Azure AD w celu pobrania tokenu, przy użyciu tokenu do wywoływania interfejsu API programu Graph i następnie Przekształcanie odpowiedzi, dzięki czemu mogą być prezentowane w widoku.

    Część najbardziej istotnych dla dyskusji jest następujący wyróżniony wiersz: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Ten wiersz oznacza nazwę użytkownika, który ma została przeprowadzona w odpowiedzi JSON i są prezentowane w widoku.

    Można wywołać interfejs API programu Graph, za pomocą elementu HttpClient i obsługi danych pierwotnych, samodzielnie, ale łatwiejszy sposób polega na użyciu [biblioteki klienckiej wykres, który jest dostępny za pośrednictwem pakietu NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Biblioteka klienta obsługuje pierwotne żądania HTTP i przekształcania danych zwracany dla Ciebie i sprawia, że znacznie łatwiej jest pracować z interfejsem API programu Graph w środowisku .NET. Zobacz powiązane przykłady kodu interfejsu API programu Graph w [usługi GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Poniższe kroki pokazują sposób wdrażania aplikacji na platformie Azure. We wcześniejszych krokach nawiązano nowy projekt z aplikacją sieci web na platformie Azure, aby był gotowy do opublikowania w kilku krokach.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Publikuj**. **Publikowanie w sieci Web** zostanie wyświetlone okno dialogowe z każdego ustawienia już skonfigurowane. Kliknij pozycję **dalej** przycisk, aby przejść do **ustawienia** strony. Może pojawić się prośba do uwierzytelniania; Upewnij się, że uwierzytelniania za pomocą konta subskrypcji platformy Azure (zazwyczaj konto Microsoft), a nie konta organizacyjnego, która została utworzona wcześniej.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Sprawdź **Włączanie uwierzytelniania organizacyjnego** opcji. W **domeny** wprowadź domenę dla katalogu. Z **poziom dostępu** listę rozwijaną, wybierz opcję **logowanie jednokrotne, Czytaj dane katalogu**. Można zauważyć, że poprzedniej została użyta bazy danych jest już wypełnione w **baz danych** sekcji. Kliknij przycisk **publikowania**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Program Visual Studio rozpocznie się wdrażanie witryny sieci Web, a następnie pojawi się nowe okno przeglądarki. Może być wyświetlony monit uwierzytelnienia do katalogu jeszcze raz. Po użytkownik został uwierzytelniony, nastąpi przekierowanie do witryny sieci Web nowo opublikowana na platformie Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli zostanie wyświetlony następujący błąd:   
 Wartość nie może być zerowa lub pusta. Nazwa parametru: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Zastąp kod w *Views\Shared\\_LoginPartial.cshtml* pliku następującym kodem:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Po uruchomieniu aplikacji, jeśli zalogowany użytkownik jest wyświetlana "O wartości Null User", wyloguj się i zaloguj się ponownie przy użyciu konta usługi Active Directory, która została utworzona wcześniej.

Doskonałe samouczka, aby wykonać to Rick Rainey [Deep Dive: Azure Websites i uwierzytelniania organizacyjnego przy użyciu usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Więcej informacji

- [Szczegółowe informacje: Usługa Azure Websites i uwierzytelniania organizacyjnego przy użyciu usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Omówienie interfejsu API programu Graph usługi Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenariusze uwierzytelniania w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Przykłady kodu platformy Azure AD, w witrynie GitHub](https://github.com/AzureADSamples)
