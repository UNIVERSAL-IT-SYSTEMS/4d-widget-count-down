タイマーウィジェット
---

簡単なカウントダウンタイマーです。(v13+)

![](https://github.com/miyako/4d-widget-count-down/blob/master/images/1.png)

使い方
---
* データベースのComponentsフォルダーに```count-down.4dbase```またはエイリアスをインストールします。
* フォームにサブフォームオブジェクトを追加し，《出力サブフォーム》プロパティを外します。
* 縦横スクロールバーは消してください。リサイズオプション・境界線スタイル・フォーカスはどれでも構いません。
* サブフォームの《詳細フォーム》リストの中から```count-down (count-down)```を選択します。
* サブフォームの変数タイプを《時間》に設定します。
* 変数名は省略することができます《フォームローカル変数》。省略しない場合，C_TIMEで宣言することが必要です。
* フォームイベントは```On Data Change```以外，すべて外してください。
* オブジェクト名は必要に応じて変更してください。

* タイマーを開始する
```
CD_START ("Timer1";?00:00:30?)
```
$1: サブフォーム（ウィジェット）のオブジェクト名
$2: スタートする時間（1時間を超える場合は```59:59```からスタートします。また負の値は無視されます。）

* タイマーを停止する
```
CD_PAUSE ("Timer1")
```

* タイマーを再開する
```
CD_RESUME ("Timer1")
```

* コールバック
時間が経過する（1秒単位）ごとに```On Data Change```イベントが発生します。

たとえば，残り時間に合わせて色を変えたり，```00:00```に達したらメッセージを表示するようなことができます。

```
$thisWidget:=OBJECT Get name(Object current)

$event:=Form event

$RED:=0x00FF0000
$YELLOW:=0x00FFD400
$GREEN:=0xC23B

Case of
: ($event=On Data Change)

Case of 
: (Self->=?00:00:00?)

OBJECT SET ENABLED(*;"Start";True)
OBJECT SET ENABLED(*;"Resume";False)
OBJECT SET ENABLED(*;"Pause";False)
OBJECT SET ENABLED(*;"Stop";False)

CANCEL

: ((Self->)<?00:00:10?)

If ($RED#CD_Get_color ($thisWidget))
CD_SET_COLOR ($thisWidget;$RED)
End if 

End case 

End case 
```
