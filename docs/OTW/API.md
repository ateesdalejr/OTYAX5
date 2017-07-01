---
layout: default
title: OTYA WINDOW SYSTEM(OTW)
---
# OTYA WINDOW SYSTEM(OTW)
開発中
(OTW5.0)
ボタンもテキストボックスもみんなウィンドウ

## Control

|関数|説明|
|---|---|
|GetWindowControl()|Windowのコントロールを取得|
|NewControl NAME$ OUT CTL,ERR|新しいコントロールを作る|
|ExtendControl NAME$,PARENT OUT CTL,ERR|既存のコントロール(Windowなど)を継承|
|CheckControl(CTL)|コントロールが正常かを確認|
|DeleteControl(CTL)|コントロールを削除|
|FindControl(NAME$)|文字列を使ってコントロールを取得|
|IsControlExtend(CTL,PARENT)|CTLコントロールとPARENTコントロールが継承関係にあるかどうか|

### Handler
Handlerの書式
COMMONは付ける

`` COMMON DEF XXX WND,CTL,TYPE,A1,A2``

|関数|説明|引数1|引数2|
|---|---|---|---|
|SetControlPainter(CTL,HANDLER$)|コントロールの描画イベントを処理する関数の登録|描画範囲XY|描画範囲WH(XY=0&&WH=0のとき全体)|
|SetControlLMouseUpHandler(CTL,HANDLER$)|左クリック(ボタンから離されたとき)のイベントを処理する関数の登録|X|Y|
|SetControlLMouseDownHandler(CTL,HANDLER$)|左クリック(ボタンが押されたとき)のイベントを処理する関数の登録|X|Y|
|SetControlMouseMoveHandler(CTL,HANDLER$)|マウスが移動したときのイベントを処理する関数の登録|XY|マウスの状態|
|SetControlNotificationHandler(CTL,HANDLER$)|通知(ボタンがクリックされた、Enterが押された)など|~~そのウィンドウのVar0~~そのウィンドウのWND||
|SetControlNotifHandler(CTL,HANDLER$)|=SetControlNotificationHandler|||
|SetControlKeyHandler(CTL,HANDLER$)|キーが押されたとき|BUTTON()から特殊キーを覗いた値||
|SetControlChFocusHandler(CTL,HANDLER$)|フォーカスが変わった時|フォーカスが移ったらTRUE||
|SetControlButtonHandler(CTL,HANDLER$)|ボタンが押されたとき|||
|SetControlCreateHandler(CTL,HANDLER$)|ウィンドウが作られたとき|||
|SetControlStrNotifHandler(CTL,HANDLER$)|文字列の通知(FileDialog等)|||
|SetControlDeleteHandler(CTL,HANDLER$)|ウィンドウが削除されたとき|||
|SetControlLDoubleClickHandler(CTL,HANDLER$)|左ダブルクリックをされたとき|||
|SetControlMouseLeaveHandler(CTL,HANDLER$)|マウスが離れた時|||
|SetControlResizeHandler(CTL,HANDLER$)|ウィンドウがリサイズされたとき||WH|
|SetControlFrameHandler(CTL,HANDLER$)|ウィンドウフレーム周りのなんか|||
|SetControlFramePainter(CTL,HANDLER$)|ウィンドウフレームの描画|||
|SetControlChildWindowHandler(CTL,HANDLER$)|子ウィンドウから送られてくるイベントを受信|type|arg|

### ControlChildWindowHandler
子ウィンドウに何か起こった時に呼ばれる

#### type:WindowMaximizeEvent()
ウィンドウが最大化されようとしたときに送られる  

argに対象ウィンドウ  
rootウィンドウ

#### type:WindowMinimizevent()
ウィンドウが最小化されようとしたときに送られる  
argに対象ウィンドウ

#### type:WindowActiveEvent()
ウィンドウがアクティブになったときに贈られる
argに対象ウィンドウ

### マウスの状態
```
   10
 0b00
 bit 0CTL_LBTNFLG
 bit 1CTL_RBTNFLG
```
1なら左クリック,2なら右クリック,3なら両方
```bas
 IF BTN AND GetControlStateLBtn()THEN ~左クリック
 IF BTN AND GetControlStateRBtn()THEN ~右クリック
```

## Window

|関数|説明|
|---|---|
|GetRootWindow()|ルートウィンドウを取得|
|CheckWindow(WND)|ウィンドウが正常化を確認|
|WindowBackFlag()|NewWindowで指定するフラグ, ウィンドウを後ろに配置する|
|WindowFrontFlag()|NewWindowで指定するフラグ, ウィンドウを前に配置する|
|WindowHideFlag()|NewWindowで指定するフラグ, ウィンドウを非表示にする|
|NewWindow CTL,NAME$,X,Y,WIDTH,HEIGHT,PARENT,FLG OUT WND,ERR|コントロールと名前と座標とサイズと親ウィンドウとフラグを使ってウィンドウを作成|
|NewTopLevelWindow CTL,NAME$,WIDTH,HEIGHT OUT WND,ERR|コントロールと名前とサイズを使ってウィンドウを作成|
|NewStyleWindowArg CTL,NAME$,X,Y,WIDTH,HEIGHT,PARENT,FLG,STYLE,A1,A2|引数を使ってスタイル指定されたウィンドウ作成|
|NewStyleWindow CTL,NAME$,X,Y,WIDTH,HEIGHT,PARENT,FLG,STYLE|スタイル指定されたウィンドウ作成|
|NewTopLevelStyleWindowArg CTL,NAME$,WIDTH,HEIGHT,FLG,STYLE,A1,A2|引数を使ってスタイル指定されたトップレベルウィンドウ作成|
|NewTopLevelStyleWindow CTL,NAME$,WIDTH,HEIGHT,FLG,STYLE|スタイル指定されたトップレベルウィンドウ作成|
|WindowMenuStyle()|メニュー付きにさせるフラグ|
|WindowResizableStyle()|リサイズ可能にさせるフラグ|
|WindowHideStyle()|非表示にさせるフラグ|
|FrontWindow(WND)|ウィンドウを手前に持ってくる|
|MoveWindow(WND,X,Y)|ウィンドウを指定座標に持っていく|
|ResizeWindow(WND,W,H)|ウィンドウをリサイズ|
|MoveResizeWindow(WND,W,H)|ウィンドウを移動してリサイズ|
|SendWindowEvent(WND,TYPE,A1,A2)|ウィンドウへイベントを送信|
|RepaintWindow(WND)|ウィンドウへ再描画イベントを送信|
|RepaintFrameWindow(WND)|ウィンドウのフレーム部分への再描画イベントを送信|
|CallBaseControlHandler(WND,CTL,TYPE,A1,A2)|(イベントのハンドラーで)親ハンドラを呼び出し|
|PeekWindowEvent(WND)->OUT CTL,TYPE,A1,A2|ウィンドウのイベントキューの先頭を削除せずに帰す|
|UpdateWindow(WND)|ウィンドウのイベントを処理|
|GetWindowName$(WND)|ウィンドウの名前を取得|
|GetWindowWidth(WND)|ウィンドウの幅を取得|
|GetWindowHeight(WND)|ウィンドウの高さを取得|
|GetWinVer$()|バージョンを取得("5.0"など)|
|GetWindowX(WND)|ウィンドウのX座標を取得|
|GetWindowY(WND)|ウィンドウのY座標を取得|
|GetNextWindow(WND)|次のウィンドウ(前面)を取得,失敗したら0が返る|
|GetPrevWindow(WND)|次のウィンドウ(後面)を取得,失敗したら0が返る|
|GetParentWindow(WND)|親ウィンドウを取得|
|GetChildWindow(WND)|子ウィンドウを取得(一番後ろ)|
|GetControl(WND)|ウィンドウのコントロールを取得|
|IsFocusWindow(WND)|ウィンドウがフォーカスされていればTRUE|
|IsActiveWindow(WND)|ウィンドウがアクティブであればTRUE|
|ShowWindow(WND)|非表示ウィンドウを表示させる|
|HideWindow(WND)|ウィンドウを非表示にする(bug?)|
|SetWindowBackColor WND,RGB|ウィンドウの背景色を設定|
|SetWindowBackColor(WND)|ウィンドウの背景色を取得|
|GetBackColor()|ウィンドウのデフォルト背景色を取得|
|GetSelectionColor()|選択時の背景色を取得|
|GetSelectionTextColor()|選択時のテキスト色を取得|
|GetWindowMinSize WND OUT W,H|ウィンドウの最小サイズを取得(リサイズ用)|
|SetWindowMinSize WND,W,H|ウィンドウの最小サイズを設定(リサイズ用)|
|SetCapture(WND)|WNDに対してマウスキャプチャを開始,MouseMoveイベントが全てWNDに対して送られるようになる。但しマウスをクリックすると解除.返り値は前にキャプチャされたウィンドウ|
|GetCapture()|現在マウスキャプチャされているウィンドウを取得|
|ReleaseCapture(WND)|WNDに対してのマウスキャプチャを終了,失敗すると0、成功すると1が返る|
|SetWindowProperty WND,PNAME$,VAL OUT ERR|実装依存のプロパティ設定、OTW5.0-28では"SHADOW"を指定すると影の有無を切り替えられる|
|CalcWindowX(BASEWND,WND)|BASEWNDに対するWNDの位置を取得|
|CalcWindowY(BASEWND,WND)|BASEWNDに対するWNDの位置を取得|
|GetActiveWindow()|現在のアクティブウィンドウを取得|
|MaximizeWindow(WND)|ウィンドウを最大化|
|MinimizeWindow(WND)|ウィンドウを最小化|
|GetWindowStyle(WND)|ウィンドウスタイルを取得|
|GetWindowFrameSize WND OUT W1,H1,W2,H2,ERR|ウィンドウフレームサイズを取得|

### Graphic

|関数|説明|
|---|---|
|GBeginWindow(WND)|描画開始を明示的に宣言する|
|GEndWindow(WND)|描画終了を明示的に宣言する|
|SetWindowDrawPos WND,X,Y|描画の始点を変更(デフォルトで(0,0)|
|GPSETWindow WND,X,Y,COL|ウィンドウに点を書く|
|GFILLWindow WND,X,Y,X2,Y2,COL||
|GBOXWindow WND,X,Y,X2,Y2,COL||
|GLINEWindow WND,X,Y,X2,Y2,COL||
|GetConsolePalette(PAL)|コンソールの色を取得|
|GPRINTWindowCC WND,X,Y,STR$,PAL|コンソール色で文字を表示|
|GPRINTWindow WND,X,Y,STR$,COL||
|GPRINTBWindow WND,X,Y,STR$,COL,BC|背景色を指定してGPRINT|
|GPUTCHRWindow WND,X,Y,A,COL||
|GPUTCHRSizeWindow WND,X,Y,A,SX,SY,COL|サイズ(SX:SY)を指定してGPUTCHR|
|GPUTCHRSize1Window WND,X,Y,A,SX,COL|サイズ(S:S)を指定してGPUTCHR|
|GPUTCHRBWindow WND,X,Y,A,COL,BC|背景色を指定してGPUTCHR|
|GLOADWindow WND,X,Y,W,H,IMG[],FLG,MODE||
|GCOPYWindow WND...|廃止予定|
|GTRIWindow WND,X,Y,X2,Y2,X3,Y3,COL||
|GCIRCLEWindow WND,X,Y,R,COL||
|GCIRCLE2Window WND,X,Y,R,S,E,F,COL||
|GLOADImageWindow WND,X,Y,IMG,F|画像をウィンドウに描画|

## 標準GUI部品

|関数|説明|
|---|---|
|GetWindowControl()|ウィンドウを表示するコントロール|
|GetButtonControl()|ボタンを表示するコントロール|
|GetToggleButtonControl()|トグルボタンを表示するコントロール|
|GetTextBoxControl()|テキストボックスを表示するコントロール|
|GetLabelControl()|文字を表示するコントロール|
|SetLabelAlignCenter LABEL|文字を中央|
|SetLabelAlignLeft LABEL|文字を左寄せ(デフォルト)|
|SetLabelAlignRight LABEL|文字を右寄せ|

### Sample
```BAS
 VAR TESTOTWCTL,TESTOTWWND
 DEF I_TEST
  IF!CHKCALL("IsWinRunning")||!IsWinRunning()THEN'OTWが存在するか、存在した場合動いているか
   ExitProcess 1
   RETURN
  ENDIF
  VAR E
  ExtendControl "TEST",GetWindowControl() OUT TESTOTWCTL,E'Windowコントロールを継承する
  IF E THEN ExitProcess 1RETURN
  E=SetControlPainter(TESTOTWCTL,"TESTOTWPainter")
  NewTopLevelWindow TESTOTWCTL,"TEST",64,64 OUT TESTOTWWND,E
  IF E THEN ExitProcess 1
 END
 DEF TESTOTWPainter WND,CTL,T,A1,A2
  VAR E=CallBaseControlHandler(WND,CTL,T,A1,A2)'親のハンドラを呼び出す(これを呼ばないと枠が描画されない)
  IF E THEN RETURN
  E=GBeginWindow(WND)
  IF E THEN RETURN
  GFILLWindow WND,0,0,64,64,RGB(0,0,0)
  GPRINTWindow WND,0,0,"HELLO",RGB(255,255,255)
  E=GEndWindow(WND)
 END
 DEF L_TEST
  IF UpdateWindow(TESTOTWWND)THEN ExitProcess 1'ウィンドウが閉じられたりした
 END
```
## flag memo
- CTL_FRMBTNHANDLER
- CTL_LBTNFLG
- CTL_RBTNFLG
- CTL_BTNDWNFLG
- CTL_BTNUPFLG

文字列は"123"[0]みたいな使い方が可能

## これから実装したいもの

|関数|
|---|
|GetScreenWidth()|
|GetScreenHeight()|
|GetWinVer$()|

### message

||
|---|
|MouseLeave|
|MouseDoubleClick|

## 標準コントロール
これらのコントロールを継承する際は親コントロールのHandlerを呼び出す必要がある

### Window

|event|説明|
|---|---|
|Paint|枠を描画|
|ChFocus|前面に移動|

### Button

|event|説明|
|---|---|
|Paint|ボタンを描画|
|LMouseUp|親ウィンドウにNotifを送信|

#### 操作

|関数|説明|
|---|---|
|SetButtonAlignLeft WND|ボタンの文字を左寄りにする|
|SetButtonAlignRight WND|ボタンの文字を右寄りにする|
|SetButtonAlignCenter WND|ボタンの文字を中央に配置する|
|IsCheckedButton(WND)|トグルボタンをチェックされているか|
|UnCheckButton WND|トグルボタンをチェックさせない|
|CheckButton WND|トグルボタンがチェックさせる|

### Label

### Scroll

|関数|説明|
|---|---|
|GetVScrollBarControl()|縦スクロールバーコントロールを取得|
|NewVScrollBar PARENT,SIZ OUT WND,E|縦スクロールバーをPARENTに長さSIZで作成|
|SetScrollBarSize WND,SIZ|縦スクロールバーのサイズを設定|
|GetScrollBarSize(WND)|縦スクロールバーのサイズを取得|
|SetScrollBarPosition WND,POS|縦スクロールバーの位置を設定|
|GetScrollBarPosition(WND)|縦スクロールバーの位置を取得|

### ListBox

|関数|説明|
|---|---|
|GetListBoxControl()|リストボックスのコントロールを取得|
|AddListBoxItem WND,ITEM$|リストボックスにITEM$を追加|
|AddArrayListBoxItem WND,ITEM$|リストボックスに配列ITEM$を追加|
|ListBoxChItem()|選択アイテムが変化すると親ウィンドウにNotif(A1=WND,A2=ListBoxChItem)を送る|
|GetListBoxSelectedText$(WND)|リストボックスで選択されているアイテム名を取得|
|SetChItemListBoxNotif WND,F|選択アイテムが変化すると親ウィンドウにNotif(A1=WND,A2=ListBoxChItem)を送るかどうか|
|ClearListBox WND|リストボックスの項目を初期化|

### NumUpDown

||
|---|
|GetNumUpDowCnontrol()|NumUpDownのコントロールを取得|
|GetNumUpDownValue(WND)|NumUpDownの値を取得|
|SetNumUpDownRange WND,MIN,MAX|NumUpDownの範囲を設定|

### DropDownList (DROPDOWNLIST)

#### GetDropDownListControl()
DropDownListのコントロールを取得

#### GetDropDownListBox(WND)
DropDownListのListBox WNDを取得(これに対して項目を追加する)

## Menu

|関数|説明|
|---|---|
|NewMenu OUT MENU,E|MENUを作成|
|SetMenuBar WND,MENU|未実装|
|ShowMenu MENU,WND|未実装,引数の順番が定まっていない|
|GetWindowMenu(WND)|WNDのMENUを取得|
|AddMenuItem MENU,STR$,IVAR|MENUにSTR$を追加,IVARはWindowNotifEventの時にARG2に指定される|
|AddMenuItemSeparator MENU|MENUにSeparatorを追加|
|CheckMenu(MENU)|MENUが存在すればTRUE|
|NewTopLevelMenuWindow CTL,NAME$,WIDTH,HEIGHT OUT WND,ERR||
|ShowContextMenu MENU,WND|コンテキストメニューを表示|
|AddSubMenuItem MENU,STR$,SUBMENU|メニューにサブメニューを追加|

## Window Group

|関数|説明|
|---|---|
|JoinWindowGroup(WND,WND2)||
|LeaveWindowGroup WND|未実装|
|GetWindowGroupOwner(WND)||

## Dialog

|関数|説明|
|---|---|
|NewDialogBox(CTL,NAME$,WIDTH,HEIGHT,OWNER,FLAG)||
|NewModalDialogBox(CTL,NAME$,WIDTH,HEIGHT,OWNER)|モーダルダイアログボックスを作成|
|NewModelessDialogBox(CTL,NAME$,WIDTH,HEIGHT,OWNER)|モーダレスダイアログボックスを作成|

## File dialog

### OpenFileDialog(OWNER,TYPE$,ID)
ファイルを開くダイアログを表示  
TYPE$にファイル種別(TXT/DAT)  
選択された場合IDをA1、ファイル名をA2$に入れてStrNotifが呼ばれる

### SaveFileDialog(OWNER,TYPE$,ID)
ファイル保存ダイアログを表示  
TYPE$にファイル種別(TXT/DAT)  
選択された場合IDをA1、ファイル名をA2$に入れてStrNotifが呼ばれる

## WindowOP
ウィンドウに対しての操作を効率化する
子ウィンドウを一々削除していたら再描画リクエストが一々確認されたりして非常に遅い
それをEndWindowOPでまとめてやる

|関数|説明|
|---|---|
|BeginWindowOP(WND)||
|EndWindowOP(WND)||
|MoveWindow2(WND,X,Y)|->MoveWindowOP(WND,X,Y)|

## 拡張コントロール群
標準コントロールの機能拡張版

### TextBoxEx
複数行編集、シンタックスハイライトに対応した拡張版

|関数|説明|
|---|---|
|GetTextBoxExControl()|TextBoxExControlを取得|
|TextBoxExSetText WND,TXT$|WNDにTXT$を設定|
|TextBoxGetText  WND OUT TXT$|WNDのTextを取得|
|SetTextBoxExPRGMode WND,FLG|FLGがTRUEならばシンタックスハイライトを有効化|
|TextBoxExSetSelectedText WND,TXT$|現在選択されているTextにTXT$を設定|
|TextBoxExGetSelectedText WND OUT TXT$|現在選択されているTextを取得|
|TextBoxExCopy WND|クリップボードにコピー|
|TextBoxExCut WND|クリップボードに切り取り|
|TextBoxExPaste WND|クリップボードから貼り付け|

### RICHTEXTEDITOR

|関数|説明|
|---|---|
|RichTextBold()|フラグ|
|RichTextItalic()|フラグ|
|RichTextStrike()|フラグ|
|RichTextUnderline()|フラグ|
|RICHTEXT X,Y,C,STYLE,SIZE,COL|RICHTEXTを表示|

|関数|説明|
|---|---|
|RTESetBold WND,F||
|RTESetItalic WND,F||
|RTESetStrike WND,F||
|RTESetUnderline WND,F||
|RTESetTextColor WND,COL||
|RTESetAlignLeft WND||
|RTESetAlignCenter WND||
|RTESetAlignRight WND||
|RTESetFontSize WND||

#### RTENew RTE
初期化

#### RTEOpen RTE,FILE$ OUT ERR
FILE$を開く

#### RTESave RTE,FILE$ OUT ERR
FILE$に保存する

#### RTEClear WND
インデントや見出しなどを消す

#### RTEIndent WND,INDENT
インデントをINDENT px増やす

#### RTESetOrderedList WND
* こういうリスト

#### RTESetUnorderedList WND
1. こういうリスト

#### RTESetHeading WND,LEVEL
LEVEL(1<=LEVEL<=6)見出しを付ける

#### RTECopy WND
コピー

#### RTEPaste WND
ペースト

#### RTECut WND
切り取り

#### RTESelectAll WND
すべて選択

#### RTEAddTable WND,ROW,COL
ROW行COL列の表を作成

## ダイアログ

|関数|説明|
|---|---|
|SaveFileDialog(OWNER,TYPE$,ID)|今の所TYPEはTXTまたはDATのみ|
|OpenFileDialog(OWNER,TYPE$,ID)|今の所TYPEはTXTまたはDATのみ|
|MessageBox(WND,TITLE$,TEXT$,FLAG)|メッセージボックスを作成|
|MessageBoxOK()|OKボタンのフラグ|
|MessageBoxNotifOK()|OKボタンが押されたときにWNDへ送信される|
|MessageBoxNotifCancel()|キャンセルされたときにWNDへ送信される|
|MessageBoxNotifID()|MessageBoxが閉じられたときにWNDへ送信されるNotifID|

## Clipboard

|関数|説明|
|---|---|
|ClearClipboard|クリップボードを初期化|
|ClipboardContainsText()|クリップボードに文字列が格納されているか|
|ClipboardGetText$()|クリップボードに格納された文字列を取得(無ければ空文字)|
|ClipboardSetText V$|クリップボードに文字列を格納|
|ClipboardContaisFile()|クリップボードにファイルが格納されているか|
|ClipboardGetFile OUT ISCUT,PATH$)|クリップボードに格納されたファイルを取得(無ければ空文字)ISCUTがTRUEならば切り取り|
|ClipboardSetFile ISCUT,PATH$|クリップボードにファイルを格納、ISCUTがTRUEならば切り取り|
|ClipboardSetData$ TYPE$,V$|クリップボードにTYPE$の文字列データを設定|
|ClipboardGetData$ TYPE$ OUT DATA$,CONTAINS|クリップボードからTYPE$の文字列データを取得、CONTAINSがFALSEなら含まれていない|

### ClipboardData
### "RichText"
リッチテキスト文字列が入っている

## Image

|関数|説明|
|---|---|
|NewImage ARRAY,WIDTH,HEIGHT OUT IMG,E|画像を作成|
|LoadImage FILE$,W,H OUT IMG,E|画像を二次元配列DATファイルから読み込み|
|GLOADImage X,Y,IMG,F|現在のグラフィック面にX,YにIMGを描画|
|CheckImage(IMG)|画像が正常か確認|
|DeleteImage(IMG)|画像を削除|

## 関連付け

|関数|説明|
|---|---|
|GetAssociatedProgram$(TYP$,EXT$)|TYP$とEXT$に関連付けられたものを取得|
|AssociateFile(TYP$,EXT$,NAME$)|拡張子をNAME$に関連付けるTYP$に" "/"*"/"/",EXT$に拡張子(e.g.TXT)成功するとFALSE|
|ExecFile(PATH$)|PATH$に関連付けられたプログラムをPATH$を引数に設定して起動|

## 直接描画

|関数|説明|
|---|---|
|GBeginDirect(WND)|直接描画を可能にする(GPSETなどが使える)|
|GCopyDirect(WND,X,Y,W,H,X3,Y3,MODE)|直接描画を終了し、GBeginWindow(WND)をして転送|

## フォント
フォント周りのAPI

### CalcTextSize
`CalcTextSize TEXT$ OUT W,H`
デフォルトフォントでのテキストのサイズを計算

## IM
[こちらを参照](https://github.com/otya128/OTYAX5/wiki/OTW-IM)