---
uid: single-page-application/overview/introduction/other-libraries
title: Sprawdzić biblioteki innego niż odcinania? | Microsoft Docs
author: madskristensen
description: Sprawdzić biblioteki innego niż odcinania?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="know-a-library-other-than-knockout"></a>Sprawdzić biblioteki innego niż odcinania?
====================
przez [Mads Kristensen](https://github.com/madskristensen)

[Szablonu jednej strony aplikacji JEDNOSTRONICOWEJ](knockoutjs-template.md) to dobry sposób, aby rozpocząć pisanie aplikacji jednej strony. Szablon używa [elementami KnockoutJS](http://knockoutjs.com/) powiązać elementy modelu DOM danych aplikacji.

Ale odcinania nie jest tylko biblioteka języka JavaScript do tworzenia aplikacji wzbogaconego klienta. Inne biblioteki rozwiązać problemy podobne na różne sposoby. Można wybrać jedną bibliotekę zamiast innego, więc wprowadziliśmy kilka szablonów utworzonych przez społeczność dostępny do pobrania. Każda z tych szablonów używa różnych kombinacji bibliotek JavaScript klienta.

Aby zainstalować szablon utworzony społeczności, można znaleźć szablonu strony wymienione poniżej i kliknij przycisk Pobierz. Szablony służą jako plików VSIX.

## <a name="backbonejs"></a>BackboneJS

[Szablon SPA backbone.js](../templates/backbonejs-template.md). Ten szablon zawiera początkowe szkielet związane z opracowywaniem [Backbone.js](http://backbonejs.org/) aplikacji na platformie ASP.NET MVC. Poza pole zawiera funkcję logowania użytkownika podstawowego, tym resetowania hasła rejestrację, logowanie, użytkownika i potwierdzenie przez użytkownika z szablonami podstawowe wiadomości e-mail.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) jest biblioteki typu open source do zarządzania zaawansowanych danych w kliencie JavaScript. Błyskawicznie obsługi zapytań, buforowania, śledzenia zmian, weryfikacji i inne. Dwa szablony funkcji błyskawicznie:

- [Błyskawicznie/odcinania](../templates/breezeknockout-template.md) szablonu rozszerza szablonu SPA odcinania, pokazujący, jak łatwo można tworzenia aplikacji jednej strony z błyskawicznie zarządzanie danymi i elementami KnockoutJS dla powiązania danych.
- [Błyskawicznie/kątową](../templates/breezeangular-template.md) rozszerzają szablonu SPA odcinania błyskawicznie, ale przy użyciu szablonu [AngularJS](http://angularjs.org) biblioteki dla powiązania danych, iniekcji zależności i zarządzanie ekranu.

Ponadto [szablonu gorących SPA ręczników](../templates/hottowel-template.md) używa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Szablon EmberJS SPA](../templates/emberjs-template.md). Ten szablon używa [Członkowskimi](http://emberjs.com/), zaawansowane biblioteki MVC JavaScript, która rozwiązuje szerokiej gamy problemy dotyczące tworzenia aplikacji wzbogaconego klienta.

Szablon SPA Członkowskimi jest ponowna implementacja SPA odcinania szablonu, za pomocą EmberJS i odległość tworzenia szablonów.

## <a name="hot-towel"></a>Ręczników dynamicznej

[Szablon SPA ręczników gorących](../templates/hottowel-template.md). Ten szablon łączy w kilku bibliotek JavaScript, w tym błyskawicznie, Knockout i RequireJS Twitter Bootstrap.

W porównaniu z innych szablonów wymienione w tym miejscu, teample gorących ręczników zapewnia bardziej szczegółowy aplikacji, w którym można tworzyć własne. Istnieje więcej pojęcia pod uwagę, ale po zrozumieniu ich tego szablonu mogą być tylko co, którego szukasz. Jeśli chcesz tworzyć SPA, ale nie zdecyduj, gdzie uruchomić, użyj gorących ręczników oraz w sekundach będziesz mieć SPA i wszystkie narzędzia musisz na nim kompilować.

## <a name="feature-table"></a>Tabeli funkcji

Poniżej przedstawiono funkcje oferowane przez każdego szablonu SPA:


|                        | ASP.NET SPA | Sieci szkieletowej | Breeze/Angular | Błyskawicznie/KO |  Członkowskimi   | Ręczników dynamicznej |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Przykładowe ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Szablon systemu od zera.      |             | &#10003; |                |           |          | &#10003;  |
| Nawigacji i Historia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotekami        |             |          |                |           |          |           |
|        dyrektywy angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Błyskawicznie         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Członkowskimi          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

