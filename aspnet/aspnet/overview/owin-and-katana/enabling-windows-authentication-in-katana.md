---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Włączanie uwierzytelniania Windows w projekcie Katana | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'W tym artykule przedstawiono sposób włączania uwierzytelniania Windows w projekcie Katana. Poruszono w nim dwa scenariusze: Korzystanie z usług IIS do hosta Katana i za pomocą HttpListener na potrzeby samodzielnego hostowania Kat...'
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 80bdc3c76c8867dc559e80a794ac8bee84b47646
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826329"
---
<a name="enabling-windows-authentication-in-katana"></a>Włączanie uwierzytelniania Windows w projekcie Katana
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> W tym artykule przedstawiono sposób włączania uwierzytelniania Windows w projekcie Katana. Poruszono w nim dwa scenariusze: Korzystanie z usług IIS do hosta Katana i za pomocą HttpListener na potrzeby samodzielnego hostowania Katana w procesie niestandardowym. Dziękujemy za Barry Dorrans David Matson i Chris Ross przeglądania w tym artykule.


Katana to implementacja firmy Microsoft [OWIN](http://owin.org/), Open Web Interface for .NET. Możesz przeczytać wprowadzenie do OWIN i Katana [tutaj](an-overview-of-project-katana.md). Architektura OWIN zawiera kilka warstw:

- Host: Zarządza proces, w którym jest uruchamiane w potoku OWIN.
- Serwer: Otwiera gniazda sieciowego i nasłuchuje żądań.
- Oprogramowanie pośredniczące: Przetwarza żądania HTTP i odpowiedzi.

Katana aktualnie obsługuje dwa serwery, które obsługuje zintegrowane uwierzytelnianie Windows:

- **Microsoft.Owin.Host.SystemWeb**. Używa usług IIS przy użyciu potoku platformy ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Ten serwer obecnie jest to opcja domyślna, gdy hostingu samodzielnego Katana.

> [!NOTE]
> Katana nie jest aktualnie dostępny oprogramowanie pośredniczące OWIN do uwierzytelniania Windows, ponieważ ta funkcja jest już dostępne na serwerach.


## <a name="windows-authentication-in-iis"></a>Windows uwierzytelniania w usługach IIS

Używając Microsoft.Owin.Host.SystemWeb, można po prostu włącz uwierzytelnianie Windows w usługach IIS.

Zacznijmy od utworzenia nowej aplikacji platformy ASP.NET przy użyciu szablonu projektu "Aplikacja sieci Web ASP.NET pusty".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Następnie dodaj pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Teraz Dodaj klasę o nazwie `Startup` następującym kodem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

To wszystko, czego potrzebujesz do tworzenia aplikacji "Hello world" dla OWIN, uruchomionych w usługach IIS. Naciśnij klawisz F5, aby debugować aplikację. Powinien zostać wyświetlony "Hello World!" w oknie przeglądarki.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Następnie firma Microsoft będzie włączyć uwierzytelnianie Windows w usługach IIS Express. Z **widoku** menu, wybierz opcję **właściwości**. Kliknij nazwę projektu w Eksploratorze rozwiązań, aby wyświetlić właściwości projektu.

W **właściwości** oknie **uwierzytelnianie anonimowe** do **wyłączone** i ustaw **uwierzytelniania Windows** do  **Włączone**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Po uruchomieniu aplikacji w programie Visual Studio, usług IIS Express będzie wymagać poświadczeń Windows. Widać to przy użyciu [Fiddler](http://fiddler2.com/home) lub innego protokołu HTTP, narzędzia debugowania. Poniżej przedstawiono przykład odpowiedzi HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Nagłówki WWW-Authenticate, w tym odpowiedzi wskazują, że serwer obsługuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokołu, który korzysta z protokołu Kerberos lub NTLM.

Później, podczas wdrażania aplikacji na serwerze, postępuj zgodnie z [te kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Aby włączyć uwierzytelnianie Windows w usługach IIS na tym serwerze.

## <a name="windows-authentication-in-httplistener"></a>Uwierzytelnianie Windows w HttpListener

Jeśli są używane na potrzeby samodzielnego hostowania Katana Microsoft.Owin.Host.HttpListener, można włączyć uwierzytelnianie Windows bezpośrednio na **HttpListener** wystąpienia.

Najpierw utwórz nową aplikację konsoli. Następnie dodaj pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Teraz Dodaj klasę o nazwie `Startup` następującym kodem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Ta klasa implementuje w tym samym przykładzie "Hello world" z przed, ale także ustawia uwierzytelniania Windows jako schematu uwierzytelniania.

Wewnątrz `Main` funkcji, zacznij potoku OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

W narzędziu Fiddler, aby upewnić się, że aplikacja używa uwierzytelniania Windows można wysyłać żądania:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Tematy pokrewne

[Omówienie projektu Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Omówienie uwierzytelniania formularzy OWIN w MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
