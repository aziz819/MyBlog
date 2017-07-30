みなさんこんにちは、新卒社員のJANです。今回はAjaxの基礎知識について説明します。


# 目次

1.[Ajaxとは](#Ajax)<br>
2.[XMLHttpRequestオブジェクトの生成](#XHR)<br>
3.[サーバへのリクエスト](#Request)<br>
4.[サーバからのレスポンス](#Response)<br>
5.[参考URL](#reference)<br>

<a id="Ajax"></a>
# Ajaxとは
 Ajax(Asynchronous JavaScript And Xml)Webページ全体のリロードを伴わずに非同期でサーバとの間でデータのやり取りを`JavaScript`、`XML`、`DOM`、`XMLHttpRequest`という複数の技術の組み合わせで通信を行うWEBアプリケーション開発に必要に不可欠な手法の一つです。Ajaxと取り入れるとバックグラウンドでサーバと非同期通信することで、ページ内で必要な部分だけが更新でき、ユーザーにもストレス感じさせずに、快適に操作することができます。

<a id="XHR"></a>
 # XMLHttpRequestオブジェクトの生成
AjaxはほとんどJavaScriptで構成されていて、サーバにHTTPリクエストを送るための機能を提供しているXMLHttpRequestオブジェクトを生成することにで非同期通信を行い、サーバ側からのファイルの内容が取得できます。データ形式としてXMLとJSONが使えます。
XMLHttpRequestオブジェクトを生成して、`open()`メソッドの引数にHTTPメソッドとURLと同期か非同期かの指定で簡単にサーバにリウエストを送信できます。

* XMLHttpRequest<br>
`XMLHttpRequest`はMicrosoftによって設計され、有名なブラウザーのGoogle,Mozilla,FireFox,Operaで使われています。
`XMLHttpRequst`のオブジェクトを生成するには、ブラウザ毎に実装方法が異なります。
chrome,firefox,safari,opera,IE7+の場合は`XMLHttpRequest`オブジェクトを生成して使いますが、サポートされていない古いIE5,IE6などのブラウザでは`ActiveXObject`オブジェクトを生成しなければなりません。

    * chrome,firefox,safari,opera,IE7+　などの場合

    ```
    let xhr = new XMLHttpRequest();
    ```
    
    * IE5,IE6 などの場合

    ```
    let axo = new ActiveXObject("Microsoft.XMLHTTP");
    ```
<a id="Request"></a>
# サーバへのリクエスト
サーバへリクエストを送信するには、`open("HTTPメソッド","URL","同期か非同期か")`メソッドを使います。

* `open()`<br>
`open()`メソッドの第1引数にはHTTPメソッドの`GET`か`POST`メソッドを、第2引数にはリクエストの送信先の`URL`を、第3引数には同期か非同期かを指定します。同期の場合に`true`を、非同期の場合の`false`を指定します。指定指定ない時に`true`となります。

    * 同期の場合(false)<br>
`false`を指定して、同期通信をした場合に、サーバ側にリクエストを送信し、応答がくるまでににブラウザはその他の処理は行えません。


    * 非同期の場合(true)<br>
`true`を指定して、非同期通信をした場合に、サーバ側にリクエストを送信し、応答が返ってくるのを待たずに他の処理をしますが、応答が来たら層の処理をします。

```JavaScript:ajaxxhr.js
XMLHttpRequest.open("GET","index.html",true);
```


* `send()`<br>
実際にサーバ側にリクエストを送信するには`sen()`メソッドを使います。引数にはサーバ側に送信するパラメータを指定します。
    * `GET`の場合<br>
    `GET`の場合はパラメータがURLの後ろに付き、`send()`メソッドの引数に`null`を指定します。

    ```JavaScript:ajaxxhr.js

    XMLHttpRequest.open("GET","index.html?firstName=Jan,lastName=Arkin,age=27",true);
    XMLHttpRequest.send(null);

    ```

    * `POST`の場合<br>
    `POST`の場合は`send()`メソドの引数に送信したいパラメータが指定でき、パラメータは文字列の形式である必要があります。

    ```
    XMLHttpRequest.open("GET","index.html",true);
    XMLHttpRequest.send("firstName=Jan,lastName=Arkin,age=27");
    ```
