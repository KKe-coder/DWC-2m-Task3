# デバッグ履歴

---

1. rails s 実行時 **SyntaxError** 発生し起動不可。

   routes.rbの7行目にend追記する。 				-> 起動可能。

   - [x] 解決チェック



2. Preview Running Application 使用時 **PendingMigurationError** 発生し起動不可。

   コンソールでrails db:migrate実行する。		->成功。再度Preview Running Application使用。

   - [x] 解決チェック



3. rails s 実行時 **SyntaxError** 発生時起動不可。

   users_controller.rbの12行目にend追記する。->起動可能。Preview Running Application使用成功。

   - [x] 解決チェック



4. 開いた画面でリダイレクトが繰り返し行われる。Parameters: {"id"=>"sign_in"}となっている。

   routes.rbのdevise_forの行を4行目から2行目に変更した。

   ->リダイレクトは繰り返し行われないが、5.のエラー発生。保留。5のエラー解決により解決。

   - [x] 解決チェック



5. **Template::Error (File to import not found or unreadable: bootstrap.)**発生。

   gemfileの中にbootstrapの記述が存在しない。

   Gemfileの67行目にgem 'bootstrap', '~> 4.5'を追記、

   コンソールでbundle install実行後起動。

   -> 起動完了。Preview Running Application使用成功。

   - [x] 解決チェック



6. ヘッダーとフッターのみしか表示されていない？

   devise/sessionsの中にはnewしか無いが…？

   ヘッダー「Bookers」「Home」「About」「login」ボタンは何も表示されない。

   top.html.erbには記載があるのでエラーなしの不具合。

   ページ遷移は行われる。「sign up」を押下した時にエラー表示。7に記載。

   application.html.erbにyield記述が存在しない。15行目に追記することで解決。

   - [x] 解決チェック



7. **NoMethodError in Devise::Registrations#new** 発生。

   dbディレクトリ内のユーザテーブル記載事項にはt.string :name記載あり。

   devise/registrations/new.html/erb確認。

   9行目 name入力欄フィールドがf.name_fieldとなっているのでf.text_fieldに修正する。

   再度起動。「sign up」押下時にもエラーが発生しないことを確認した。

   - [x] 解決チェック



8. Bookers 未ログイン時には√(top画面)が表示出来ない。

   会員登録が正常に行われた後は、表示が行えることからユーザー認証で自動的に弾かれているものだと思われる。

   controllers/application.controller.rbの2行目 user!後に除外設定,expectがない。

   追記,except: [:top, :about]

   - [x] 解決チェック



9. 新規会員登録が出来ない。入力してもBooks must existと出る。

   model/user.rb 7行目 **belongs_to** :booksとなってしまっている。belongs_to -> has_manyに修正。

   修正後、10.エラー発生。ページ遷移は発生している。

   - [x] 解決チェック



10. **NameError in Homes#top**発生。ルーティング参照し、

    homes/top.html.rbの10行目 ~~ new_user_session**s**_path -> new_user_session_pathに修正。

    同様に13行目 ~~ new_user_registration**s**_path -> new_user_registration_pathに修正。

    修正後、会員登録が正常に出来ることを確認。

    - [x] 解決チェック

11. -会員登録直後、Top(√)ページに遷移するようになっている。

    controllers/application_controller.rbの7行目

    after_sign_in_path_forの中身がroot_pathとなってしまったことによる。

    ルーティングを確認し、user_path(current_user.id)に修正した。

    - [x] 解決チェック



12. ログインした状態で、ヘッダー「Users」押下時エラー発生。

    **ActionView::MissingTemplate in Users#index**

    --部分テンプレートを使用する際には、view以下同ディレクトリにある場合は名前のみでいいが、view以下別ディレクトリにある場合は上位ディレクトリ名まで記述しなければならない。

    views/users/index.html.erb 4行目 'form' -> 'books/form' に修正。

    正常にusersページに移動できることを確認した。

    - [x] 解決チェック



13. ログインした状態で、Usersページに移動した際、他のページとの体裁が異なる。

    (左側にユーザー詳細、右側に項目が表示されるところが1行になる)

    views/users/index.html.erbの記述をviews/books/index.html.erbと同様にして解決。

    - [x] 解決チェック



14. ログインした状態で、ユーザー編集した際に、ユーザー名の変更の保存がされない。

    また、ラベル名がtitleになってしまっている。

    viws/users/edit.html.erbの8,9行目 :title -> :name に変更。

    - [x] 解決チェック

15. プロフィール画像が正常に表示されない。**メンターへ質問**

    model/book.rb内の記述ミス -> belongs_toとするところをhas_manyとしていた。

    また、各場所のattachment_image_tag内サイズ記述を [w],[h],:fillよりsize指定に変更。

    修正し解決。

    - [x] 解決チェック



16. bookが投稿できない -> エラーメッセージ表示

    **ActionController::ParameterMissing in BooksController#create**

    BooksController ストロングパラメータ内にbodyの記載がない。

    46行目.permit(:title) -> .permit(:title, :body)に修正。

    空白のときはbooksにrenderされることを確認。

    空白でないときはエラーが発生した -> エラー17

    - [x] 解決チェック



17. bookが内部入力値があるとき投稿できない -> エラーメッセージ表示

    **NameError in Books#show**

    エラー15により引き起こされたもの。

    - [x] 解決チェック



18. Users画面:投稿の際空白で投稿できないが、エラーメッセージが表示されない。

    books_controllerを追記。.newメソッドを2回使うことで修正完了。

    - [x] 解決チェック



19. Books_show画面:books詳細画面から投稿しようとすると、表示本の編集フォームとなってしまう。

    books#showに@booknewという空のインスタンス変数を作成し、そこに保存するとした。

    同様にbooksボタンを押した際にも左下表記がsaveとなっていた為、books#indexも同様とした。

    - [x] 解決チェック



20. books_show画面:詳細画面から削除が行えない。

    books_controller.rbの39行目 def deleteとなっていた為、def deleteに修正。

    41行目 @books.destoyとなっていた為 @books.destroyに修正。

    ->削除が出来ることを確認した。

    - [x] 解決チェック



21. user introductionのバリデーションが無い。

    追記：models/user.rb 11行目を行った。

    - [x] 解決チェック



22. 他ユーザーでもURL直打ちすれば他ユーザーのプロフィールを変更出来る不具合

    修正：users_controller.rb内部 before_action :ensure_correct_user, only: [:update]に:editを追記。

    URL直打ちでログイン中ユーザー詳細画面にリダイレクトされるように。

    - [x] 解決チェック

23. 他ユーザーでも投稿した本の「Edit」「Destroy」ボタンが表示される不具合

    views/books/show.html.erb 20行目にif文を追加した。

    -> 「Edit」「Destroy」ボタンが表示されないように。

    - [x] 解決チェック



24. 更新成功のテスト リダイレクト先が、自分のユーザ詳細画面になっていない(RSpecより)

    リダイレクト先URLが /users.idとなっている。

    users_controller.rb 21行目 users_path(@user_id) -> user_path(current_user.id)とし解決。

    - [x] 解決チェック



25. 他人の投稿編集画面 遷移できず、投稿一覧画面にリダイレクトされない(RSpecより)

    URL直打ちを行った際に、遷移できてしまう。

    books_controller.rb 27~31行目 editアクション内にif文を追加し解決。

    - [x] 解決チェック



26. 他人のユーザ詳細画面のテスト 詳細画面で左側に他人のプロフィール詳細が表示されない(Rspecより)

    users/show.html.erb infoへのrender部分 userへの変数代入として

    current_userではなく@user=User.find(params[:id])を指定した。

    - [x] 解決チェック
