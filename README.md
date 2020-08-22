# Jekyll Actions の始め方

GitHubに対応したJekeyll Actionsを始めてみましょう。

GitHubへpushすると、Markdown等で記述したページが、自動的にGitHub Pagesに静的ページとして展開されます。
ここで紹介するJekyll Actionは、[PlantUML](https://plantuml.com/)にも対応していますので、ページ内で、UML表記が可能です。 
また、GitHub上だけでなく、Windows10上でもWSLを利用した静的ページを生成する方法もご紹介します。

# GitHub上でのリポジトリの作り方

まずは、GitHubにログインします。テンプレートを元にリポジトリーを作成します。

## テンプレートを使ったリポジトリ作成

GitHubの [jp-rad/jekyll-actions-quickstart](https://github.com/jp-rad/jekyll-actions-quickstart) リポジトリのページを開きます。
このページは、**Template**として設定してありますので、 [`Use this template`](https://github.com/jp-rad/jekyll-actions-quickstart/generate) というボタンをクリックするとこのページをテンプレートとして、新しいリポジトリを追加作成することができます。

この[`Use this template`](https://github.com/jp-rad/jekyll-actions-quickstart/generate)リンクを直接クリックしても同様に新しいリポジトリを追加作成することができます。

## Actionsのエラー解消

テンプレートを元に新しいリポジトリを追加作成すると、ActionsのJobが自動的に実行されますが、すぐに失敗し×マークが表示されてしまいます。

``` Build and Deploy GH Pages / jekyll
v Run jp-rad/jekyll-action@2.0.3-plantuml
  :
  6 Starting the Jekyll Action
  7 No token provided. Please set the JEKYLL_PAT environment variable.
```

ログを確認すると分かりますが、`JEKYLL_PAT`という環境変数(`Secrets`)を設定する必要があるためです。
これは、`gh-pages`ブランチを操作する権限をJobに許可するために設定するものです。

### 権限の設定

次の手順で権限の設定と`JEKYLL_PAT`の設定を行います。

1. GitHubプロファイルの `Settings` > `Developer settings` > `Personal Access Tokens` セクションに移動し、`Generate new token`で新しくトークンを作成します。
1. Noteの欄には、例えば「GitHub Actions」のような名前を付け、`public_repos`をチェックし、`Generate token`でトークンを生成します。
1. 生成されたトークンの値をコピーします（公開しないようにしてください）。
1. GitHubのリポジトリを開き、`Settings` > `Secrets` タブへ進み、`New secret`で生成したトークンを設定できます。
1. Valueの欄にコピーしたトークンをペーストし、Nameの欄に、`JEKYLL_PAT`という名称を入力し、`Add secret`で設定を追加します。


### Jobの再実行

GitHub Actions から、実行に失敗したJobを開き、再実行します。実行に成功すると、新たに`gh-pages`ブランチに作成されます。
GitHubリポジトリで、`Settings` > `Options` タブの `GitHub Pages`へ移動すると、生成されたサンプルページへのリンクが表示されています。

> Your site is published at https://jp-rad.github.io/jekyll-example/

また、GitHubリポジトリのメインページに`Environments`というリンクが新たに表示されていますので、`github-pages`の`View deployment`ボタンをクリックすると生成されたサンプルページが表示されます。

## ページの編集

サンプルページのソースファイルは、`sample_site`フォルダ内にありますので、これをGitHub上で直接編集して、コミットすれば、自動的に最新のサンプルページが生成されます。


# Widnows10上での編集方法

Windows 10 のローカル環境にGitリポジトリをクローンし、ページのソースファイルを編集し、Jekyllページを生成することもできます。

## WSLを用いた開発環境の準備

### WSL - Ubuntu20.04 LTS

Windows10で、Ubuntu 20.04 LTS の WSL をインストールします。

### Ruby開発環境

WSL を起動し、 Ruby開発環境をフルでインストールします。

```
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install -y ruby-full build-essential zlib1g-dev

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Jekyllとbundler
Jekyllとbundler gemsをインストールします。

```
gem install jekyll bundler
```

### Jekyllサイト

./myblogに新しいJekyllサイトを作ります。

```
jekyll new myblog
```

### サーバーの起動

新しいディレクトリに移動後、ローカルサーバを起動します。

```
cd myblog
bundle exec jekyll serve
```

http://localhost:4000/ で閲覧できます。
サーバーは、`Ctrl + C`キーで停止できます。

### PlantUMLのインストール

`Jekyll-PlantUML`プラグインをインストールすることにより、Jekyllページ内でPlantUMLを使用することが可能ですが、PlantUMLパッケージがインストールされている必要があります。

次のコマンドで、PlantUMLパッケージをインストールします。

```
sudo apt-get install -y plantuml
```

## GitHubリポジトリのクローンとWindows10上でのJekyllページ生成

### GitHubリポジトリのクローン

テンプレートを元に追加作成したリポジトリをWindows10上にクローンします。
例えば、`C:\workgit\jekyll-example`ディレクトリにクローンします。

### WSL上でのJekyllページ生成

WSLを起動し、クローンしたディレクトリに移動します。Windows10上のドライブCは、WSL上で`/mnt/c`としてマウントされています。
移動後、`make`コマンドを実行します。

```
cd /mnt/c/workgit/jekyll-example
make install
make s
```

`make s`コマンドで、ページの生成が完了すると、Webサーバーが起動しますので、次のURLで生成されたページを確認することができます。

 http://localhost:4000/jekyll-actions-quickstart/


### WSLでの注意点

WSLで、`make serve`を実行すると次の警告が表示されます。

```
    Auto-regeneration may not work on some Windows versions.
    Please see: https://github.com/Microsoft/BashOnWindows/issues/216
    If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch.
```

`Makefile`の`s serve:`の次の行の末尾に`--no-watch`オプションを追記します。

```Makefile
:
s serve:
	bundle exec jekyll serve --source sample_site --destination build/ --livereload --trace --no-watch
```

# License

- Released under [MIT](/LICENSE).
- Feel free to modify and reuse this project. You are required to include the license when using this code. Copy `LICENSE` to `LICENSE-source` and then modify `LICENSE` with your own name.
- Please link back to this repo as well.
