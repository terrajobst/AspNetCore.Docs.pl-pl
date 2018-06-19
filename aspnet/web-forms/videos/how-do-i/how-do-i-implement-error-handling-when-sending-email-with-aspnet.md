---
uid: web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
title: '[Jak i.] Implementowanie obsługi błędów podczas wysyłania poczty E-mail za pomocą programu ASP.NET | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Krzysztof Pels pokazano, jak wdrożyć obsługę błędów podczas wysyłania wiadomości e-mail z platformy ASP.NET. ADAM tworzy stronę sieci web ASP.NET do wysyłania wiadomości e-mail, pokazuje, jak skonfigurować & lt....
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/06/2008
ms.topic: article
ms.assetid: c02ffd50-aa19-4cdc-b1bf-760989979a61
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
msc.type: video
ms.openlocfilehash: a860eb958956bcac1682e8be7d4e70a9d44b2c4d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572234"
---
<a name="how-do-i-implement-error-handling-when-sending-email-with-aspnet"></a><span data-ttu-id="ea7e8-104">[Jak i.] Implementowanie obsługi błędów podczas wysyłania wiadomości E-mail z platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea7e8-104">[How Do I:] Implement Error Handling when Sending Email with ASP.NET</span></span>
====================
<span data-ttu-id="ea7e8-105">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ea7e8-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ea7e8-106">Krzysztof Pels pokazano, jak wdrożyć obsługę błędów podczas wysyłania wiadomości e-mail z platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ea7e8-106">Chris Pels shows how to implement error handling when sending an email with ASP.NET.</span></span> <span data-ttu-id="ea7e8-107">ADAM tworzy stronę sieci web ASP.NET do wysyłania wiadomości e-mail, przedstawiono sposób konfigurowania &lt;mailSettings&gt; w pliku web.config, w tym artykule opisano klasy System.Net.Mail i jak jest używany do tworzenia i wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="ea7e8-107">He creates an ASP.NET web page to send email, shows how to configure &lt;mailSettings&gt; in the web.config file, describes the System.Net.Mail class and how it's used to create and send email messages.</span></span> <span data-ttu-id="ea7e8-108">Następnie dodaje przy użyciu klasy wyjątku System.Net.Mail, które zawierają informacje o błędach, które mogą wystąpić podczas wysyłania wiadomości e-mail i monitoruje wyliczenie SmtpStatusCode, który zawiera listę możliwych wartości podczas wysyłania wiadomości e-mail z Obsługa błędów SmtpClient.</span><span class="sxs-lookup"><span data-stu-id="ea7e8-108">He then adds error handling using System.Net.Mail exception classes, which provide information about errors that can occur when sending email, and reviews the SmtpStatusCode enumeration, which provides a list of possible outcomes when sending an email with the SmtpClient.</span></span> <span data-ttu-id="ea7e8-109">Na koniec wysyła on testowa wiadomość e-mail, zgłasza wyjątek, który monitoruje błąd obsługi informacji w debugerze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea7e8-109">Finally, he sends a test email that raises an exception and reviews the error handling information in the Visual Studio debugger.</span></span>

[<span data-ttu-id="ea7e8-110">&#9654; Obejrzyj klip wideo (minuty 24)</span><span class="sxs-lookup"><span data-stu-id="ea7e8-110">&#9654; Watch video (24 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-error-handling-when-sending-email-with-aspnet)
