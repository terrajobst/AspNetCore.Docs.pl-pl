---
title: Wyłącz ochronę ładunków, której klucze zostały odwołane w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wyłączyć ochronę danych i chronione przy użyciu kluczy, które od chwili zostały odwołane, w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: b721bba63d0673f4e22fd9d1456af33489a2a389
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a><span data-ttu-id="be969-103">Wyłącz ochronę ładunków, której klucze zostały odwołane w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be969-103">Unprotect payloads whose keys have been revoked in ASP.NET Core</span></span>


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="be969-104">Interfejsy API ochrony danych platformy ASP.NET Core nie są głównie przeznaczone do nieograniczonego trwałość ładunki poufne.</span><span class="sxs-lookup"><span data-stu-id="be969-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="be969-105">Innych technologii, takich jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczonego magazynu i mają możliwości odpowiednio silne zarządzania kluczami.</span><span class="sxs-lookup"><span data-stu-id="be969-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="be969-106">Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu interfejsy API ochrony danych platformy ASP.NET Core dla długoterminowej ochrony poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="be969-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="be969-107">Klucze są nigdy nie usuwane z pierścienia klucza, więc `IDataProtector.Unprotect` zawsze można odzyskać istniejących ładunków tak długo, jak klucze są dostępne i prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="be969-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="be969-108">Jednak problemy występują, gdy developer podejmie próbę wyłączania ochrony danych, który jest chroniony za pomocą klucza odwołane, jako `IDataProtector.Unprotect` spowoduje zgłoszenie wyjątku w takim przypadku.</span><span class="sxs-lookup"><span data-stu-id="be969-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="be969-109">Może to być poprawnie dla ładunków krótkim okresie lub przejściowy (na przykład tokeny uwierzytelniania), tego rodzaju ładunki można łatwo odtworzyć przez system, a w najgorszym przypadku użytkownik może być konieczne ponowne zalogowanie.</span><span class="sxs-lookup"><span data-stu-id="be969-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="be969-110">Nawet w przypadku utrwalonego ładunków, o `Unprotect` throw może prowadzić do utraty danych można zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="be969-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="be969-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="be969-111">IPersistedDataProtector</span></span>

<span data-ttu-id="be969-112">Na potrzeby scenariusza umożliwienia ładunków do usunięcia ochrony nawet w wypadku klucze odwołane, system ochrony danych zawiera `IPersistedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="be969-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="be969-113">Można pobrać wystąpienia `IPersistedDataProtector`, wystarczy pobrać wystąpienia `IDataProtector` w sposób normalne i spróbuj wykonać rzutowanie `IDataProtector` do `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="be969-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="be969-114">Nie wszystkie `IDataProtector` wystąpienia mogą być rzutowane na `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="be969-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="be969-115">Programiści powinni używać języka C# jako operator lub podobne, aby uniknąć wyjątki środowiska uruchomieniowego spowodowane przez nieprawidłowy rzutowania i powinny być przygotowane do odpowiednią obsługę w przypadku awarii.</span><span class="sxs-lookup"><span data-stu-id="be969-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="be969-116">`IPersistedDataProtector` udostępnia następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="be969-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="be969-117">Ten interfejs API przyjmuje chronione obciążenie (w postaci tablicy bajtów) i zwraca niechronione ładunku.</span><span class="sxs-lookup"><span data-stu-id="be969-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="be969-118">Nie ma żadnych przeciążenia opartego na ciągach.</span><span class="sxs-lookup"><span data-stu-id="be969-118">There's no string-based overload.</span></span> <span data-ttu-id="be969-119">Dostępne są następujące dwa parametry wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="be969-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="be969-120">`requiresMigration`: ustawionej na wartość true, jeśli klucz używany do ochrony tego ładunku nie jest już aktywne domyślny klucz, np. klucz używany do ochrony tego ładunku jest stary i ma klucz Wycofanie operacji ponieważ nastąpi miejsce.</span><span class="sxs-lookup"><span data-stu-id="be969-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="be969-121">Obiekt wywołujący możesz rozważyć ponownej ochrony ładunku w zależności od ich potrzeb biznesowych.</span><span class="sxs-lookup"><span data-stu-id="be969-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="be969-122">`wasRevoked`: zostanie ustawiona na true, jeśli klucz używany do ochrony tego ładunku został odwołany.</span><span class="sxs-lookup"><span data-stu-id="be969-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="be969-123">Zachować wyjątkową ostrożność podczas przekazywania `ignoreRevocationErrors: true` do `DangerousUnprotect` metody.</span><span class="sxs-lookup"><span data-stu-id="be969-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="be969-124">Jeśli po wywołaniu tej metody `wasRevoked` ma wartość true, a następnie klucz używany do ochrony tego ładunku został odwołany i autentyczności ładunku powinny być traktowane jako podejrzana.</span><span class="sxs-lookup"><span data-stu-id="be969-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="be969-125">W takim przypadku tylko nadal działających na niechronione ładunku, jeśli masz niektórych oddzielne gwarancji jest autentyczna, np. czy jest pochodzi z bezpiecznej bazie danych nie są wysyłane przez klienta niezaufanych sieci web.</span><span class="sxs-lookup"><span data-stu-id="be969-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
