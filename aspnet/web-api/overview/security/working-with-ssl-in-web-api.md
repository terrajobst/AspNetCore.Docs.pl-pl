---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Praca z protokołem SSL w składniku Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje, jak używać protokołu SSL przy użyciu interfejsu API sieci Web platformy ASP.NET, w tym o korzystaniu z certyfikatów klientów SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4e14607f11fcd376b4ceca4bf7862259abe015e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396633"
---
<a name="working-with-ssl-in-web-api"></a>Praca z protokołem SSL w składniku Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Kilka typowych schematów uwierzytelniania nie są bezpieczne przy użyciu zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy Wyślij niezaszyfrowane poświadczeń. Do zabezpieczenia, te schematy uwierzytelniania *musi* używania protokołu SSL. Ponadto certyfikaty klienta SSL może służyć do uwierzytelniania klientów.

## <a name="enabling-ssl-on-the-server"></a>Włączanie protokołu SSL na serwerze

Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszy:

- Utwórz lub uzyskać certyfikat. W przypadku testowania można utworzyć certyfikatu z podpisem własnym.
- Dodaj powiązanie HTTPS.

Aby uzyskać więcej informacji, zobacz [jak się protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

W przypadku lokalnego testowania można włączyć protokołu SSL w usługach IIS Express z programu Visual Studio. W oknie właściwości ustaw **włączony protokół SSL** do **True**. Zanotuj wartość ustawienia **adresu URL protokołu SSL**; Użyj tego adresu URL do testowania połączeń HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Wymuszanie protokołu SSL w kontrolerze interfejsu API sieci Web

Jeśli masz zarówno przy użyciu protokołu HTTPS i powiązanie HTTP, klienci nadal mogą używać protokołu HTTP dostęp do witryny. Mogą zezwalać na niektóre zasoby, które mają być dostępne za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL. W takim przypadku umożliwia filtr akcji wymagać protokołu SSL dla chronionych zasobów. Poniższy kod pokazuje filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Dodaj filtr do wszystkich działań interfejsu API sieci Web wymaga protokołu SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certyfikaty klienta SSL

Protokół SSL umożliwia uwierzytelnianie za pomocą certyfikatów infrastruktury kluczy publicznych. Serwer, musisz podać certyfikat, który uwierzytelnia serwer do klienta. Mniej typowe dla od klienta zapewnienia certyfikatu do serwera, ale są jedną z opcji uwierzytelnianie klientów. Aby używać certyfikatów klientów przy użyciu protokołu SSL, należy rozdystrybuować podpisanych certyfikatów dla użytkowników. Dla wielu typów aplikacji nie będzie to środowisko użytkownika, ale w niektórych środowiskach (na przykład enterprise) może być możliwe.

| Zalety | Wady |
| --- | --- |
| -Certyfikat poświadczeń są mocniejszych niż nazwy użytkownika i hasła. — SSL zapewnia pełną bezpiecznego kanału, za pomocą uwierzytelniania, integralności i szyfrowania wiadomości. | -Należy uzyskać i zarządzanie certyfikatami infrastruktury kluczy publicznych. Platforma klienta muszą obsługiwać certyfikatów klientów SSL. |

Aby skonfigurować usługi IIS, aby zaakceptować certyfikaty klienta, otwórz Menedżera usług IIS i wykonaj następujące czynności:

1. Kliknij węzeł witryny w widoku drzewa.
2. Kliknij dwukrotnie **ustawienia protokołu SSL** funkcji w środkowym okienku.
3. W obszarze **certyfikaty klienta**, wybierz jedną z następujących opcji: 

    - **Zaakceptuj**: IIS będzie akceptować certyfikat klienta, ale nie wymaga jednego.
    - **Wymagaj**: wymagany certyfikat klienta. (Aby włączyć tę opcję, należy również wybrać "Wymagaj protokołu SSL")

Te opcje można również ustawić w pliku ApplicationHost.config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** flagi oznacza, że usługi IIS będzie akceptować certyfikat klienta, ale nie wymaga jednego (odpowiada opcji "Zaakceptuj" w Menedżerze usług IIS). Aby jest wymagany certyfikat, należy ustawić **SslRequireCert** flagi. W przypadku testowania można również ustawić te opcje w usługach IIS Express, w lokalnym hosta aplikacji. Plik konfiguracji znajduje się w "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Tworzenie certyfikatu klienta do testowania

Do celów testowych możesz użyć [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) do utworzenia certyfikatu klienta. Najpierw utwórz urząd główny testu:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Użycie narzędzia MakeCert wyświetli monit o podanie hasła klucza prywatnego.

Następnie Dodaj certyfikat do badania serwera "Zaufane główne urzędy certyfikacji" przechowywać w następujący sposób:

1. Otwórz program MMC.
2. W obszarze **pliku**, wybierz opcję **Dodaj/Usuń przystawkę**.
3. Wybierz **konto komputera**.
4. Wybierz **komputera lokalnego** i Ukończ pracę kreatora.
5. W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".
6. Na **akcji** menu wskaż **wszystkie zadania**, a następnie kliknij przycisk **importu** można uruchomić Kreatora importu certyfikatów.
7. Przejdź do pliku certyfikatu, TempCA.cer.
8. Kliknij przycisk **Otwórz**, następnie kliknij przycisk **dalej** i Ukończ pracę kreatora. (Możesz wyświetli monit o ponowne wprowadzenie hasła.)

Teraz można utworzyć certyfikatu klienta, który jest podpisany przez pierwszy certyfikat:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Przy użyciu certyfikatów klienta w interfejsie Web API

Po stronie serwera można uzyskać certyfikat klienta, przez wywołanie metody [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) komunikatu żądania. Metoda zwraca wartość null, jeśli brak certyfikatu klienta. W przeciwnym razie zwraca **X509Certificate2** wystąpienia. Aby uzyskać informacje na podstawie certyfikatu, takie jak wystawcy i tematu, należy użyć tego obiektu. Następnie przy użyciu tych informacji do uwierzytelniania i/lub autoryzacji.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
