みなさんこんにちは、新卒社員のJANです。今回はAjaxの基礎知識について説明します。


# 目次

1.[Ajaxとは](#Ajax)<br>
2.[XMLHttpRequestオブジェクトの生成](#XHR)<br>
3.[サーバへのリクエスト](#Request)<br>
4.[サーバからのレスポンス](#Response)<br>
5.[参考URL](#reference)<br>

<a id="Ajax"></a>
# Ajaxとは
 Ajax(Asynchronous JavaScript And Xml)Webページ全体のリロードを伴わずに非同期でサーバとの間でデータのやり取りを`JavaScript`、`XML`、`DOM`、`XMLHttpRequest`という複数の技術の組み合わせで通信を行う、WEBアプリケーション開発に必要に不可欠な手法の一つです。Ajaxと取り入れるとバックグラウンドでサーバと非同期通信することで、ページ内で必要な部分だけが更新でき、ユーザーにもストレス感じさせずに、快適に操作することができます。

<a id="XHR"></a>
 # XMLHttpRequestオブジェクトの生成
AjaxはほとんどJavaScriptで構成されていて、サーバにHTTPリクエストを送るための機能を提供しています。XMLHttpRequestオブジェクトを生成することにで非同期通信を行い、サーバ側からのファイルの内容が取得できます。データ形式としてXMLとJSONが使えます。


* XMLHttpRequest<br>
`XMLHttpRequest`はMicrosoftによって設計され、有名なブラウザーのGoogle,Mozilla,FireFox,Operaで使われています。
`XMLHttpRequst`のオブジェクトを生成するには、ブラウザ毎に実装方法が異なります。
chrome,firefox,safari,opera,IE7+の場合は`XMLHttpRequest`オブジェクトを生成して使いますが、サポートされていない古いIE5,IE6などのブラウザでは`ActiveXObject`オブジェクトを生成しなければなりません。

    * 「chrome」、「firefox」、「safari」、「opera」、「IE7+」

    ```
    let xhr = new XMLHttpRequest();
    ```
    
    * 「IE5」、「IE6」

    ```
    let axo = new ActiveXObject("Microsoft.XMLHTTP");
    ```
<a id="Request"></a>
# サーバへのリクエスト
サーバへリクエストを送信するには、リクエストを初期化するための`open("HTTPメソッド","URL","同期か非同期か")`メソッドとリクエストを送信するための`sen()`メソッドを使います。

* `open()`<br>
`open()`メソッドの第1引数にはHTTPメソッドの`GET`か`POST`メソッドを、第2引数にはリクエストの送信先の`URL`を、第3引数には同期か非同期かを指定します。同期の場合に`false`を、非同期の場合の`true`を指定します。指定指定ない時に`true`となります。

    * 同期の場合(false)<br>
`false`を指定して、同期通信をした場合に、サーバ側にリクエストを送信し、応答がくるまでににブラウザはその他の処理は行えません。

    ```JavaScript:ajaxopen.js
    XMLHttpRequest.open(`GET`,`index.html`,false);
    ``` 

    * 非同期の場合(true)<br>
`true`を指定して、非同期通信をした場合に、サーバ側にリクエストを送信し、応答が返ってくるのを待たずに他の処理ができますが、応答が来たらその処理をします。

    ```JavaScript:ajaxopen.js
    XMLHttpRequest.open(`GET`,`index.html`,true);
    ```


* `send()`<br>
最終的にサーバ側にリクエストを送信するのは`sen()`メソッドです。引数にはサーバ側に送信するパラメータを指定できます。
    * `GET`の場合<br>
    `GET`の場合はパラメータがURLの後ろに付き、`send()`メソッドの引数に`null`を指定します。

    ```JavaScript:ajaxsend.js

    XMLHttpRequest.open(`GET`,`index.html?firstName=Jan,lastName=Arkin,age=27`,true);
    XMLHttpRequest.send(null);

    ```

    * `POST`の場合<br>
    `POST`の場合は`send()`メソドの引数に送信したいパラメータを指定します。パラメータは文字列の形式である必要があります。

    ```JavaScript:ajaxsend.js
    XMLHttpRequest.open(`GET`,`index.html`,true);
    XMLHttpRequest.send(`firstName=Jan,lastName=Arkin,age=27`);
    ```

* `setRequestHeader()`<br>
パラメータを`POST`する時に、パラメータのMIMEタイプを指定しなければなりません。

```JavaScript:ajaxsetrequestheader.js
XMLHttpRequest.open(`GET`,`index.html`,true);
XMLHttpRequest.setRequestHeader(`Content-Type`,`application/x-xxx-form-urlencoded`);
XMLHttpRequest.send(`firstName=Jan,lastName=Arkin,age=27`);
```
<a id="Response"></a>
# サーバからのレスポンス
サーバからのレスポンスを受け取るにはオブジェクトの`onreadystatechange`プロパティを指定します。

```JavaScript:ajaxonreadystatechange.js
XMLHttpRequest.onreadystatechange=function(){
    if(XMLHttpRequest.readyState == 4 ){
        if(XMLHttpRequest.status == 200){
            //　受け取ったレスポンスの処理
        }
    }
}
```
下記のページに移動して実際に試してみてください。<br>[https://www.w3schools.com/xml/tryit.asp?filename=tryajax_first]

* readystate<br>
`onreadystatechange`は`readyState`属性が変更されるたびに呼び出されたイベントハンドラを返す。`readystate`は0~4の番号によって定義されているので、値を監視することでデータの受信が完了したかどうかを判別できます。

|値|状態|
|:---|:---|
|0|初期化されていません|
|1|sendメソッドでデータが送信されていない|
|2|HTTPヘッダを受信しましたが、データ本体まだ受信していません|
|3|レスポンス受信中|
|4|全てのデータが受信完了しました|

* status<br>
サーバからのレスポンスを受信して、受け取ったレスポンスが正常であるかどうかという状態をstatusプロパティで表します。

|値|状態|
|:---|:---|
|200|成功|
|401|未認証|
|403|アクセス権がない|
|404|ファイルが見つからない|
|500|サーバエラー|

* ResponseTextプロパティとResponseXMLプロパティ<br>
サーバからのレスポンスがテキスト形式かXML形式かで使い分けます。

```JavaScript:ajaxonreadystatechange.js
XMLHttpRequest.onreadystatechange=function(){
    if(XMLHttpRequest.readyState == 4 ){
        if(XMLHttpRequest.status == 200){
            console.log(XMLHttpRequest.responseText);
        }else{
            // エラー表示
        }
    }
}
```

<a id="reference"></a>
# 参考URL
[1]MDN web docs,MDN web docs,(最終閲覧日：2017年7月29日)<br>[https://developer.mozilla.org/ja/docs/AJAX/Getting_Started]<br>
[2]TATSUO IKURA,Ajax Tower,(最終閲覧日：2017年7月29日)<br>[https://www.ajaxtower.jp/ini/http/index2.html]<br>
[3]WebOS Goodies,WebOS Goodies,(最終閲覧日)<br>[http://webos-goodies.jp/archives/50548720.html]<br>
[4]Yuki Mori,DIGTAL SKILL,(最終閲覧日：2017年7月29日)<br>[http://csspro.digitalskill.jp/%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB/jquery-js/ajax-%E5%85%A5%E9%96%80%E8%AC%9B%E5%BA%A7/]<br>
[5]w3schools.com,w3schools.com,(最終閲覧日：2017年7月26)<br>
[https://www.w3schools.com/xml/ajax_intro.asp]<br>
