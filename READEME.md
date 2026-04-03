# WSLでC++環境構築

この資料では、Windows上でWSLを使ってC++の開発環境を構築する方法を、初心者向けに丁寧に解説します。

---

## 1. WSLのインストール

### 手順

1. Windowsで「PowerShell」を管理者として起動
2. 以下を実行：

```
wsl --install
```

### 補足

* 自動的にUbuntuもインストールされます
* 完了後、**PCを必ず再起動してください**

---

## 2. Ubuntuの初期設定（重要）

WSLを起動すると、以下の入力を求められます：

```
Enter new UNIX username:
```

 好きなユーザー名を入力（例：sosan）

```
Enter new UNIX password:
```

 パスワードを入力

⚠️ 注意：

* 入力しても**何も表示されません（正常）**
* そのままEnterを押してください

```
Retype new UNIX password:
```

 同じパスワードをもう一度入力

---

### 補足

* このユーザー名とパスワードは今後も使います
* Windowsのログインとは別です
* `sudo` コマンド使用時に必要になります

---

## 3. パッケージ更新

以下を実行：

```
sudo apt update
sudo apt upgrade
```

### 補足

* 初回は時間がかかることがあります
* 途中で「Y/n」と聞かれたら → `Y` を入力

---

## 4. C++コンパイラ（g++）のインストール

```
sudo apt install g++
```

### 確認

```
g++ --version
```

 バージョンが表示されれば成功

---

## 5. 動作確認（Hello World）

### ファイル作成

```
nano main.cpp
```

以下を入力：

```
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, World!" << endl;
    return 0;
}
```

### 保存方法

* `Ctrl + O` → Enter
* `Ctrl + X` → 終了

---

### コンパイルと実行

```
g++ main.cpp
./a.out
```

 「Hello, World!」が表示されれば成功

---

## 6. VS Code連携（おすすめ）

### 手順

1. VS Codeをインストール
2. 拡張機能「Remote - WSL」をインストール

### 起動

WSL上で：

```
code .
```

フォルダがVS Codeで開く

---

## 7. AtCoder用設定（任意）

よく使うテンプレート：

```
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define rep(i,n) for(int i=0;i<(n);i++)

int main(){
    
    return 0;
}
```

---

## 8. よくあるエラーと対処法

### g++がない

```
sudo apt install g++
```

---

### 実行できない

`./a.out` を忘れている可能性

---

### コマンドが動かない

`sudo` の後にパスワードを求められる
入力しても表示されないが正常

---

### nanoが分からない

* 保存：`Ctrl + O`
* 終了：`Ctrl + X`

---

## 9. GitHubで管理する

変更を保存するには：

```
git add .
git commit -m "update"
git push
```

---

---

## 参考資料

本資料は以下の動画を参考に作成しました：

* https://www.youtube.com/watch?v=uhnASau7fB4

---

※ 動画と本資料を併用すると理解しやすいです

