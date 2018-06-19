---
title: SignalR HubContext
author: rachelappel
description: Dowiedz się, jak używać usługi ASP.NET Core SignalR HubContext wysyłania powiadomień do klientów z poza koncentratora.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726106"
---
# <a name="send-messages-from-outside-a-hub"></a>Wysyłanie komunikatów z poza koncentrator

Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)

Centrum SignalR to core pozyskiwania do wysyłania wiadomości do klientów dołączonych do serwera SignalR. Istnieje również możliwość wysyłania wiadomości w innych miejscach w aplikacji za pomocą `IHubContext` usługi. W tym artykule opisano sposób uzyskiwania dostępu SignalR do `IHubContext` do wysyłania powiadomień do klientów z poza koncentratora.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Pobranie wystąpienia `IHubContext`

W ASP.NET Core SignalR, można uzyskać dostępu do wystąpienia `IHubContext` za pomocą iniekcji zależności. Można wstawić wystąpienia `IHubContext` do kontrolera, oprogramowanie pośredniczące lub innych usług Podpisane. Użyj wystąpienia do wysyłania wiadomości do klientów.

> [!NOTE]
> Ta różni się od biblioteki SignalR platformy ASP.NET, w której używane GlobalHost w celu zapewnienia dostępu do `IHubContext`. Platformy ASP.NET Core ma framework iniekcji zależności, która eliminuje potrzebę tym globalnych pojedynczym wystąpieniu.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Wstaw wystąpienia `IHubContext` w kontrolerze

Można wstawić wystąpienia `IHubContext` do kontrolera, dodając ją z konstruktora:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Teraz, dając im dostęp do wystąpienia `IHubContext`, można wywoływać metod koncentratora tak, jakby był w Centrum w samej siebie.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Pobranie wystąpienia `IHubContext` w oprogramowaniu pośredniczącym

Dostęp `IHubContext` w potoku oprogramowania pośredniczącego w następujący sposób:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Kiedy metod koncentratora są wywoływane z poza `Hub` klasa, nie istnieje żadne wywołującego skojarzone z wywołania. Dlatego nie jest Brak dostępu do `ConnectionId`, `Caller`, i `Others` właściwości.

## <a name="related-resources"></a>Zasoby pokrewne

* [Wprowadzenie](xref:signalr/get-started)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)