---
title: "Wyłączanie ochrony ładunków, której klucze zostały odwołane."
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak wyłączyć ochronę danych i chronione przy użyciu kluczy, które od chwili zostały odwołane, w aplikacji platformy ASP.NET Core."
keywords: "Klucze, IPersistedDataProtector odwołany platformy ASP.NET Core, ochrony danych"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d431f0bbe7152525c9a360a6e90bccbd26be93d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Wyłączanie ochrony ładunków, której klucze zostały odwołane.

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Interfejsy API ochrony danych platformy ASP.NET Core nie są głównie przeznaczone do nieograniczonego trwałość ładunki poufne. Innych technologii, takich jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczonego magazynu i mają możliwości odpowiednio silne zarządzania kluczami. Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu interfejsy API ochrony danych platformy ASP.NET Core dla długoterminowej ochrony poufnych danych. Klucze są nigdy nie usuwane z pierścienia klucza, więc `IDataProtector.Unprotect` zawsze można odzyskać istniejących ładunków tak długo, jak klucze są dostępne i prawidłowe.

Jednak problemy występują, gdy developer podejmie próbę wyłączania ochrony danych, który jest chroniony za pomocą klucza odwołane, jako `IDataProtector.Unprotect` spowoduje zgłoszenie wyjątku w takim przypadku. Może to być poprawnie dla ładunków krótkim okresie lub przejściowy (na przykład tokeny uwierzytelniania), tego rodzaju ładunki można łatwo odtworzyć przez system, a w najgorszym przypadku użytkownik może być konieczne ponowne zalogowanie. Nawet w przypadku utrwalonego ładunków, o `Unprotect` throw może prowadzić do utraty danych można zaakceptować.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Na potrzeby scenariusza umożliwienia ładunków do usunięcia ochrony nawet w wypadku klucze odwołane, system ochrony danych zawiera `IPersistedDataProtector` typu. Można pobrać wystąpienia `IPersistedDataProtector`, wystarczy pobrać wystąpienia `IDataProtector` w sposób normalne i spróbuj wykonać rzutowanie `IDataProtector` do `IPersistedDataProtector`.

> [!NOTE]
> Nie wszystkie `IDataProtector` wystąpienia mogą być rzutowane na `IPersistedDataProtector`. Programiści powinni używać języka C# jako operator lub podobne, aby uniknąć wyjątki środowiska uruchomieniowego spowodowane przez nieprawidłowy rzutowania i powinny być przygotowane do odpowiednią obsługę w przypadku awarii.

`IPersistedDataProtector`udostępnia następujące powierzchni interfejsu API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Ten interfejs API przyjmuje chronione obciążenie (w postaci tablicy bajtów) i zwraca niechronione ładunku. Nie ma żadnych przeciążenia opartego na ciągach. Dostępne są następujące dwa parametry wyjściowe.

* `requiresMigration`: ustawionej na wartość true, jeśli klucz używany do ochrony tego ładunku nie jest już aktywne domyślny klucz, np. klucz używany do ochrony tego ładunku jest stary i ma klucz Wycofanie operacji ponieważ nastąpi miejsce. Obiekt wywołujący możesz rozważyć ponownej ochrony ładunku w zależności od ich potrzeb biznesowych.

* `wasRevoked`: zostanie ustawiona na true, jeśli klucz używany do ochrony tego ładunku został odwołany.

>[!WARNING]
> Zachować wyjątkową ostrożność podczas przekazywania `ignoreRevocationErrors: true` do `DangerousUnprotect` metody. Jeśli po wywołaniu tej metody `wasRevoked` ma wartość true, a następnie klucz używany do ochrony tego ładunku został odwołany i autentyczności ładunku powinny być traktowane jako podejrzana. W takim przypadku tylko kontynuowania działania na niechronionych ładunku, jeśli masz niektórych oddzielne gwarancji czy jest autentyczna, np. jego obiektu pochodzące z bezpiecznej bazie danych, a nie są wysyłane przez klienta niezaufanych sieci web.

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]