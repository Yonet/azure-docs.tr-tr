---
title: Azure Işlevinden dönüş değeri kullanma
description: Azure Işlevleri için dönüş değerlerini yönetmeyi öğrenin
author: craigshoemaker
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 58ee9b682bc97dc4044d811392cf4ff5b51a69fd
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491367"
---
# <a name="using-the-azure-function-return-value"></a>Azure Işlevi dönüş değerini kullanma

Bu makalede, dönüş değerlerinin bir işlev içinde nasıl çalıştığı açıklanmaktadır.

Dönüş değerine sahip dillerde bir işlev [Çıkış bağlamasını](./functions-triggers-bindings.md#binding-direction) bir dönüş değerine bağlayabilirsiniz:

* C# sınıf kitaplığında, çıkış bağlama özniteliğini yöntem dönüş değerine uygulayın.
* Java 'da, çıkış bağlama ek açıklamasını Function yöntemine uygulayın.
* Diğer dillerde, `name` *üzerindefunction.js* özelliğini olarak ayarlayın `$return` .

Birden çok çıkış bağlaması varsa, bunlardan yalnızca biri için dönüş değerini kullanın.

C# ve C# komut dosyasında, çıkış bağlamalarına veri göndermenin alternatif yolları `out` Parametreler ve [toplayıcı nesneleridir](functions-reference-csharp.md#writing-multiple-output-values).

# <a name="c"></a>[C#](#tab/csharp)

Aşağıda, bir çıkış bağlaması için dönüş değerini kullanan ve ardından zaman uyumsuz bir örnek olan C# kodu verilmiştir:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Dosyadaki *function.js* çıktı bağlaması aşağıda verilmiştir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Aşağıda, bir zaman uyumsuz örnek tarafından izlenen C# betik kodu verilmiştir:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="f"></a>[F#](#tab/fsharp)

Dosyadaki *function.js* çıktı bağlaması aşağıda verilmiştir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

F # kodu aşağıda verilmiştir:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Dosyadaki *function.js* çıktı bağlaması aşağıda verilmiştir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript 'te, dönüş değeri için ikinci parametreye gider `context.done` :

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```
# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

Dosyadaki *function.js* çıktı bağlaması aşağıda verilmiştir:

```json
{
    "name": "Response",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{blobname}"
}
```

Bir http çıkış bağlaması için dönüş değerini kullanan PowerShell kodu aşağıda verilmiştir:

```powershell
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = [HttpStatusCode]::OK
    Body = $blobname
    })
```

# <a name="python"></a>[Python](#tab/python)

Dosyadaki *function.js* çıktı bağlaması aşağıda verilmiştir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Python kodu aşağıda verilmiştir:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

# <a name="java"></a>[Java](#tab/java)

Aşağıda, bir çıkış bağlaması için dönüş değerini kullanan Java kodu verilmiştir:

```java
@FunctionName("QueueTrigger")
@StorageAccount("AzureWebJobsStorage")
@BlobOutput(name = "output", path = "output-container/{id}")
public static String run(
  @QueueTrigger(name = "input", queueName = "inputqueue") WorkItem input,
  final ExecutionContext context
) {
  String json = String.format("{ \"id\": \"%s\" }", input.id);
  context.getLogger().info("Java processed queue message. Item=" + json);
  return json;
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri bağlama hatalarını işleme](./functions-bindings-errors.md)
