---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: "[Jak]: utrwalanie stanu formantu użytkownika podczas odświeżania strony | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym wideo Pels Krzysztof pokazuje, jak można utrwalić stanu jednego lub więcej obiektów w formancie użytkownika. Po pierwsze kontrola użytkownika jest tworzony reprezentujący abilit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="62295-104">[Jak]: utrwalanie stanu formantu użytkownika podczas odświeżania strony</span><span class="sxs-lookup"><span data-stu-id="62295-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="62295-105">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="62295-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="62295-106">W tym wideo Pels Krzysztof pokazuje, jak można utrwalić stanu jednego lub więcej obiektów w formancie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="62295-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="62295-107">Najpierw kontrolkę użytkownika jest tworzony, który reprezentuje zdolność użytkownikowi określić kryteria filtru wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="62295-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="62295-108">Ponadto pomocnik klasy filtru jest tworzony do przechowywania informacji o filtrze.</span><span class="sxs-lookup"><span data-stu-id="62295-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="62295-109">Kilka elementów interfejsu użytkownika są dodawane do formantu filtru wraz z niektórych metod i właściwości do przechowywania informacji o filtrach w wystąpieniu klasy filtru.</span><span class="sxs-lookup"><span data-stu-id="62295-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="62295-110">Następnie trwałości formantu użytkownika jest implementowane za pomocą metody RegisterRequiresControlState i skojarzone metody zapisać/przywrócić.</span><span class="sxs-lookup"><span data-stu-id="62295-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="62295-111">Te metody przechowywania wystąpienia klasy filtru i jego danych podczas ogłaszania zwrotnego strony.</span><span class="sxs-lookup"><span data-stu-id="62295-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="62295-112">Na koniec jest omówienie sposobu przechowywania wielu obiektów w implementacji stan formantu.</span><span class="sxs-lookup"><span data-stu-id="62295-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="62295-113">&#9654; Obejrzyj klip wideo (minuty 23)</span><span class="sxs-lookup"><span data-stu-id="62295-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
