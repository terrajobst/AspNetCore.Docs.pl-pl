---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio programu SQL Server Compact: wprowadzenie - 1 do 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9c0edb301de85d15b9a3527382b72211f6f3d3ec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio programu SQL Server Compact: wprowadzenie - 1 do 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010.
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Te samouczki pomocne przy najpierw wdrażania usług IIS na komputerze deweloperskim lokalnego do testowania, a następnie do dostawcy hostingu innych firm. W przypadku wdrażania aplikacji korzysta z aplikacji i bazy danych członkostwa programu ASP.NET. Możesz rozpocząć przy użyciu programu SQL Server Compact i wdrażających aplikacje w programie SQL Server Compact i kolejnych samouczkach dowiesz się, jak wdrożyć zmian w bazie danych i jak przeprowadzić migrację do programu SQL Server.
> 
> Samouczków przyjęto założenie, że wiesz, jak pracować z programem ASP.NET w programie Visual Studio. Jeśli użytkownik nie jest dobrym miejscem do rozpoczęcia [podstawowy samouczek formularzy sieci Web ASP.NET](../tailspin-spyworks/tailspin-spyworks-part-1.md) lub [basic ASP.NET MVC samouczek](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Omówienie

Te samouczki pomocne przy najpierw wdrażania usług IIS na komputerze deweloperskim lokalnego do testowania, a następnie do dostawcy hostingu innych firm. W przypadku wdrażania aplikacji korzysta z aplikacji i bazy danych członkostwa programu ASP.NET. Możesz rozpocząć przy użyciu programu SQL Server Compact i wdrażających aplikacje w programie SQL Server Compact i kolejnych samouczkach dowiesz się, jak wdrożyć zmian w bazie danych i jak przeprowadzić migrację do programu SQL Server.

Liczba samouczków — 11 we wszystkich oraz rozwiązywania problemów strony — może uniemożliwić procesu wdrażania wydaje się być czasochłonnym zadaniem. W rzeczywistości podstawowe procedury dotyczące wdrażania lokacji tworzą stosunkowo mały części samouczka zestawu. Jednak w sytuacjach rzeczywistych często konieczne informacje na temat pewien aspekt dodatkowe mały, ale ważne wdrożenia — na przykład ustawienie uprawnień do folderu na serwerze docelowym. Dodaliśmy wiele z tych innych technik w samouczków, z nadzieję, że nie należy pozostawiać samouczków informacje, które mogą uniemożliwiać pomyślne wdrożenie rzeczywistej aplikacji.

Samouczki są przeznaczone do uruchamiania w sekwencji, a każda część kompilacje w poprzedniej części. Jednak można pominąć części, które nie są odpowiednie do sytuacji. (Pomijanie części mogą wymagać dostosować procedury w kolejnych samouczkach.)

## <a name="intended-audience"></a>Docelowa grupa odbiorców

Celem jest samouczków deweloperów platformy ASP.NET, którzy pracują w innych środowiskach i małe firmy gdzie:

- Proces ciągłej integracji (kompilacjach zautomatyzowanych i wdrożenia) nie jest używany.
- W środowisku produkcyjnym jest dostawcą hostingu innych firm.
- Jedna osoba zwykle wypełnia wiele ról (ta sama osoba rozwija, testy i wdraża).

W środowiskach przedsiębiorstw jest bardziej typowego do implementacji procesów ciągłej integracji i środowiska produkcyjnego jest zwykle znajdujących się na serwerach przez firmę. Różne osoby zwykle także wykonywać różne role. Informacji o wdrażaniu przedsiębiorstwa, zobacz [wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizacjach różnej wielkości można także wdrożyć aplikacje sieci web na platformie Azure, a większość zgodnie z procedurami przedstawionymi w tych samouczkach stosuje się również do aplikacji sieci Web usługi aplikacji Azure. Aby obejrzeć wprowadzenie do platformy Azure, zobacz [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Dostawca hostingu pokazano samouczków

Samouczków prowadzi użytkownika przez proces konfigurowania konta z hostingu firmy i wdrażanie aplikacji do tego dostawcy hostingu. Określonych firma została wybrana, dzięki czemu samouczków można zilustrować pełne środowisko wdrożenia na żywo witryny sieci Web. Każda firma zapewnia różne funkcje, a środowisko wdrażania serwerów różni się nieco; Jednak proces opisany w tym samouczku jest typowa dla całego procesu.

Dostawca hostingu używane w tym samouczku Cytanium.com, jest jednym z wielu, które są dostępne, a jego użycie w tym samouczku nie stanowi potwierdzenie lub zalecenia.

## <a name="deploying-web-site-projects"></a>Wdrażanie projektu witryny sieci Web

Contoso University jest projektu aplikacji sieci web programu Visual Studio. Większość metody wdrażania i narzędzia przedstawiona w tym samouczku nie dotyczą [projektów witryny sieci Web](https://msdn.microsoft.com/library/dd547590.aspx). Aby uzyskać informacje o sposobie wdrażania projektów witryny sieci web, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projekty składnika ASP.NET MVC

W tym samouczku wdrażania projektu formularzy sieci Web ASP.NET, ale wszystko poznać sposoby wykonywania ma zastosowanie do platformy ASP.NET MVC również. Projekt programu Visual Studio MVC jest po prostu inną formę projektu aplikacji sieci web. Jedyną różnicą jest to, że jeśli wdrażasz do dostawcy usług hosta, który nie obsługuje platformy ASP.NET MVC ani Twojej wersji docelowej, należy się upewnić, że zainstalowano odpowiedni ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) lub [MVC 4](http://nuget.org/packages/aspnetmvc)) Pakietu NuGet w projekcie.

## <a name="programming-language"></a>Język programowania

Przykładowa aplikacja korzysta z C#, ale samouczków nie wymaga znajomości języka C# i techniki wdrażania wynika z samouczkami dotyczącymi nie są specyficzne dla języka.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów podczas tego samouczka

Po wystąpieniu błędu podczas wdrażania lub wdrożonej witrynie nie działa poprawnie, komunikaty o błędach nie zawsze stanowią rozwiązanie. Aby ułatwić niektórych typowych scenariuszy problem, [Rozwiązywanie problemów z strona referencyjna](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) jest dostępna. Jeśli coś nie działa podczas wykonywania kroków samouczków wyświetlony komunikat o błędzie, należy sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Komentarze — Zapraszamy!

Komentarze dotyczące samouczków są-Zapraszamy!, a po zaktualizowaniu samouczka wszelkich starań, zostaną wprowadzone należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia, które znajdują się w samouczku komentarze.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz system Windows 7 lub nowszy, a jeden z następujących produktów zainstalowanych na komputerze:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 z dodatkiem SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Jeśli masz program Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer Express 2010 z dodatkiem SP1 zainstaluj również następujące produkty:

- [Zestaw Azure SDK dla platformy .NET (VS 2010 z dodatkiem SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (w tym aktualizacji publikowania w sieci Web)
- [Microsoft Visual Studio 2010 SP1 Tools dla programu SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Niektóre inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie trzeba mieć jeszcze załadowany. Samouczek przeprowadzi Cię przez kroki instalacji, gdy są potrzebne.

## <a name="downloading-the-sample-application"></a>Pobieranie przykładowej aplikacji

W przypadku wdrażania aplikacji o nazwie Contoso University i został już utworzony dla Ciebie. Jest to wersja uproszczony university witryny sieci web, luźno na podstawie Contoso University aplikacji opisane w [Samouczki programu Entity Framework w witrynie platformy ASP.NET](https://asp.net/entity-framework/tutorials).

Jeśli masz zainstalowane warunki wstępne, Pobierz [aplikacji sieci web firmy Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *Zip* plik zawiera wiele wersji projektu i pliku PDF, który zawiera wszystkie samouczki 12. Aby pracować kroki tego samouczka, rozpoczynać ContosoUniversity Begin. Aby zobaczyć, jak wygląda projektu na końcu samouczków, otwórz ContosoUniversity-End. Aby zobaczyć, jak wygląda projektu przed migracją do pełnej wersji programu SQL Server w samouczku 10, otwórz ContosoUniversity AfterTutorial09.

Aby przygotować się do kroków samouczka, należy zapisać ContosoUniversity Begin do dowolnego folderu używać do pracy z projektów Visual Studio. Domyślnie jest to następujący folder:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Dla zrzutów ekranu, w tym samouczku, folder projektu znajduje się w katalogu głównym na `C`: dysk.)

Uruchom program Visual Studio, otwórz projekt, a następnie naciśnij klawisz CTRL-F5, aby uruchomić go.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Strony sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących funkcji:

- Wyświetlanie statystyk uczniów (strona informacje).
- Wyświetlanie, edytowanie usunąć i dodać studentów.
- Wyświetl i Edytuj szkolenia.
- Wyświetl i Edytuj instruktorów.
- Wyświetl i Edytuj działów.

Poniżej przedstawiono zrzuty ekranu kilka stron reprezentatywny.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Przegląd funkcji aplikacji, które mają wpływ na wdrożenie

Poniższe funkcje aplikacji wpływa na sposób wdrażania lub masz celu jej wdrożenia. Każdy z tych jest co omówiono bardziej szczegółowo w następujących samouczkach z tej serii.

- Contoso University korzysta z programu SQL Server Compact bazy danych do przechowywania danych aplikacji, takich jak nazwy dla użytkowników domowych i instruktora. Baza danych zawiera zarówno dane testowe i danymi produkcyjnymi, a podczas wdrażania w środowisku produkcyjnym należy wyłączyć danych testowych. W dalszej części samouczka serii będzie migracji z programu SQL Server Compact do programu SQL Server.
- Aplikacja używa systemu członkostwa programu ASP.NET, w której są przechowywane informacje o koncie użytkownika w bazie danych programu SQL Server Compact. Aplikacja definiuje użytkownika administracyjnego, który ma dostęp do niektórych chronionych informacji. Należy wdrożyć w bazie danych członkostwa bez testowe konta, ale jedno konto administratora.
- Ponieważ bazy danych aplikacji i bazy danych członkostwa użyć programu SQL Server Compact jako aparat bazy danych, należy wdrożyć aparatu bazy danych dostawcy usług hostingu, jak również samych bazach danych.
- Aplikacja używa dostawców uniwersalnych członkostwa ASP.NET, aby system członkostwa może przechowywać dane w bazie danych programu SQL Server Compact. Zestaw zawierający dostawców członkostwa uniwersalnych należy wdrożyć w aplikacji.
- Aplikacja używa Entity Framework w wersji 5.0 dostępu do danych w bazie danych aplikacji. Zestaw zawierający Entity Framework w wersji 5.0 należy wdrożyć w aplikacji.
- Błąd innej firmy, rejestrowania i raportowania narzędzie używane przez aplikację. To narzędzie jest dostępne w zestawie, do którego należy wdrożyć w aplikacji.
- Narzędzia rejestrowania błędów zapisuje informacje o błędzie w plikach XML do folderu plików. Należy upewnić się, czy konto, które program ASP.NET jest uruchamiana w witrynie wdrożonej ma uprawnienie zapisu do tego folderu, i czy masz do wykluczenia z wdrożenia tego folderu. (W przeciwnym razie błąd dane dzienników ze środowiska testowego mógł zostać wdrożony w środowisku produkcyjnym i/lub plików dziennika błędów produkcji może zostać usunięty.)
- Aplikacja zawiera pewne ustawienia, które można zmienić w programie w wdrożone *Web.config* pliku, w zależności od środowiska docelowego (testowym lub produkcyjnym) i inne ustawienia, które musi zostać zmienione w zależności od kompilacji Konfiguracja (debugowanie czy wydanie).
- Rozwiązanie programu Visual Studio zawiera projektu biblioteki klas. Powinny być wdrażane tylko zestaw, który generuje ten projekt, nie projektu.

W tym samouczku pierwszy z serii pobraniu przykładowy projekt programu Visual Studio i przejrzeć funkcji witryny, które mają wpływ na sposób wdrażania aplikacji. W następujących samouczkach przygotować się do wdrożenia, konfigurując niektóre z nich mają zostać obsłużone automatycznie. Inne rozwiązywane ręcznie.

>[!div class="step-by-step"]
[Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
