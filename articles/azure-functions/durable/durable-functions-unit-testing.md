---
title: Azure Dayanıklı İşlevler birim testi
description: Birim testi Dayanıklı İşlevler nasıl yapılacağını öğrenin.
ms.topic: conceptual
ms.date: 11/03/2019
ms.openlocfilehash: 7786a0a2e2d31086e1938b70e63fe2374e16fe7f
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96601365"
---
# <a name="durable-functions-unit-testing"></a>Dayanıklı İşlevler birim testi

Birim testi modern yazılım geliştirme uygulamalarının önemli bir parçasıdır. Birim testleri, iş mantığı davranışını doğrular ve gelecekte önemli değişikliklerden kaçınmadan korur. Dayanıklı İşlevler, birim testlerinin önemli değişikliklerden kaçınmanıza yardımcı olacak şekilde karmaşıklığa kolayca büyüyebilir. Aşağıdaki bölümlerde,-Orchestration Client, Orchestrator ve Activity işlevlerinin üç işlev türünü test etme işlemi açıklanmaktadır.

> [!NOTE]
> Bu makalede, Dayanıklı İşlevler 1. x ' i hedefleyen Dayanıklı İşlevler uygulamalar için birim testi Kılavuzu sağlanmıştır. Henüz Dayanıklı İşlevler 2. x sürümünde tanıtılan değişiklikler için hesaba güncelleştirilmedi. Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örneklerde aşağıdaki kavramlar ve çerçeveler hakkında bilgi sahibi olmanız gerekir:

* Birim testi

* Dayanıklı İşlevler

* [xUnit](https://github.com/xunit/xunit) -test çerçevesi

* [moq](https://github.com/moq/moq4) -Mocking çerçevesi

## <a name="base-classes-for-mocking"></a>Sahte işlem için temel sınıflar

Mocking Dayanıklı İşlevler 1. x içinde üç soyut sınıf aracılığıyla desteklenir:

* `DurableOrchestrationClientBase`

* `DurableOrchestrationContextBase`

* `DurableActivityContextBase`

Bu sınıflar,, `DurableOrchestrationClient` `DurableOrchestrationContext` ve `DurableActivityContext` düzenleme istemcisi, Orchestrator ve etkinlik yöntemlerini tanımlayan temel sınıflardır. Birim testin iş mantığını doğrulayabilmesi için, bu, taban sınıf yöntemleri için beklenen davranışı ayarlar. Orchestration Istemcisinde ve Orchestrator 'da iş mantığını birim testi için iki adımlı bir iş akışı vardır:

1. Orchestration Client ve Orchestrator işlev imzalarını tanımlarken somut uygulama yerine temel sınıfları kullanın.
2. Birim testlerinde, temel sınıfların davranışını ve iş mantığını doğrular.

Orchestration istemci bağlamasını ve Orchestrator tetikleyicisi bağlamasını kullanan işlevleri test etmek için aşağıdaki paragraflarda daha fazla ayrıntı bulabilirsiniz.

## <a name="unit-testing-trigger-functions"></a>Birim testi tetikleme işlevleri

Bu bölümde, birim testi yeni düzenlemeleri başlatmak için aşağıdaki HTTP tetikleme işlevinin mantığını doğrular.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

Birim testi görevi, `Retry-After` Yanıt yükünde belirtilen üstbilginin değerini doğrulamak olacaktır. Bu nedenle, birim testi `DurableOrchestrationClientBase` tahmin edilebilir davranış sağlamak için bazı yöntemleri sahte hale getirebilir.

İlk olarak, temel sınıfın bir sahte olması gerekir `DurableOrchestrationClientBase` . Sahte, uygulayan yeni bir sınıf olabilir `DurableOrchestrationClientBase` . Ancak [moq](https://github.com/moq/moq4) gibi bir sahte işlem çerçevesinin kullanılması işlemi basitleştirir:

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Daha sonra `StartNewAsync` Yöntem, iyi bilinen bir örnek kimliği döndürecek şekilde yapılır.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Sonraki adımda `CreateCheckStatusResponse` her zaman boş BIR HTTP 200 yanıtı döndürülür.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
            Headers =
            {
                RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
            }
        });
```

`ILogger` Ayrıca şu şekilde olur:

```csharp
    // Mock ILogger
    var loggerMock = new Mock<ILogger>();
```  

Artık, `Run` yöntemi birim testten çağrılır:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object,
        functionName,
        loggerMock.Object);
 ```

 Son adım, çıktıyı beklenen değerle karşılaştırmaktır:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Tüm adımları birleştirdikten sonra birim testi aşağıdaki koda sahip olur:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Birim testi Orchestrator işlevleri

Genellikle daha fazla iş mantığı olduğundan, Orchestrator işlevleri birim testi için daha da ilginç.

Bu bölümde, birim testleri `E1_HelloSequence` Orchestrator işlevinin çıkışını doğrular:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Birim test kodu, bir sahte oluşturmaya başlayacaktır:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Ardından etkinlik yöntemi çağrıları yeniden yapılır:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Birim testi daha sonra yöntemi çağıracaktır `HelloSequence.Run` :

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

Son olarak çıktının doğrulanması gerekir:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Tüm adımları birleştirdikten sonra birim testi aşağıdaki koda sahip olur:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Birim testi etkinlik işlevleri

Etkinlik işlevleri, birim tarafından, dayanıklı olmayan işlevlerle aynı şekilde test edilebilir.

Bu bölümde, birim testi etkinlik işlevinin davranışını doğrulayacaktır `E1_SayHello` :

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Ve birim testleri çıktının biçimini doğrular. Birim testleri doğrudan veya sahte sınıf parametre türlerini kullanabilir `DurableActivityContextBase` :

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [XUnit hakkında daha fazla bilgi](https://xunit.net/docs/getting-started/netcore/cmdline)
> 
> [Moq hakkında daha fazla bilgi](https://github.com/Moq/moq4/wiki/Quickstart)
