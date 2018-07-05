---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Uaktualnianie aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano sposób uaktualniania ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2. Ten dokument jest również dostępna dla d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381919"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Uaktualnianie aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2
====================
> W tym dokumencie opisano sposób uaktualniania ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2. Ten dokument jest również dostępna dla [pobierania](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Wprowadzenie

ASP.NET MVC 2 można zainstalować równolegle z programem ASP.NET MVC 1.0 na tym samym serwerze. Zapewnia to elastyczność deweloperom aplikacji w wyborze kiedy aktualizować aplikację ASP.NET MVC 1.0 do ASP.NET MVC 2.

Program Visual Studio 2010 zawiera Kreatora tego uaktualnienia istniejących projektów programu ASP.NET MVC 1.0 utworzonych za pomocą programu Visual Studio 2008 do wzorca ASP.NET MVC 2. Kreator uaktualniania jest inicjowane przez otwarcie projektu programu ASP.NET MVC 1.0 w programie Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Uaktualnij kreatora dla platformy ASP.NET MVC 1.0 w programie Visual Studio 2008 z dodatkiem SP1

Aby uaktualnić aplikację ASP.NET MVC 1.0 do ASP.NET MVC 2 w programie Visual Studio 2008 z dodatkiem SP1, należy użyć aplikacji (nieobsługiwane) MvcAppConverter. Możesz pobrać tę aplikację z następującego adresu URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Ręczne uaktualnianie projektu ASP.NET MVC 1.0

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 1.0 do wersji 2, wykonaj następujące kroki:

1. Tworzenie kopii zapasowych istniejący projekt.
2. W edytorze tekstu Otwórz plik projektu (plik z rozszerzeniem pliku .csproj lub .vbproj) i Znajdź ProjectTypeGuid element. Jako wartość tego elementu należy zastąpić identyfikatorem GUID {603c0e0b-db56-11dc-be95-000d561079b0} z {F85E285D-A4E0-4152-9332-AB1D724D3325}. Gdy wszystko będzie gotowe, wartość tego elementu powinna być następująca: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. W folderze głównym aplikacji sieci Web należy edytować plik Web.config. Wyszukaj System.Web.Mvc, wersja = 1.0.0.0 i Zamień wszystkie wystąpienia System.Web.Mvc, wersja = 2.0.0.0 lub nowszej.
4. Powtórz poprzedni krok dla pliku Web.config znajduje się w folderze widoków.
5. Otwórz projekt za pomocą programu Visual Studio, a następnie w **Eksploratora rozwiązań**, rozwiń węzeł **odwołania** węzła. Usuń odwołanie do System.Web.Mvc (wskazujący zestaw w wersji 1.0). Dodaj odwołanie do System.Web.Mvc (v2.0.0.0).
6. Dodaj następujący element bindingRedirect do pliku Web.config w katalogu głównym aplikacji, w sekcji configuraton:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Utwórz nową aplikację ASP.NET MVC 2 puste. Skopiuj pliki z folderu skryptów w nowej aplikacji do folderu skryptów w istniejącej aplikacji.
8. Aktualizowanie istniejących € applicationâ™ s pliku CSS przy użyciu definicji stylów CSS w pliku Site.css.
9. Kompilowanie aplikacji i uruchom go. Jeśli wystąpią błędy, zapoznaj się z sekcją fundamentalne zmiany [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) strony.
