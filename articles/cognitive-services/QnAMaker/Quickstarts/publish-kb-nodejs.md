---
title: "Hızlı başlangıç: Node.js için REST API 'lerle Soru-Cevap Oluşturma"
description: Bu hızlı başlangıçta, Node.js için Soru-Cevap Oluşturma REST API 'Lerine nasıl başlacağınız gösterilmektedir. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-js
ms.topic: how-to
ms.openlocfilehash: 645f9ffb1d907eb576f5414b593ef57679d26daa
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351155"
---
# <a name="quickstart-qna-maker-rest-apis-for-nodejs"></a>Hızlı başlangıç: Node.js için REST API 'Leri Soru-Cevap Oluşturma

Node.js için Soru-Cevap Oluşturma REST API 'Leri ile çalışmaya başlayın. Temel görevler için örnek kodu denemek üzere bu adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.

Node.js için Soru-Cevap Oluşturma REST API 'Lerini kullanın:

* Bilgi bankası oluşturma
* Bilgi bankasını değiştirme
* Bilgi bankası yayımlama
* Bilgi bankasını silme
* Bilgi Bankası indirin
* İşlemin durumunu al

[Başvuru belgeleri](/rest/api/cognitiveservices/qnamaker/knowledgebase)  |  [Node.js örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/rest-api)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org)geçerli sürümü.
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-qna-maker-azure-resource"></a>Soru-Cevap Oluşturma Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](../../cognitive-services-apis-create-account.md) veya [Azure CLI](../../cognitive-services-apis-create-account-cli.md) kullanarak soru-cevap oluşturma için bir kaynak oluşturun.

Kaynağından bir anahtar aldıktan sonra, ve adlı kaynak için [ortam değişkenleri oluşturun](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) `QNAMAKER_RESOURCE_KEY` `QNAMAKER_AUTHORING_ENDPOINT` . Azure portal kaynağın **hızlı başlangıç** sayfasında bulunan anahtar ve uç nokta değerlerini kullanın.

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir myapp && cd myapp
```

`npm init -y`Bir düğüm dosyası oluşturmak için komutunu çalıştırın `package.json` .

```console
npm init -y
```

`reqeuestretry`Ve `request` NPM paketlerini ekleyin:

```console
npm install requestretry request --save
```

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için Soru-Cevap Oluşturma REST API 'Leri ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Bilgi Bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankasını değiştirme](#replace-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [Bilgi Bankası indirin](#download-the-knowledge-base)
* [İşlemin durumunu al](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Adlı bir dosya oluşturun `rest-apis.js` ve aşağıdaki bağımlılıkları ekleyin.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="dependencies":::

## <a name="add-utility-functions"></a>Yardımcı program işlevleri ekleme

Aşağıdaki yardımcı program işlevlerini ekleyin.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="utility":::

## <a name="add-azure-resource-information"></a>Azure Kaynak bilgilerini ekleme

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

Aşağıdaki ortam değerlerini ayarlayın:

* `QNAMAKER_RESOURCE_KEY` - **Anahtar** bir 32 karakter dizesidir ve **hızlı başlangıç** sayfasında soru-cevap oluşturma kaynağında Azure Portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.
* `QNAMAKER_AUTHORING_ENDPOINT` -Kendi biçimindeki yazma uç noktanız, `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` **kaynak adınızı** içerir. Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="authorization":::

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, bir JSON nesnesinden oluşturulan soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**.
* **Dosyalar** -herhangi bir izin gerektirmeyen yerel dosyalar.
* **URL 'ler** -genel olarak kullanılabilir URL 'ler.

[Bilgi Bankası oluşturmak için REST API](/rest/api/cognitiveservices/qnamaker/knowledgebase/create)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="createKb":::

## <a name="replace-a-knowledge-base"></a>Bilgi bankasını değiştirme

[Bilgi bankasını değiştirmek için REST API](/rest/api/cognitiveservices/qnamaker/knowledgebase/replace)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="replaceKb":::

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Bilgi Bankası 'nı yayımlayın. Bu işlem, bilgi bankasını bir HTTP sorgu tahmin uç noktasından kullanılabilir hale getirir.

[Bilgi Bankası yayımlamak için REST API](/rest/api/cognitiveservices/qnamaker/knowledgebase/publish)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="publishKb":::

## <a name="download-the-knowledge-base"></a>Bilgi Bankası 'nı indirin

[Bilgi Bankası indirmek için REST API](/rest/api/cognitiveservices/qnamaker/knowledgebase/download)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="downloadKb":::

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası 'nı kullanarak işiniz bittiğinde silin.

[Bilgi Bankası 'nı silmek için REST API](/rest/api/cognitiveservices/qnamaker/knowledgebase/delete)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="deleteKb":::

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma işlemi gibi uzun süre çalışan işlemler, ayrı bir REST API çağrısıyla denetlenmesi gereken bir işlem KIMLIĞI döndürür. Bu işlev, oluşturma yanıtının gövdesini alır. Önemli anahtar, `operationState` yoklamaya devam etmeniz gerektiğini belirleyen ' dır.

[Bilgi Bankası 'ndaki işlemleri izlemek için REST API](/rest/api/cognitiveservices/qnamaker/operations/getdetails)kullanın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="operationDetails":::

## <a name="add-main-method"></a>Main yöntemi ekle

Aşağıdaki yöntemi ekleyin `main` .

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="main":::

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node rest-apis.js` uygulama dizininizdeki komutla çalıştırın.

```console
node rest-apis.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Öğretici: bir KB oluşturun ve yanıtlayın](./create-publish-knowledge-base.md)

* [Soru-Cevap Oluşturma API'si nedir?](../Overview/overview.md)
* [Bilgi bankası düzenleme](../how-to/edit-knowledge-base.md)
* [Kullanım analizlerini al](../how-to/get-analytics-knowledge-base.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/rest-api/rest-api.js)' da bulunabilir.