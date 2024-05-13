# 大規模言語モデルによるレイアウト生成エージェント

人工知能学会2024に投稿した論文「大規模言語モデルによるレイアウト生成エージェント」のコードです。ユーザの指示を入力すると、言語モデル駆動のエージェントが仮想空間にオブジェクトを配置することで、指示を反映したレイアウトを生成します。

- [人工知能学会論文リンク](https://confit.atlas.jp/guide/event-img/jsai2024/2F4-GS-5-03/public/pdf?type=in)
- arXiv論文リンク (Link be later)
    - 人工知能学会論文を英語に翻訳したもので、内容はほぼ同内容です。

<div align="center">
    タスク概要<br>
    ![task_overview](https://github.com/ckdjrkffz/layout-agent-jp/assets/129842419/088028d2-a122-42e9-a19a-8c46519da470)
</div>
<br><br>

<div align="center">
    アルゴリズム概要<br>
    ![method_overview](https://github.com/ckdjrkffz/layout-agent-jp/assets/129842419/06af82a7-fdfc-4893-a01e-db9faeca7e01)
</div>


## Requirements

- python 3.10.5
- ライブラリはrequirements.txtを用いてインストールしてください。
```
pip install -r requirements.txt
```

## How to run & use

- 最初に、`config/config.json`内の`openai_api_key`、`frontend port`、`backend port`を設定してください。
- システムは、A-Frameベースのフロントエンドと、Flaskベースのバックエンドで構成されています。両方ともこのレポジトリ内に含まれています。

### Backend

- `python api.py`で起動してください。
    - 起動には1分程度掛かります。

### Frontend

- HTTPサーバを`python run.py`で起動し、ブラウザで`<host_name>:<port>/frontend/index_house.html`にアクセスします。
    - 2つの仮想環境のサンプルが用意されています：1つ目は草原の中央に家を配置した `index_house.html` で、2つ目は部屋を模した `index_room.html`です。
- 使い方
    - カーソル移動： カーソルの移動はWASDで、カメラの回転はQEで行います。
    - オブジェクトの配置： テキストボックスに生成したいオブジェクトの名前や詳細な説明（例えば、「木」や「赤い花」）を入力し、「Place」ボタンを押すと、カーソル位置にオブジェクトが配置されます。
        - 実際には、指定したテキストが3Dオブジェクト生成モデルに入力され、それによって生成されたオブジェクトが配置されます。一度生成されたオブジェクトはサーバー内（`assets/3d_object`）に保存され、同じオブジェクトが再度指定されたときは再利用されます。
        - shap-eによるオブジェクト生成は、ランダム性の影響により、同じテキストに対して常に同じオブジェクトが生成されるとは限らないことに注意してください。
    - オブジェクトの回転： カーソル位置のオブジェクトを回転します。「Rotate」ボタンをクリックします。
    - 自動レイアウト生成：テキストボックスにレイアウト指示（例えば"trees standing around the house"）を入力し、「Layout」ボタンを押すと、レイアウト指示を反映した空間が生成されます。このレイアウトは、GPT-4V駆動のエージェントがユーザ指示に従って動作することによって生成されます。詳細なアルゴリズムについては論文を参照してください。
        - ただし、OpenAIからのレスポンスが"Your input image may contain content that is not allowed by our safety system."となり、レイアウト生成を続行できなくなることがあります。この場合、ブラウザをリロードしてやり直す必要があります。

## License
Apache License 2.0