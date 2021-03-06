---
title: Azure Data Lake Analytics'i Node.js için Azure SDK'yı kullanarak yönetme
description: Bu makale, Data Lake Analytics hesaplarını, veri kaynaklarını, işlerini ve kullanıcılarını Node.js için Azure SDK'yı kullanarak yönetmeyi açıklar.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 12/05/2016
ms.custom: devx-track-js
ms.openlocfilehash: ca1c865b31739e7163b0db26cb666a8a42edbaa2
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218468"
---
# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Azure Data Lake Analytics'i Node.js için Azure SDK'yı kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makale, Node.js için Azure SDK ile yazılmış bir uygulama kullanarak Azure Data Lake Analytics hesaplarının, veri kaynaklarının, kullanıcılarının ve işlerinin nasıl yönetileceğini açıklar. 

Aşağıdaki sürümler desteklenir:
* **Node.js sürümü: 0.10.0 veya üzeri**
* **Hesap için REST API sürümü: 2015-10-01-önizleme**
* **Katalog için REST API sürümü: 2015-10-01-önizleme**
* **İş için REST API sürümü: 2016-03-20-önizleme**

## <a name="features"></a>Özellikler
* Hesap yönetimi: oluşturma, alma, listeleme, güncelleştirme ve silme.
* İş yönetimi: gönderme, alma, listeleme ve iptal etme.
* Katalog yönetimi: alma ve listeleme.

## <a name="how-to-install"></a>Yükleme
```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kimlik doğrulama
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Data Lake Analytics istemcisi oluşturma
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var accountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>İşlerin listesini alma
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Data Lake Analytics Kataloğu'ndaki veritabanlarının listesini alma
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Ayrıca bkz.
* [Node.js için Microsoft Azure SDK](https://github.com/azure/azure-sdk-for-node)
