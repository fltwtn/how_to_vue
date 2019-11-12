# develop vue project

## prerequisite

- Docker version 19.03.1, build 74b1e89
- docker-compose version 1.24.1, build 4667896b
- ndenv 0.4.0 https://qiita.com/griffin3104/items/a8ae5b271bf9246eeadd
- node.js v10.15.3
- python 3.6.8

## nodejs のインストール

フロントエンド開発には nodejs を利用します。ndenv を入れて仮想環境上での開発をお勧めします。

```
$ brew install ndenv
$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(ndenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
$ git clone https://github.com/riywo/node-build.git $(ndenv root)/plugins/node-build

$ ndenv -V # versionが表示されればOK

$ ndenv install v10.15.3
```

## docker & docker-compose のインストール

システムアーキテクチャの構成に docker を利用します。

### docker

https://hub.docker.com/editions/community/docker-ce-desktop-mac  
から docker for mac をインストールする。

```
$ docker -v # Docker version 19.03.1, build xxxxxxが表示されればOK
```

### docker-compose

```
$ curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

$ chmod +x /usr/local/bin/docker-compose
```

## install

```
$ git clone <your_project>

$ cd <your_project>

# install for api server development
# venv環境で実行してください。
$ pip install -r requirements.txt

# batchサーバのビルドに時間がかかります。
# batchサーバを触る予定のない人はdocker-compose.ymlでbatchを定義している行を全てコメントアウトしてください。
$ docker-compose build
$ docker-compose up -d

# install for front app development
$ cd front-web
$ ndenv local v10.15.3
$ npm install
$ npm run serve    # start vue devserver
```

## 一から Vue プロジェクトを作る場合

クライアントサイドで起動するアプリは Vue.js で書かれている。ここでは Vue プロジェクトを作成し、アプリを開発できる環境を整える。

## 大まかな流れ

- `ndenv` をインストールする。（node.js における pyenv のようなもの）

```
$ brew install ndenv
$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(ndenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
$ git clone https://github.com/riywo/node-build.git $(ndenv root)/plugins/node-build

$ ndenv --version # versionが表示されればOK
```

- `ndenv`で `v10.15.3`をインストールし、グローバル環境で`npm`コマンドが使えるようにする。

```
$ ndenv install v10.15.3
$ ndenv global v10.15.3
$ npm --version #バージョンが表示されればOK
```

- `vue-cli` が使えるようにする。

```
$ npm install -g @vue/cli
$ vue --version #バージョンが表示されればOK
```

- プロジェクトルートにて、vue-cli でプロジェクトを作成する。

```
$ vue create front-web

# インタラクティブ画面に遷移するので、以下のように答えていく

? Please pick a preset: (Use arrow keys)
  default (babel, eslint)
❯ Manually select features　＃こちらを選択

? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router    #選択
 ◉ Vuex    #選択
 ◯ CSS Pre-processors
 ◉ Linter / Formatter    #選択
 ◉ Unit Testing    #選択
 ◉ E2E Testing    #選択

? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)    # Yを入力

? Pick a linter / formatter config: (Use arrow keys)
  ESLint with error prevention only
  ESLint + Airbnb config
  ESLint + Standard config
❯ ESLint + Prettier    #選択

? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ Lint on save    #選択
 ◯ Lint and fix on commit

? Pick a unit testing solution: (Use arrow keys)
❯ Mocha + Chai     #選択
  Jest

? Pick a E2E testing solution: (Use arrow keys)
  Cypress (Chrome only)
❯  Nightwatch (Selenium-based)     #選択

? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
❯ In dedicated config files    #選択
  In package.json

? Save this as a preset for future projects? (y/N)    #Nを入力
```

- `/front-web`ディレクトリに移動し、以下のファイルを作成する。

```
$ cd front-web
$ mkdir .vscode
$ touch ./.vscode/settings.json
# ファイルに以下を記載
{
    "editor.formatOnSave": true,
    "eslint.validate": ["javascript", { "autoFix": true, "language": "vue" }],
    "prettier.eslintIntegration": true,
    "eslint.autoFixOnSave": true
  }
$ touch vue.config.js
# ファイルに以下を記載
const path = require("path");

module.exports = {
  devServer: {
    host: "localhost",
    port: 8081,
    disableHostCheck: true
  }
};

```

- 開発サーバを起動し、`localhost:8081`にブラウザでアクセス。アイコンが表示されることを確認する。

```
$ npm run serve
```
