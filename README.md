# ニューラルネットワークを使った手書き文字認識：初級編

## このレポジトリについて
セラクの1Dayインターンで行うハンズオンのためのレポジトリ。  
内容はシンプルな **ニューラルネットワーク(NNW)** で行う **手書き数字認識問題(MNIST)** 。

### SimpleClassificationModel
認識モデル作成用のJupyter Notebook。

### ImageFileClassifier
作成したモデルを使用するためのJupyter Notebook。

## やること
NNWの基礎を体験するために、手書き数字認識モデルを構築する。

### ニューラルネットワークとは
機械学習モデルの一つ。  
ディープラーニングの基礎となる技術。  
脳の神経回路を模した非線形モデル。  
詳しくは[こちら](https://ja.wikipedia.org/wiki/%E3%83%8B%E3%83%A5%E3%83%BC%E3%83%A9%E3%83%AB%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF "Wikipedia:ニューラルネットワーク")。  
数多くのNNWライブラリが存在するが、今回は[Keras](https://keras.io/ja/ "Keras公式HP")を使用する。

### MNISTとは
手書き数字認識データセット。  
画像認識の入門問題であり、性能評価のベンチマークに使われる。  
訓練データ60000サンプル、評価データ10000サンプルに分かれている。  
入力データは28x28ピクセルの256階調のグレースケール画像。  
教師データは、画像が表す数字1文字(0~9)のラベル。  
*データの一例*  
![MNIST](https://en.wikipedia.org/wiki/File:MnistExamples.png "MNISTサンプル")


## 流れ

### 前処理
元のデータをモデルに適した形へと加工する。  
* **1hotベクトル** 化  
多クラス分類問題は、各クラスへの確率分布という形で予測を行うのが一般的である。  
そのため、元々の教師データを各数字の要素のみが1でそれ以外が0であるようなベクトルに変換する。  
*例)教師データが5である場合、以下の形に変換する。*  
*0から数えて5にあたる要素のみが1となっている。*  
`[0, 0, 0, 0, 0, 1, 0, 0, 0, 0]`  

* **正規化**  
データの分布を揃える。  
具体的には、今回は[-1, +1]の範囲に収まるように縮尺を変える。

### モデル設計
学習させるNNWモデルの構造を設計する。  
今回は中間層１層のミニマルな構成とする。  
出力層は **ソフトマックス関数** を通すことにより、出力ベクトルに確率分布的な意味を持たせる。  

### 訓練(チューニング)
モデルに訓練データを与えて、最適な形へと学習させる。  
具体的には、正解に対する*外れ具合*(損失関数)が小さなる方向へ、パラメータを調整する。  
パラメータ調整アルゴリズムは種々あるが、今回は最もシンプルな **確率的勾配降下法(SGD)** を用いる。  
本来は訓練の結果を見て、前処理方法やモデル設計を見直す工程を繰り返していく。

### 評価
完成した学習済みモデルの性能を評価データで評価する。  
最終評価以前に評価データを用いてはいけない。カンニング。  
今回は評価指標として **分類精度** と、 **混同行列** を見る。  
* 分類精度  
モデルの予測結果の何割が正解していたか、単純な正解率。  

* 混同行列  
正解に対してモデルがどう予測したのかの詳細な結果。  
どの数字がどのように間違えられやすいのか、などの知見が得られる。  

### 使用
今回作成したモデルを実際に使って、手書き数字画像ファイルの認識を行う。  


## 動作環境
このレポジトリは `requirements.txt` のPython環境があれば(おそらく)動作する。  
しかし、手元にPython環境がなくても、Googleアカウントがあればブラウザ上で実行可能。  
その際、 **Google Colaboratory** というクラウドサービスを以下の手順で利用する。

### Google Colaboratoryでの実行方法
1. ブラウザから[GoogleDrive](https://drive.google.com)を開く
2. 「 **新規** 」→「 **その他** 」から、「 **Colaboratory** 」を開く
3. 「 **ファイル** 」から「 **ノートブックを開く...** 」を選択
4. 「 **GitHub** 」のタブを選択肢
5. 検索窓からこのレポジトリのURL(https://github.com/ShinyaKANAI/NNW_MNIST.git )を入力
6. 「 **SimpleClassificationModel.ipynb** 」を開き、実行する
