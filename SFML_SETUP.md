# WSLでSFMLを動かす方法

この資料では、WSL上でSFMLを用いたC++プログラムを動かす方法を説明する。
ウィンドウ表示を行うプログラムや、簡単なビジュアライザを作りたい場合に便利である。

---

## 1. SFMLとは

SFMLは、C++で以下のような機能を扱いやすくするライブラリである。

* ウィンドウ表示
* 図形の描画
* 文字表示
* キーボード入力
* 音声再生

競技プログラミングにはあまり使わないが、ビジュアライザや簡単なゲーム、GUI付きの確認用プログラムを作るときに便利である。

---

## 2. SFMLのインストール

以下を実行する。

```bash
sudo apt update
sudo apt install libsfml-dev
```

---

## 3. 動作確認用プログラム

まず、以下のファイルを作成する。

```bash
nano test.cpp
```

中身は以下のようにする。

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "SFML Test");

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) window.close();
        }

        window.clear();
        window.display();
    }

    return 0;
}
```

---

## 4. コンパイル方法

以下のようにコンパイルする。

```bash
g++ test.cpp -o test -lsfml-graphics -lsfml-window -lsfml-system
```

実行は以下の通り。

```bash
./test
```

ウィンドウが表示されれば成功である。

---

## 5. WSLで使うときの注意点

WSLでは、CUIのプログラムと違って、GUI表示が関わるため環境によってはそのままでは動かないことがある。

主な原因は以下である。

* GUI表示の設定ができていない
* Windows側との連携がうまくいっていない
* WSLの種類やバージョンによって動作が異なる

Windows 11 のWSLg環境では、そのままウィンドウが表示されることが多い。
一方で、環境によっては追加設定が必要な場合がある。

---

## 6. フォントについて

SFMLで文字を表示する場合、フォントファイルが必要になる。

例えば以下のように使う。

```cpp
sf::Font font;
if (!font.loadFromFile("arial.ttf")) {
    return 1;
}
```

このとき、`arial.ttf` のようなフォントファイルが実行ファイルから見える場所に必要である。

### 注意点

* フォントファイルが存在しないと文字は表示できない
* パスが間違っていると読み込みに失敗する

### おすすめの管理方法

例えば以下のように `fonts` フォルダを作ると整理しやすい。

```text
project/
├── main.cpp
└── fonts/
    └── arial.ttf
```

この場合は以下のように読み込む。

```cpp
font.loadFromFile("fonts/arial.ttf");
```

---

## 7. ビジュアライザについて

SFMLはビジュアライザ作成にも向いている。
例えば以下のような用途に使える。

* 配列のソートの様子を表示する
* グラフ探索の進み方を表示する
* マップ上でオブジェクトが移動する様子を表示する
* ヒューリスティック問題の盤面確認を行う

ビジュアライザを作ると、プログラムの動作確認がしやすくなり、バグの発見にも役立つ。

### 作るときの基本方針

* 状態を表すデータ構造を用意する
* 1フレームごとに画面を更新する
* 必要に応じてキーボードやマウス入力を受け取る
* 描画とロジックを分ける

---

## 8. よくあるエラー

### ヘッダが見つからない

```text
fatal error: SFML/Graphics.hpp: No such file or directory
```

`libsfml-dev` がインストールされていない可能性がある。

```bash
sudo apt install libsfml-dev
```

---

### リンクエラーが出る

```text
undefined reference to ...
```

コンパイル時に `-lsfml-graphics -lsfml-window -lsfml-system` を付け忘れている可能性がある。

---

### フォントが表示されない

* フォントファイルが存在しない
* パスが間違っている
* 読み込み失敗を確認していない

---

## 9. まとめ

WSL上でも、環境が整っていればSFMLを用いたC++プログラムを動かすことができる。
特に、ビジュアライザや簡単なGUI確認ツールを作るときに有用である。
ただし、文字表示にはフォントファイルが必要であるため、コードだけでなく実行時に必要なファイルも一緒に管理することが重要である。

---

## 10. Makefileの導入（おすすめ）

SFMLのプログラムをコンパイルするとき、毎回以下のような長いコマンドを書く必要があります。

```bash
g++ test.cpp -o test -lsfml-graphics -lsfml-window -lsfml-system
```

これを簡単にするために、Makefileを使用します。

---

### Makefileとは

Makefileとは、コンパイル手順を自動化するための設定ファイルです。
一度作れば、`make`コマンドだけでビルドできます。

---

### Makefileの作成

```bash
nano Makefile
```

以下を記述します。

```makefile
CXX = g++
CXXFLAGS = -std=c++17
LDFLAGS = -lsfml-graphics -lsfml-window -lsfml-system

TARGET = main
SRC = main.cpp

all:
	$(CXX) $(SRC) -o $(TARGET) $(CXXFLAGS) $(LDFLAGS)

run: all
	./$(TARGET)

clean:
	rm -f $(TARGET)
```

※ `all:` や `run:` の次の行は**Tabキー**でインデントする必要があります（スペース不可）

---

### 使い方

#### コンパイル

```bash
make
```

#### 実行

```bash
make run
```

#### 削除

```bash
make clean
```

---

### メリット

* コマンドを毎回書かなくてよい
* ミスが減る
* ファイルが増えても管理しやすい

---

### よくあるミス

#### スペースとTabの違い

Makefileでは、インデントに**Tab**を使う必要があります。

```text
× スペース → エラー
○ Tab → 正しい
```

---

#### ファイル名が違う

`main.cpp` 以外を使う場合は、以下を変更する必要があります。

```makefile
SRC = your_file.cpp
TARGET = your_program
```

---

## 11. まとめ（発展）

Makefileを使うことで、SFMLのような複雑なコンパイルも簡単に管理できる。
特にビジュアライザのようにファイル数が増える場合に有効である。
