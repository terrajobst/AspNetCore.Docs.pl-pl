---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Uruchamianie okna podręcznego modalne z kodu serwera (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednakże w niektórych scenariuszach wymagane tego t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="4b370-104">Uruchamianie okna podręcznego modalne z kodu serwera (C#)</span><span class="sxs-lookup"><span data-stu-id="4b370-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="4b370-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4b370-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4b370-106">[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4b370-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="4b370-107">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4b370-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4b370-108">Jednak w niektórych scenariuszach wymagane, że otwarcie modalnych menu podręczne jest wyzwalane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4b370-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="4b370-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4b370-109">Overview</span></span>

<span data-ttu-id="4b370-110">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4b370-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4b370-111">Jednak w niektórych scenariuszach wymagane, że otwarcie modalnych menu podręczne jest wyzwalane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4b370-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="4b370-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4b370-112">Steps</span></span>

<span data-ttu-id="4b370-113">Po pierwsze kontrolkę przycisku ASP.NET sieci web jest wymagany do pokazują, jak działa Kontrola ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="4b370-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="4b370-114">Dodawanie przycisku w &lt;formularza&gt; element na nową stronę:</span><span class="sxs-lookup"><span data-stu-id="4b370-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="4b370-115">Następnie należy kod znaczników dla elementu popup, który chcesz utworzyć.</span><span class="sxs-lookup"><span data-stu-id="4b370-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="4b370-116">Zdefiniuj go jako `<asp:Panel>` kontroli i upewnij się, że zawiera on kontrolkę przycisku.</span><span class="sxs-lookup"><span data-stu-id="4b370-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="4b370-117">Formant ModalPopup oferuje funkcje dokonanie takiego przycisku Zamknij podręcznego; w przeciwnym razie jest prosty sposób pozwól mu znikają.</span><span class="sxs-lookup"><span data-stu-id="4b370-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="4b370-118">Następnie dodaj formant ModalPopup w zestawie narzędzi programu ASP.NET AJAX do strony.</span><span class="sxs-lookup"><span data-stu-id="4b370-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="4b370-119">Ustawianie właściwości przycisku, który jest ładowany formant, przycisk, dzięki czemu znikają i identyfikator rzeczywiste menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="4b370-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="4b370-120">W przypadku wszystkich stron sieci web oparta na programie ASP.NET AJAX; Menedżer skryptów jest wymagane do załadowania wymagane biblioteki JavaScript w przypadku przeglądarek inny element docelowy:</span><span class="sxs-lookup"><span data-stu-id="4b370-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="4b370-121">Można uruchomić przykład w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4b370-121">Run the example in the browser.</span></span> <span data-ttu-id="4b370-122">Po kliknięciu przycisku pojawi się modalne okno podręczne.</span><span class="sxs-lookup"><span data-stu-id="4b370-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="4b370-123">Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagany jest przycisk Nowy:</span><span class="sxs-lookup"><span data-stu-id="4b370-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="4b370-124">Jak widać, kliknij przycisk odświeżania strony generuje i wykonuje `ServerButton_Click()` metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4b370-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="4b370-125">W przypadku tej metody wywołuje funkcję JavaScript `launchModal()` wykonaniu funkcji JavaScript należy dokładnie, będą wykonywane po załadowaniu strony:</span><span class="sxs-lookup"><span data-stu-id="4b370-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="4b370-126">Zadaniem `launchModal()` są wyświetlane ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="4b370-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="4b370-127">`launchModal()` Funkcja jest wykonywana po załadowaniu pełną strony HTML.</span><span class="sxs-lookup"><span data-stu-id="4b370-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="4b370-128">W tej chwili jednak ASP.NET AJAX framework nie został całkowicie załadowany jeszcze.</span><span class="sxs-lookup"><span data-stu-id="4b370-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="4b370-129">W związku z tym `launchModal()` funkcja po prostu ustawia zmienną kontroli ModalPopup musi być wyświetlana w późniejszym czasie na:</span><span class="sxs-lookup"><span data-stu-id="4b370-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="4b370-130">`pageLoad()` Funkcji JavaScript jest specjalnych funkcji, który jest wykonywany po całkowitym załadowaniem ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="4b370-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="4b370-131">W związku z tym dodamy kod do tej funkcji, aby pokazać ModalPopup sterowania, ale tylko wtedy, gdy `launchModal()` została wywołana przed:</span><span class="sxs-lookup"><span data-stu-id="4b370-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="4b370-132">`$find()` Funkcji szuka nazwanego elementu na stronie i oczekuje identyfikator po stronie serwera jako parametr.</span><span class="sxs-lookup"><span data-stu-id="4b370-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="4b370-133">W związku z tym `$find("mpe")` zwraca reprezentację klienta formantu ModalPopup; `show()` metoda umożliwia podręcznego są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="4b370-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="4b370-134">[![Modalnych menu podręczne jest wyświetlane, gdy zostanie kliknięty albo przycisków](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4b370-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="4b370-135">Modalnych menu podręczne jest wyświetlane, gdy zostanie kliknięty albo przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4b370-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4b370-136">Next</span><span class="sxs-lookup"><span data-stu-id="4b370-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
