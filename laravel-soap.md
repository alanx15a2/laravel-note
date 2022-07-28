# Laravel-soap

這是針對 CodeDredd/laravel-soap 的筆記

## 取得 Request body

    $body = $rs->transferStats->getRequest()->getBody();
    $body->rewind();
    dd($body->getContents());
