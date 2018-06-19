---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Uaktualnianie aplikacji ASP.NET MVC 1.0 do platformy ASP.NET MVC 2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano zarówno jak uaktualnić ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 ASP.NET MVC 2. Ten dokument jest również dostępny do d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573323"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Uaktualnianie aplikacji ASP.NET MVC 1.0 do platformy ASP.NET MVC 2
====================
> W tym dokumencie opisano zarówno jak uaktualnić ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 ASP.NET MVC 2. Ten dokument jest również dostępny do [Pobierz](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Wprowadzenie

ASP.NET MVC 2 można zainstalować równolegle program ASP.NET MVC w wersji 1.0 na tym samym serwerze. Zapewnia elastyczność deweloperzy aplikacji, wybierając podczas uaktualniania aplikacji ASP.NET MVC 2 platformy ASP.NET MVC w wersji 1.0.

Program Visual Studio 2010 zawiera Kreatora tego uaktualnienia istniejących projektów programu ASP.NET MVC w wersji 1.0 skompilowanej za pomocą programu Visual Studio 2008 ASP.NET MVC 2. Kreator uaktualniania jest inicjowane przez otwarcie projektu programu ASP.NET MVC w wersji 1.0 w programie Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Uaktualnij kreatora dla platformy ASP.NET MVC 1.0 w programie Visual Studio 2008 z dodatkiem SP1

Do uaktualniania aplikacji ASP.NET MVC w wersji 1.0 do platformy ASP.NET MVC 2 w Visual Studio 2008 z dodatkiem SP1, należy użyć aplikacji MvcAppConverter (nieobsługiwany). Można pobrać tę aplikację z następującego adresu URL:

[https://go.microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Ręczne uaktualnianie projektu programu ASP.NET MVC w wersji 1.0

Aby ręcznie uaktualnić istniejących aplikacji ASP.NET MVC w wersji 1.0 w wersji 2, wykonaj następujące kroki:

1. Wykonaj kopię zapasową istniejącego projektu.
2. W edytorze tekstów otwórz plik projektu (plik z rozszerzeniem pliku .csproj lub .vbproj) i Znajdź ProjectTypeGuid element. Jako wartość tego elementu Zastąp identyfikator GUID {603c0e0b-db56-11dc-be95-000d561079b0} z {F85E285D-A4E0-4152-9332-AB1D724D3325}. Gdy wszystko będzie gotowe, wartość tego elementu powinno się następująco: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. W folderze głównym aplikacji sieci Web należy zmodyfikować plik Web.config. Wyszukaj System.Web.Mvc, wersja = 1.0.0.0 i Zastąp wszystkie wystąpienia System.Web.Mvc, wersja = 2.0.0.0.
4. Powtórz poprzedni krok w pliku Web.config znajduje się w folderze widoków.
5. Otwórz projekt za pomocą programu Visual Studio i w **Eksploratora rozwiązań**, rozwiń węzeł **odwołania** węzła. Usuń odwołanie do System.Web.Mvc (wskazujący zestaw w wersji 1.0). Dodaj odwołanie do System.Web.Mvc (v2.0.0.0).
6. Dodaj następujący element w parametrze bindingRedirect do pliku Web.config w katalogu głównym aplikacji, w sekcji Wyświetl:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Utwórz nową aplikację ASP.NET MVC 2 puste. Skopiuj pliki z folderu skryptów nowej aplikacji do folderu skryptów istniejącej aplikacji.
8. Aktualizowanie istniejących € applicationâ™ s pliku CSS z definicji stylów CSS w pliku Site.css.
9. Kompilowanie aplikacji i uruchom go. Jeśli wystąpią błędy, zapoznaj się z sekcją fundamentalne zmiany [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) strony.
