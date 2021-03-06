### 【羅針盤を手に入れる】<br>Django基礎ハンズオンⅡ

0. ブログアプリの機能のおさらい（20分）
1. ブログ画面からブログ記事を作る（35分）
2. ブログ記事の詳細が見られるようにする（25分）
3. <div class="django-girls-highlight">ブログ記事に画像を追加できるようにする（35分）</div>

+++

### 画像の追加

- ブログなのだから画像を追加したい（例：旅行の記事に写真を載せる）
- **注**：Django Girls Tutorialの範囲外

+++

### ブログ記事の詳細が見られるようにする ゴール

- 記事作成フォームで画像をアップロードできる（0枚or1枚）
- 記事詳細画面で画像が見られる

+++

### 羅針盤のうち何が必要？

<span class="eighty-percent-img">
![テンプレート？URL設定？モデル？ビュー？](elv_Feb_django_developcompass/assets/django_compass.png)
</span>

+++

### 羅針盤に照らして 1/2

新しくページを作るわけではない

- リンクを追加しない
- URLも追加しない

+++

### 羅針盤に照らして 2/2

変更箇所

- モデル
- ビュー
- 既存のテンプレート（画像表示）

+++

### 画像追加のケースの羅針盤

<span class="eighty-percent-img">
![1.モデル、2.ビュー、3.（既存）テンプレート](elv_Feb_django_developcompass/assets/part3/django_compass_part3.001.png)
</span>


+++

### 画像アップロード機能 実装手順

1. 画像を扱うための設定変更
2. Postモデルに画像を扱う項目を追加
3. フォームからの画像アップロード（ビュー）
4. 詳細画面に表示（テンプレート）

---

### (1)画像を扱うための設定変更

1. 画像の扱いに必要なモジュールをインストール
2. 画像アップロード用フォルダを作る
3. settings.py変更
4. urls.py変更

+++

### (1-1)画像の扱いに必要なモジュールをインストール

- 画像処理に使うモジュール：Pillow
- コマンドラインで`pip install Pillow`
  - **注**：Anacondaの方は`conda install Pillow`
- Pillowでできること参考：[『退屈なことはPythonにやらせよう』英語版](http://automatetheboringstuff.com/chapter17/)

+++

### (1-2)画像アップロード用フォルダ

- ブログの@color[#ff9400](ユーザがアップロードした)画像を置くフォルダ
- 慣例によりフォルダ名はmediaとする
- 注：`mysite`プロジェクトや`blog`アプリがあるのと同じ階層のフォルダ
- コマンドラインで`cd ~/errorfriends && mkdir media`

+++

### (1-3)mysite/settings.py

```python
# 末尾に以下の2行を追加
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

+++

### （参考）settings.py設定内容

- MEDIA_ROOT：アップロードした画像を置くフォルダ
- MEDIA_URL：配信設定（外に見えるURL）
- 開発時向けの設定をしています [参考](https://docs.djangoproject.com/ja/2.1/howto/static-files/#serving-files-uploaded-by-a-user-during-development)

+++

### (1-4)mysite/urls.py

```python
from django.contrib import admin
from django.urls import path, include
# 以下の2行を追加
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
# 以下の2行を追加
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

+++

### （参考）mysite/urls.py設定内容

- 開発環境のアップロードファイルの扱いを追加
- 開発時向けの設定をしています [参考](https://docs.djangoproject.com/ja/2.1/howto/static-files/#serving-files-uploaded-by-a-user-during-development)
- ここまでの設定により、アップロードした画像のURLは次のようになる
  - @size[75%](`http://127.0.0.1:8000/media/photos/2019/02/14/a_image.jpg`)

---

### (2)Postモデルに画像を扱う項目を追加

<span class="eighty-percent-img">
![まずはモデルを変更](elv_Feb_django_developcompass/assets/part3/django_compass_part3.002.png)
</span>

+++

### (2)Postモデルに画像を扱う項目を追加

1. models.py編集（エディタ）
2. マイグレーション実施（コマンドライン）

+++

### (2-1)models.py編集内容

- 画像を扱う"列"をimageとして追加する

author | title | text | @color[#ff9400](image（追加）)
----- | ----- | ----- | -----

Postモデルの一部の列を表示

+++

### blog/models.py

```python
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    # 以下の2行を追加
    image = models.ImageField(
            upload_to='photos/%Y/%m/%d', blank=True)
    # この上の2行を追加
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def __str__(self):
        return self.title
```

+++

### （参考）ImageField

- `upload_to`の指定
- `blank=True`の指定

+++

### （参考）`upload_to='photos/%Y/%m/%d'`

- mediaディレクトリの下にphotosディレクトリができる
- photosディレクトリの下に日毎に分けて保存
- [ドキュメント中の参考箇所](https://docs.djangoproject.com/ja/2.1/ref/models/fields/#django.db.models.FileField.storage)

+++

### （参考）2019/02/14に保存する場合

- media/
  - ┗ photos/
    - ┗ 2019/
      - ┗ 02/
        - ┗ 14/
          - ┗ アップロードした画像

+++

### （参考）列の追加のイメージ

Postモデルの一部の列を表示

author | title | text | @color[#ff9400](image（追加）)
----- | ----- | ----- | -----
ftnext | 既存記事1 | 本文1 | ？

+++

### （参考）`blank=True`

- すでに作られた記事がある状態で、画像用の"列"を追加
- すでに作成された記事に画像を設定するかを決める必要がある
- 今回はしないとして設定した（設定する場合は、デフォルトで取る値を指定する）

+++

### (2-2)models.pyの変更をデータベースに反映

**注**：models.pyを変更しただけでは画像はアップロードできない

1. `python manage.py makemigrations blog`
2. `python manage.py migrate blog`

+++

### （参考）makemigrationsとmigrate

- makemigrations：models.pyを元に移行ファイルを作成
  - 移行ファイルはテーブルの定義でマイグレーションと呼ばれる
- migrate：データベースに移行ファイルを適用
	- データベースのテーブルが変更される

+++

### 動作確認

- Ctrl+Cで止めている場合、コマンドラインで`python manage.py runserver`
- ブラウザで管理画面にアクセス http://127.0.0.1:8000/admin/
- ログインし、Postの追加画面へ

+++

<span class="eighty-percent-img">
![管理画面からアップロードできるようになりました](elv_Feb_django_developcompass/assets/part3/1_add_image_from_admin.png)
</span>

---

### (3)フォームからの画像アップロード

<span class="eighty-percent-img">
![続いてビューを変更](elv_Feb_django_developcompass/assets/part3/django_compass_part3.003.png)
</span>

+++

### (3)フォームからの画像アップロード

1. post_edit.html
2. views.py
3. forms.py

+++

### (3-1)blog/post_edit.htmlの編集

```html
<!-- 省略 -->
        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    <h2>New post</h2>
                    <!-- 画像アップロードができるようにenctype追加 -->
                    <form method="POST" class="post-form" enctype="multipart/form-data">{% csrf_token %}
                        {{ form.as_p }}
                        <button type="submit" class="save btn btn-default">Save</button>
                    </form>
                </div>
            </div>
        </div>
<!-- 省略 -->
```

+++

### （参考）blog/post_edit.htmlの編集

- formタグのenctype属性
- フォームから画像ファイルを送るために`enctype="multipart/form-data"`と設定
- [MDN 参考ドキュメント](https://mzl.la/2HZ3VfL)

+++

### (3-2)blog/views.pyの編集

```python
# importと post_list関数には変更なし

def post_new(request):
    if request.method == "POST":
        # 変更点は以下の行に引数を追加するだけ
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_list')
    else:
        form = PostForm()
    return render(request, 'blog/post_edit.html', {'form': form})
```

+++

### （参考）blog/views.pyの編集

- `form = PostForm(request.POST, request.FILES)`（引数追加）
- [Django ドキュメント 参考箇所](https://docs.djangoproject.com/ja/2.1/ref/forms/api/#binding-uploaded-files)

+++

### (3-3)blog/forms.pyの編集

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ('title', 'text', 'image',) # imageを追加
```

+++

### blog/forms.pyの編集

- フォームで入力できる項目を設定した
- 変更前：タイトルと本文（画像はフォームからアップロードできない）
- 変更後：フォームから画像が選択できる

+++

### 動作確認

- Ctrl+Cで止めている場合、コマンドラインで`python manage.py runserver`
- ブラウザで http://127.0.0.1:8000/ にアクセス
- +アイコン（リンク）からフォームへ。画像をアップロードしてみる

+++

### 記事が作られました

<span class="seventy-percent-img">
![画像つきで記事が作成できます](elv_Feb_django_developcompass/assets/part3/2_add_image_from_form.png)
</span>

+++

### 画像アップロードはあくまでオプション

<span class="seventy-percent-img">
![画像を設定しなくても記事が作れます](elv_Feb_django_developcompass/assets/part3/3_create_post_no_image.png)
</span>

+++

### 作成した記事は一覧に表示

![画像つきの記事も画像なしの記事も一覧に表示されます](elv_Feb_django_developcompass/assets/part3/4_post_list.png)

+++

### (3)フォームからの画像アップロード まとめ

1. post_edit.html（画像アップロード用にformの設定変更）
2. views.py（アップロードされた画像が保存できるように修正）
3. forms.py（画像がアップロードできるように設定変更）

---

### (4)詳細画面に表示

<span class="eighty-percent-img">
![最後にテンプレートを変更](elv_Feb_django_developcompass/assets/part3/django_compass_part3.004.png)
</span>

+++

### (4)詳細画面に表示

- post_detail.html
- 画像がある記事は、本文の前に表示する

+++

### blog/post_detail.html

`src="{{ post.image.url }}"`をimgタグに指定

```html
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        <div class="date">
            {% if post.published_date %}
                <p>published: {{ post.published_date }}</p>
            {% endif %}
            <p>created: {{ post.created_date }}</p>
        </div>
        <h1>{{ post.title }}</h1>
        <p><em>Author: {{ post.author }}</em></p>
        <!-- 画像の表示を追加 -->
        {% if post.image %}
            <img ><!-- src属性を追加してください -->
        {% endif %}
        <!-- 画像の表示を追加 終わり -->
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

+++

### 画像の表示を追加

- `{% if post.image %}`：画像が設定されているかをチェック
- 画像が設定されているとき、True（post.image.urlで画像のURLが取れる）
- 画像が設定されていないページには画像を表示しない

+++

### （参考）画像の属性

- post.image.url
- post.image.width
- post.image.height

[Django ドキュメント ImageField](https://docs.djangoproject.com/ja/2.1/ref/models/fields/#django.db.models.ImageField)

+++

### 動作確認

- Ctrl+Cで止めている場合、コマンドラインで`python manage.py runserver`
- ブラウザで http://127.0.0.1:8000/ にアクセス
- 手順3で作った記事の詳細画面へ遷移

+++

<span class="eighty-percent-img">
![アップロードした画像が表示されています](elv_Feb_django_developcompass/assets/part3/5_post_detail_with_image.png)
</span>

+++

### まとめ：ブログ記事に画像を追加できるようにする

1. 画像を扱うための設定変更
2. Postモデルに画像を扱う項目を追加
3. フォームからの画像アップロード
4. 詳細画面に表示
