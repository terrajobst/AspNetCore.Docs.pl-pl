---
title: "Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure przy użyciu programu Visual Studio"
author: rick-anderson
description: 
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publikowanie aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Silveira Blum Cesarowi](https://github.com/cesarbs), i [Rachel Appel](https://twitter.com/rachelappel)

Zobacz [publikowania na platformie Azure w programie Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) podczas pracy na komputerach Mac.

## <a name="set-up"></a>Konfigurowanie

* Otwórz [bezpłatne konto platformy Azure](https://aka.ms/K5y5yh) Jeśli nie istnieje. 

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W Visual Studio — strona początkowa, wybierz **Plik > Nowy > Projekt...**

![menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Zakończenie **nowy projekt** okna dialogowego:

* W okienku po lewej stronie wybierz **.NET Core**.

* W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core**.

* Wybierz **OK**.

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

W **nową aplikację sieci Web Core ASP.NET** okna dialogowego:

* Wybierz **aplikacji sieci Web**.

* Wybierz **Zmień uwierzytelnianie**.

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

**Zmień uwierzytelnianie** zostanie wyświetlone okno dialogowe. 

* Wybierz **indywidualne konta użytkowników**.

* Wybierz **OK** aby powrócić do **nową aplikację sieci Web Core ASP.NET**, a następnie wybierz pozycję **OK** ponownie.

![Okno dialogowe nowego uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Program Visual Studio tworzy rozwiązanie.

## <a name="run-the-app-locally"></a>Uruchamianie aplikacji lokalnie

* Wybierz **debugowania** następnie **uruchomić bez debugowania** Aby uruchomić aplikację lokalnie.

* Kliknij przycisk **o** i **skontaktuj się z** łącza do weryfikowanie działania aplikacji sieci web.

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

* Wybierz **zarejestrować** i zarejestrować nowego użytkownika. Można użyć adresu e-mail fikcyjne. Podczas przesyłania, zostanie wyświetlona strona następujący błąd:

    *"Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania. Wyjątku SQL: nie można otworzyć bazy danych. Stosowanie istniejących migracje dla kontekst bazy danych aplikacji może rozwiązać ten problem."*

* Wybierz **zastosować migracje** i po aktualizacji strony, Odśwież stronę.

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodła się podczas przetwarzania żądania. Wyjątku SQL: nie można otworzyć bazy danych. Stosowanie istniejących migracje dla kontekst bazy danych aplikacji może rozwiązać ten problem.](publish-to-azure-webapp-using-vs/_static/mig.png)

Aplikacja wyświetla wiadomości e-mail używany do rejestrowania nowego użytkownika i **Wyloguj się** łącza.

![Otwórz aplikację sieci Web w programie Microsoft Edge. Link rejestrowania zastępuje tekst Hello email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Zamknij stronę sieci web, wróć do programu Visual Studio i wybierz **Zatrzymaj debugowanie** z **debugowania** menu.

Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania...** .

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

W **publikowania** okno dialogowe, wybierz opcję **Microsoft Azure App Service** i kliknij przycisk **publikowania**.

![Okno dialogowe publikowania](publish-to-azure-webapp-using-vs/_static/maas1.png)

* Nadaj nazwę unikatową nazwę aplikacji. 

* Wybierz subskrypcję.

* Wybierz **nowych...**  zasobu grupy, a następnie wprowadź nazwę nowej grupy zasobów.

* Wybierz **nowych...**  dla planu usługi aplikacji i wybierz lokalizację w pobliżu. Można zachować na nazwę, która jest generowana przez domyślny.

![Usługi aplikacji — okno dialogowe](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Wybierz **usług** kartę, aby utworzyć nową bazę danych.

* Wybierz zielonego  **+**  ikonę, aby utworzyć nową bazę danych SQL

![Nowej bazy danych SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* Wybierz **nowych...**  na **Konfigurowanie bazy danych SQL** okna dialogowego, aby utworzyć nową bazę danych.

![Nowe bazy danych SQL i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

**Konfiguruj serwer SQL** zostanie wyświetlone okno dialogowe.

* Wprowadź nazwę użytkownika administratora i hasło, a następnie wybierz **OK**. Nie zapomnij nazwę użytkownika i hasło utworzone w tym kroku. Domyślne można zachować **nazwy serwera**. 

* Wprowadź nazwy parametrów połączenia i bazy danych.

> [!NOTE]
> "Administrator" nie jest dozwolona jako nazwa użytkownika administratora.

![Konfigurowanie okna dialogowego programu SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Wybierz **OK**.

Visual Studio zwraca do **Tworzenie usługi App Service** okna dialogowego.

* Wybierz **Utwórz** na **Tworzenie usługi App Service** okna dialogowego.

![Konfigurowanie bazy danych SQL w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Kliknij przycisk **ustawienia** łącze w **publikowania** okna dialogowego.

![Okno dialogowe publikowania: panel połączenia](publish-to-azure-webapp-using-vs/_static/pubc.png)

Na **ustawienia** strony **publikowania** okna dialogowego:

  * Rozwiń węzeł **baz danych** i sprawdź **Użyj tych parametrów połączenia w czasie wykonywania**.

  * Rozwiń węzeł **Entity Framework migracje** i sprawdź **Zastosuj publikowania tej migracji na**.

* Wybierz **zapisać**. Visual Studio zwraca do **publikowania** okna dialogowego. 

![Okno dialogowe publikowania: panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

Kliknij przycisk **publikowania**. Visual Studio będzie publikowanie aplikacji na platformie Azure i uruchomić aplikację w chmurze w przeglądarce.

### <a name="test-your-app-in-azure"></a>Testowanie aplikacji na platformie Azure

* Test **o** i **skontaktuj się z** łącza

* Zarejestruj nowy użytkownik

![Aplikacja sieci Web otworzyć w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Aktualizowanie aplikacji

* Edytuj *Pages/About.cshtml* Razor strony i zmienić jego zawartość. Na przykład można zmodyfikować akapitu znaczy "Hello platformy ASP.NET Core!":

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Kliknij prawym przyciskiem myszy na projekt i wybierz **publikowania...**  ponownie.

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

* Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Czyszczenie

Po zakończeniu testowania aplikacji, przejdź do [portalu Azure](https://portal.azure.com/) i usuwania aplikacji.

* Wybierz **grup zasobów**, następnie wybierz utworzoną grupę zasobów.

![Portalu Azure: Grupy zasobów w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* W **grup zasobów** wybierz pozycję **usunąć**.

![Portalu Azure: Strona grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Wprowadź nazwę grupy zasobów i wybierz **usunąć**. Aplikacji i innych zasobów utworzonej w tym samouczku są teraz usuwane z platformy Azure.

### <a name="next-steps"></a>Następne kroki

* [Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git](../publishing/azure-continuous-deployment.md)
