# kintoneCafe_kochi_Hands-on

## サンプルコード
```
//データを登録するkintoneの選択
function sendToKintone(e) {
    'use strict';
    Logger.log('Form submitted');
  
  　//サブドメインを入力(~.cybozu.com)
    var subdomain = "devcumafy.cybozu.com";
  
  　//データを登録するアプリの選択
    var apps = {
        YOUR_APPLICATION1: { appid: 37, name: "kintoneCafe-Vol.13参加アンケート", token: "mPUnXhCSr47vQJPN5Zi0JE9j7r3a58LtHYX9J1Sy" }
    };
  
  　// ライブラリーの初期化
    var manager = new KintoneManager.KintoneManager(subdomain, apps);
    var str = getFormResponse(e);
  　
  　//JSON形式に変換
    str = str.replace(/\n/g, "\\n").replace(/\r/g, "\\r").replace(/\t/g, "\\t");
    var records = JSON.parse(str);
  
  　//kintone レコードの生成
    var response = manager.create("YOUR_APPLICATION1", records);
  
    // ステータスコード
    // 成功すれば200になる
    var code = response.getResponseCode();
    Logger.log('Response code is "%s"', code);
}

function getFormResponse(e) {
    'use strict';
  
  　//アンケートの回答を取得
    var itemResponses = e.response.getItemResponses();
  
  　//取得した回答を配列化
    var records = '[';
  　
  　//回答者のEmailアドレスの取得
    records += Utilities.formatString('{"mail": { "value": "%s" }', e.response.getRespondentEmail());
  
    //質問の数だけ処理を繰り返す。
    for (var i = 0; i < itemResponses.length; i++) {
        var itemResponse = itemResponses[i];
      
      　//質問ごとに処理を分ける(条件分岐)
        switch (itemResponse.getItem().getTitle()) {
            
            //質問1に対する回答を取得
            case "参加しますか？":
                records += Utilities.formatString(',"question1" : { "value": "%s" }',
                    itemResponse.getResponse());
                break;
            
            //質問2に対する回答を取得
            case "参加者氏名":
                records += Utilities.formatString(',"question2" : { "value": "%s" }',
                    itemResponse.getResponse());
                break;
            
            //質問3に対する回答を取得
            case "参加経緯":
                records += Utilities.formatString(',"question3" : { "value": "%s" }',
                    itemResponse.getResponse());
                break;
        }
    }
    records += '}]';
  
    //取得した値の確認(デバックコード)
    Logger.log('Response JSON is "%s"', records);
  
    return records;
}
```
