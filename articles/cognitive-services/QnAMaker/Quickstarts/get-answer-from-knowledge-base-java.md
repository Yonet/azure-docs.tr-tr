---
title: "Hızlı başlangıç: Bilgi Bankası 'ndan yanıt alın-REST, Java-Soru-Cevap Oluşturma"
description: Bu Java REST tabanlı hızlı başlangıç, Bilgi Bankası 'ndan programlama yoluyla yanıt alma konusunda size kılavuzluk eder.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-java
ms.topic: how-to
ms.openlocfilehash: 8075371bddfc92d7477b3b036575836aa65f1cd0
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96346040"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-java"></a>Hızlı başlangıç: Java ile Bilgi Bankası 'ndan bir soruya yanıt alın

Bu hızlı başlangıçta, yayımlanan Soru-Cevap Oluşturma bilgi bankasından programlı bir yanıt alma işlemi adım adım gösterilmektedir. Bilgi Bankası, SSS gibi [veri kaynaklarından](../index.yml) gelen sorular ve yanıtlar içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap oluşturma hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) , en fazla tahmini yanıtı içerir.

[Başvuru belgeleri](/rest/api/cognitiveservices/qnamaker4.0/Runtime)  |  [Örnek](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/get-answer/GetAnswer.java)

## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](/azure/developer/java/fundamentals/java-jdk-long-term-support)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, HTTP bileşenlerinden Apache [http istemcisini](https://hc.apache.org/httpcomponents-client-ga/) kullanır. Projenize aşağıdaki Apache HTTP istemci kitaplıklarını eklemeniz gerekir:
    * HttpClient-4.5.3. jar
    * httpcore-4.4.6. jar
    * Commons-logging-1.2. jar
* [Visual Studio Code](https://code.visualstudio.com/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı almak için Azure panonuzda Soru-Cevap Oluşturma kaynağınız için **kaynak yönetimi** altındaki **anahtarlar** ' ı seçin.
* **Yayımlama** sayfası ayarları. Yayımlanmış bir bilgi tabanınız yoksa, boş bir Bilgi Bankası oluşturun ve ardından **Ayarlar** sayfasına bir Bilgi Bankası alın ve ardından yayımlayın. [Bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)'nı indirip kullanabilirsiniz.

    Yayımlama sayfası ayarları, POST yol değerini, ana bilgisayar değerini ve EndpointKey değerini içerir.

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-java-file"></a>Java dosyası oluşturma

VSCode 'u açın ve adlı yeni bir dosya oluşturun `GetAnswer.java` ve aşağıdaki sınıfı ekleyin:

```Java
public class GetAnswer {

    public static void main(String[] args)
    {

    }
}
```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Bu hızlı başlangıçta HTTP istekleri için Apache sınıfları kullanılmaktadır. GetAnswer sınıfının üstünde, dosyanın en üstünde, `GetAnswer.java` projeye gerekli bağımlılıkları ekleyin:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="dependencies":::

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Sınıfının üst kısmında `GetAnswer.java` , soru-cevap oluşturma erişmek için gereken sabitleri ekleyin. Bu değerler, Bilgi Bankası 'nı yayımladıktan sonra **Yayımla** sayfasında bulunur.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="constants":::

## <a name="add-a-post-request-to-send-question"></a>Soru göndermek için bir POST isteği ekleyin

Aşağıdaki kod, soruyu bilgi tabanına göndermek için Soru-Cevap Oluşturma API'si HTTPS isteği yapar ve yanıtı alır:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="post":::

`Authorization`Üstbilginin değeri dizeyi içerir `EndpointKey` .

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Komut satırından programı derleyin ve çalıştırın. İstek otomatik olarak Soru-Cevap Oluşturma API'si gönderilir, ardından konsol penceresine yazdırılır.

1. Dosyayı oluşturun:

    ```bash
    javac -cp "lib/*" GetAnswer.java
    ```

1. Dosyayı çalıştırın:

    ```bash
    java -cp ".;lib/*" GetAnswer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase)