# Git のブランチ戦略練習用リポジトリです

## Git リポジトリの作成と接続
- Git Hub にて、新規のリポジトリを作成する
- ローカルに練習用のディレクトリを作成する
- リモートリポジトリへの接続
    - 練習用ディレクトリで ```git init``` コマンドを実行する
    - 適当なファイルを作成する（ここでは、sample.txt）
    - ```git add sample.txt``` で sample.txt をステージする
    - ```git commit -m "first commit"``` でコミットする
    - ```git branch -M "master"``` で master ブランチを作成する
    - ```git remote add origin {リモートリポジトリの URL}``` で接続先となるリモートリポジトリを設定する
    - ```git push origin master``` でローカルリポジトリ内容でリモートリポジトリを同期する

## develop ブランチの作成
- ```git checkout -b develop``` コマンドを実行し develop ブランチをローカルリポジトリに作成する
- ```git push origin develop``` でローカルリポジトリの develop ブランチの内容をリモートリポジトリに同期する

## 作業用ブランチを作成する
ここでは、練習のため複数の作業用ブランチを作成し、並行で作業をしていくことにする

- １つ目の作業用ブランチを作成する
    - ```git branch``` を実行し、develop ブランチにいることを確認する
    - develop ブランチ以外にいる場合、```git checkout develop``` コマンドを実行し、develop ブランチに切り替える
    - ```git checkout -b feature/add_sample2``` コマンドを実行し、作業用ブランチを作成する
- 新規ファイルを作成する
    - 練習用ディレクトリ直下に sample2.txt ファイルを作成する
    - リモートリポジトリに feature/add_sample2 の内容を push する
- Git Hub 上で、develop ブランチに向けてプルリクエストを作成する
    - チーム開発の場合、レビュワーを指定し、レビューが通ればマージされる
    - プルリクエストを承認し、develop へマージする
- マージ済の作業用ブランチは削除する

## リリース用のブランチを作成する
- ```git checkout develop``` で develop ブランチに切り替える
- ```git checkout -b release/add_sample2``` コマンドを実行し、リリース用のブランチを作成する
- このブランチはリリースモジュールに対する修正やドキュメントの変更のみ加えることができる
- ```git push origin release/add_sample2``` でリリースブランチを push する
- master ブランチへ向けてプルリクエストを作成する
- マージは squash and merge で実行する
- リリースブランチで、不具合修正等の変更があった場合、develop にも sqush and merge で変更を戻す

## チーム開発でのブランチ戦略
- develop ブランチから feature/add_sample3 ブランチを作成する
    - sample3.txt を作成しコミットする
    - sample3.txt の内容を変更しコミットする
- develop ブランチから feature/modify_sample2 ブランチを作成する
    - sample2.txt の内容を変更しコミットする
    - feature/modify_sample2 ブランチを push する
    - develop ブランチに向けてマージリクエストを作成する
    - non-fasfoward でマージする
- feature/add_sample3 に別の作業ブランチの変更を取り込む
    - develop ブランチに移動する
    - develop ブランチを pull する
    - feature/add_sample3 ブランチに移動する
    - sample2.txt に変更を加える
    - feature/add_sample3 に変更をコミットする
    - feature/add_sample3 を push し、プルリクエストを作成する
    - マージのチェックでコンフリクトが発生する

## コンフリクトの解消
- ```git rebase develop``` コマンドを実行し、develop の変更履歴を取り込む
- コンフリクトした変更点を両方とも取り込む
- ```git rebase --continue``` でリベースを続行する
    - リベースするとコミット履歴が feature/modify_sample2 の後ろに続く形でマージされる
- feature/add_sample3 ブランチを push する
    - ```git push -f origin feature/add_sample3``` コマンドで push する
    - rebase するとコミット ID が変わるため、-f オプションをつけて強制的に push をする必要がある
    - -f オプション付きの push は変更点を上書きするため、作業ブランチ以外では絶対に使用しない
- コンフリクトが解消できたら、non-fastfoward でマージする

## 変更のリリース
- develop ブランチに移動し pull をする
- release/modify_sample2_add_sample3 ブランチを作成し push する
- release/modify_sample2_add_sample3 に変更を加える
    - fix/add_sample2 ブランチを作成し、sample3.txt を修正する
    - 変更を push し、リリースブランチへプルリクエストを作成する
    - squash and merge でリリースブランチへマージする
- release ブランチの変更を develop へ squash and merge でマージする
- release ブランチの変更を master へ squash and merge でマージする