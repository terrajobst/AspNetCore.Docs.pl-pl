---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Dokumentacja firmy Microsoft
author: xqiu
description: Szablon EmberJS
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823780"
---
<a name="emberjs-template"></a>Szablon EmberJS
====================
przez [Xinyang Qiu](https://github.com/xqiu)

> Szablon EmberJS MVC są zapisywane przez Nathana Totten, Thiago Santos i Xinyang Qiu.
> 
> [Pobierz szablon EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Szablon EmberJS SPA został zaprojektowany ułatwią Ci rozpoczęcie pracy szybkiego tworzenia aplikacji sieci web interactive po stronie klienta przy użyciu EmberJS.

"Aplikacja jednostronicowa" (SPA) jest ogólnym terminem dla aplikacji sieci web, która ładuje z pojedynczą stroną HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowanie nowych stron. Po załadowaniu strony początkowej SPA komunikuje się z serwerem za pośrednictwem żądań AJAX.

![](emberjs-template/_static/image1.png)

AJAX to nic nowego, ale obecnie ma platformy JavaScript, które ułatwiają tworzenie i zarządzanie nimi dużej zaawansowanych aplikacji SPA. Ponadto HTML 5 i CSS3 są ułatwia tworzenie rozbudowanych interfejsów użytkownika.

Szablon EmberJS SPA używa [użyciu](http://emberjs.com/) biblioteki JavaScript do obsługi aktualizacji stron AJAX żądań. Ember.js używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych. Dzięki temu nie trzeba pisać kodu, który przeprowadzi dane JSON i aktualizuje DOM. Zamiast tego należy umieścić deklaratywne atrybuty w formacie HTML, informacje Ember.js sposobu prezentowania danych.

Po stronie serwera jest niemal identyczny szablon EmberJS [szablon KnockoutJS SPA](../introduction/knockoutjs-template.md). Używa platformy ASP.NET MVC do udostępniania dokumentów HTML i Web API platformy ASP.NET do obsługi żądań AJAX od klienta. Aby uzyskać więcej informacji na temat tych aspektów szablonu dotyczą [szablon KnockoutJS](../introduction/knockoutjs-template.md) dokumentacji. Ten temat koncentruje się na temat różnic między szablon Knockout i szablon EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Tworzenie szablonu projektu SPA EmberJS

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.

![](emberjs-template/_static/image2.png)

W **nowy projekt** kreatora wybierz **projekt SPA Ember.js**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Omówienie szablonu SPA EmberJS

Szablon EmberJS używa kombinacji jQuery, Ember.js, Handlebars.js, aby utworzyć płynne interaktywnego interfejsu użytkownika.

Ember.js to biblioteka języka JavaScript, który korzysta ze wzorca MVC po stronie klienta.

- A *szablonu*robaków napisanych w języku szablonów Handlebars, w tym artykule opisano interfejs użytkownika aplikacji. W trybie wydania [kompilatora Handlebars](https://github.com/Myslik/csharp-ember-handlebars) służy do pakietu i skompilować szablon handlebars.
- A *modelu* przechowuje dane aplikacji, która otrzymuje od serwera (listy zadań do wykonania i zadania do wykonania).
- A *kontrolera* zapisuje stan aplikacji. Kontrolery przedstawiają często model danych do odpowiedniego szablonów.
- A *widoku* przekształca pierwotne zdarzeń z aplikacji i przekazuje je do kontrolera.
- A *routera* zarządza stan aplikacji, synchronizacja adresy URL i szablony.

Ponadto biblioteka danych o użyciu może służyć do synchronizowania obiektów JSON (uzyskana z serwera za pomocą interfejsu API RESTful) i modeli klienta.

Szablon EmberJS SPA organizuje skrypty w ośmiu warstwy:

- webapi\_adapter.js, webapi\_serializer.js: rozszerza biblioteki danych o użyciu do pracy z interfejsu API sieci Web platformy ASP.NET.
- Scripts/helpers.js: Definiuje nowy pomocników Handlebars użyciu.
- Scripts/App.js: Utworzy aplikację i konfiguruje karty i serializatora.
- Modeleskrypty/aplikacji/\*js: definiuje modeli.
- Skrypty/app/widoki/\*js: zawiera definicje widoków.
- Skrypty/app/kontrolerów/\*js: definiuje kontrolerów.
- Skrypty/app/tras, Scripts/app/router.js: Definiuje trasy.
- Szablony /\*.hbs: definiuje szablony handlebars.

Spójrzmy na niektóre z tych skryptów bardziej szczegółowo.

## <a name="models"></a>Modele

Modele są definiowane w folderze skrypty/app/modeli. Istnieją dwa pliki modelu: todoItem.js i todoList.js.

**TODO.model.js** definiuje modeli po stronie klienta (przeglądarka) do listy zadań do wykonania. Istnieją dwie klasy modelu: todoItem i listy zadań. W użyciu modele są podklasy DS. Model. Model może mieć właściwości, za pomocą atrybutów:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modele można zdefiniować relacje z innymi modelami:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modele mogą mieć obliczane właściwości, powiązanych do innych właściwości:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modele mogą mieć obserwatora funkcji, które są wywoływane, gdy zmieni się właściwość obserwacji:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Widoki

Widoki są definiowane w folderze skrypty/app/widoków. Widok dokonuje translacji zdarzeń z aplikacji interfejsu użytkownika. Program obsługi zdarzeń można wywołania zwrotnego do funkcji kontrolera lub po prostu bezpośrednio wywoływać kontekstu danych.

Na przykład następujący kod pochodzi z views/TodoItemEditView.js. Definiuje zdarzenia obsługi dla pola wprowadzania tekstu.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Kontroler

Kontrolery są definiowane w folderze skrypty/app/kontrolerów. Do reprezentowania jednego modelu, należy rozszerzyć `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Kontroler może również reprezentować kolekcję modeli, rozszerzając `Ember.ArrayController`. Na przykład TodoListController reprezentuje tablicę `todoList` obiektów. Kontroler sortowane według Identyfikatora listy zadań, w kolejności malejącej:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Kontroler definiuje funkcję o nazwie `addTodoList`, który tworzy nowe listy zadań i dodaje go do tablicy. Aby zobaczyć, jak ta funkcja jest wywoływana, otwórz plik szablonu o nazwie todoListTemplate.html w folderze szablonów. Poniższy kod szablonu wiąże przycisk, aby `addTodoList` funkcji:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Zawiera także kontrolera `error` właściwość, która zawiera komunikat o błędzie. Poniżej przedstawiono kod szablonu, aby wyświetlić komunikat o błędzie (również w todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Trasy

Router.js definiuje trasy i szablonu domyślnego, aby wyświetlić, ustawia stan aplikacji oraz odpowiada adresy URL do tras:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js powoduje załadowanie danych do TodoListRoute przez przesłonięcie funkcji setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Użyciu używa konwencji nazewnictwa w celu dopasowania adresów URL, nazwy tras, kontrolerów i szablony. Aby uzyskać więcej informacji, zobacz [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS dokumentację.

## <a name="templates"></a>Szablony

Folder szablonów zawiera cztery szablony:

- Application.HBS: domyślny szablon, który jest renderowany po uruchomieniu aplikacji.
- About.HBS: szablonu trasy "/ informacje".
- index.HBS: szablon główny trasy "/".
- todoList.hbs: szablonu dla "/ zadań do wykonania" trasy.
- \_navbar.HBS: szablon definiuje menu nawigacji.

Szablon aplikacji działa jak strony wzorcowej. Zawiera nagłówek, stopka i "{{ujścia}}" do wstawienia innych szablonów, w zależności od trasy. Aby uzyskać więcej informacji na temat szablonów aplikacji w użyciu, zobacz [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ TodoList" szablon zawiera dwa wyrażenia pętli. Poza pętla jest `{{#each controller}}`oraz wewnątrz pętli jest `{{#each todos}}`. Poniższy kod przedstawia wbudowaną `Ember.Checkbox` wyświetlić dostosowany `App.TodoItemEditView`i łącze z `deleteTodo` akcji.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Klasy, zdefiniowanej w Controllers/HtmlHelperExensions.cs, definiuje pomocnika pliki funkcji do pamięci podręcznej i Wstaw szablon, gdy **debugowania** jest ustawiona na **true** w pliku Web.config. Ta funkcja jest wywoływana z pliku widok ASP.NET MVC, które są zdefiniowane w Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Wywołać bez argumentów, funkcja powoduje wyświetlenie wszystkich plików szablonu w folderze szablonów. Można również określić podfolder lub pliku określonego szablonu.

Gdy **debugowania** jest **false** w pliku Web.config, aplikacja zawiera element pakietu "~/bundles/templates". Ten element pakietu zostanie dodany w BundleConfig.cs, za pomocą biblioteki kompilatora Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
