# DMM WEBCAMPコンテンツ【応用課題3】- 開発メモ他

## ①いいね機能の追加

カリキュラム手順通りに進める。

- routes.rbの記述(l5) booksにネストする。ルーティングの確認でPOSTとDELETEのみ形成確認。

- Favoriteモデルの作成。ここでは id, user_id, book_idの3つのカラムを作成する。

- db:migrate行い、関連付けをuser,bookモデルに記載。has_manyの関係。

- ここで、dependent: :destroyとは、親モデルを削除した場合に消える、という記載である。

- (削除時に一緒に消える項目名)

- 同様に関連付けをfavoriteモデルに記載。belongs_toの関係。

- favoritess_controllerの作成。createとdestroyのみアクションを作成する。

- view/books/index.html(投稿一覧)にいいねリンク追加。

  ログインユーザーがいいねしている場合→♡削除 deleteメソッド

  ログインユーザーがいいねしていない場合→♥追加 postメソッド として場合分けする。

- favorites_controller create及びdestroyアクションの内部追記

- サーバー起動。SyntaxErrorエラー発生→_index.htmlのif文end抜け。修正して再起動確認

- いいねが削除できない不具合発生→favorites_controllerの記載ミス。newメソッドではなくfind_byメソッドを使用する。

- 投稿一覧画面のいいね数・いいねボタンは作成完了。

- 投稿詳細画面においても、_index.html同様にshow.htmlの追記を行い、いいねが表示出来ることを確認した。

- 以下はどういう意味の仕様か分からないので後回しにする。

  \- いいねされていない記事に対しては、いいね作成ボタンを表示させる
  \- いいねされている記事に対しては、いいね削除ボタンを表示させる

- 不具合：投稿一覧画面においてのいいねボタンを押すと、投稿詳細画面にリダイレクトされる。

  :bulb:「直前のページにリダイレクト」記載 

  favorites_controller.rb内 7,14行目のredirect_to~文を

  **redirect_back(fallback_location: root_path)** として解決。

---

## ②コメント機能の追加

①同様に、カリキュラム手順通りに進める。

- コメント保存用のテーブルとモデルを作成する。

- モデルの関連付けを行う。

  1  user - has_many -> post_comment**s**  N

  1  book - has_many -> post_comment**s**  N　の関連性となる。

- コントローラの作成を行う。

  create及びdestroyアクションの作成。

- ルーティングの設定を行う。

  bookにネスト関係とする。						-> rails routesでPOSTとDELETEメソッドとなっていることを確認した。

- views/books/show.html.erbの編集…コメント部分の追加

- books_controller.rbの編集…showアクションへpost_commentのインスタンス変数を追記

- views/books/index.html.erbの編集…コメント数の追加

- 投稿機能の完成。また、一覧のコメント数への記載も完成している。

- コメント削除機能の実装…カリキュラム通り進めるが削除ができない。

  nilclass エラーが発生するのでIDが渡せていない状況である。

  [詳細commit](https://github.com/KKe-coder/DWC-2m-Task3/commit/359a83ab9de27c858e042feb525ff9d18d8c74e5)

  ファイルのミスは計3ファイルにある。

  - app/controllers/post_comments_controller.rbの記述

    find_byの複雑な書き方をせず、インスタンス変数@postcommentを用いる。

    :book_idの情報を渡した後、.destroyで削除を行う。

  - app/views/books/show.html.erbの記述

    既にeachメソッドで繰り返しを行ってる際、id情報は記録されている為、改めてpathに変数を渡す必要がない。2個→1個にする。

  - config/routes.rbの記述

    resourcesとしなければならないところをresourceとしていた。

    これによりidを渡すことが出来ない。

- 体裁を整え完成。