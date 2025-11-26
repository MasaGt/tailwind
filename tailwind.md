### Tailwind　とは

Bootstrapのような既にスタイルが定義されたクラス名を利用してDOMのスタイリングを簡単にできるフレームワーク

Bootstrapはコンポーネントのスタイリングも用意している

    例: ボタンを作成したい場合 Bootstrap では .btn-primary .btn-secondary などそれ一つのクラスでボタンのスタイリングができるクラスを用意している

Tailwind ではそのようなコンポーネントレベルのクラスではなく、utility class と呼ばれる定義済みの基本的なスタイリングを利用する

    例: 色やフォントの大きさ、パディングの設定などを組み合わせてボタンを自分好みにスタイリングしていく

上記の意味において Tailwind は low level CSS framework と呼ばれている

---

### Tailwind の利用方法

まずはインストール  

- 以下の2つの方法がある

    1. CDNから取得する方法

    2. npm や yarn からインストールし、プロジェクトに含める方法

        - 開発環境のみで利用するので、`install -D` でインストールする

            ```bash
            npm install -D tailwindcss
            ```


        - この方法は、さらに以下の方法に分かれる

            - Vite で利用する

                - Vite のプラグインとして利用する

                - `tailwindcss` の他に `@tailwindcss/vite` のインストールが必要になる

            <br>

            - [PostCSS](#postcss) のプラグインとして利用する

                - `tailwindcss` の他に `@tailwindcss/postcss` のインストールが必要になる

            <br>

            - Tailwindcss CLI で利用する

                - `tailwindcss` の他に `@tailwindcss/cli` のインストールが必要になる

                - CSS記述後に、**明示的に tailwindcss cli コマンドでCSSファイルを変換する必要がある**

<br>

#### 例: npm や yarn からインストールし、プロジェクトに含める方法 (tailwindcss cli を利用 ver.)

1. 必要なパッケージのインストール

    ```bash
    npm install -D tailwindcss @tailwindcss/cli
    ```

<br>

2. Tailwindcss で提供されてるスタイルを利用 

    - CSS ファイルで利用したい場合
        
        <img src="./img/Tailwindcss-Usecase_1.svg" />

    <br>

    - HTML ファイル中にインラインスタイルで Tailwindcss が提供しているスタイルを利用したい場合

        <img src="./img/Tailwindcss-Usecase_2.svg" />

<br>

3. コマンドで tailwind から css をビルドする

    ```bash
    npx @tailwindcss/cli build (-i) <ビルド対象> -o <出力先>
    ```

<br>

#### Tailwindcss v4 (2025/11時点) と v3 (以前のバージョン) の大きな違い

- ★Tailwindcss v3 以前では、設定ファイル (tailwind.config.js) を作成しトランスパイル対象のcssファイルの指定などをする必要があった
(html ファイル中で直接 tailwindcss が提供しているクラス名をつけている場合は html もトランスパイルの対象として指定する必要がある)

    ```bash
    #tailwind.config.jsを作成するコマンド
    npx tailwindcss init
    ```

    ```js
    //tailwind.config.js
    module.exports = {
        content: {
            extend: ["./src/**/*.{html,js}"] //コンパイル対象のファイル
        }
    };
    ```

<br>

- ★Tailwindcss の @import 指定の方法が変わった

    - v3

        ```css
        @tailwind "base";
        @tailwind "components";
        @tailwind "utilities";
        ```

    <br>

    - v4

        ```css
        @import "tailwindcss";
        ```



<br>

- Tailwindcss v4 から、**taliwindcss.config.js をそもそも作成する必要がない (原則不要)**
    
    - よって、taliwindcss.config.js 中に content でトランスパイル対象を指定する必要がなくなった 

        → v4 では tailwindcss が提供しているクラスを使用しているファイルを自動検出するようになった

    <br>

    - v3 以前では tailwindcss.config.js 中に設定していた `theme` などは、個別のCSSファイル中に定義できるようになった

        ```js
        //v3 で独自のカラークラスを定義したい場合
        //tailwindcss.config.js
        module.exports = {
            theme: {
                colors: {
                    silver-gray: "#afafb0"
                }
            }
        };
        ```

        ```css
        /* v4 からは個別のファイルに定義可能になった */
        /* style.css */
        @theme{
             /* `colors` は `--color-*` 形式で定義 */
             --color-silver-gray: #afafb0
        }
        ```

    <br>

    - [tailwindcss-railsの導入とTailwind CSS v4の変更点まとめ](https://qiita.com/GleapPost/items/195dfd2720ec425f9404#tailwind-css-v4の主な変更点) や [Tailwind CSS V4まとめ！](https://zenn.dev/miz_dev/articles/tailwind-css-v4#コンテンツの自動検出) の記事がわかりやすい

<br>
<br>

参考サイト

[【備忘録】Tailwind CSSの環境構築（CDN版・CLI版）](https://qiita.com/nao_nba/items/bf4048f33ff8bab227b5)

[Get started with Tailwind CSS][https://tailwindcss.com/docs/installation/tailwind-cli]

[tailwindcss-railsの導入とTailwind CSS v4の変更点まとめ](https://qiita.com/GleapPost/items/195dfd2720ec425f9404#tailwind-css-v4の主な変更点)

[Upgrade guide](https://tailwindcss.com/docs/upgrade-guide#using-tailwind-cli)

---

### なぜ -D でインストールするのか?

Tailwind はビルド時に実際に利用されているクラスのみを CSS ファイルに別途出力し、参照側はそっちのファイルを見るようになるから

---

### Tailwind を CSS プリプロセッサの plugin として利用する方法

- [PostCSS](#postcss) の plugin として利用することができる **(Next.js ではこちらの方法で利用する)**

<br>

- create-next-app で Tailwind を利用するを選択した場合、 postcss, tailwindcss はデフォルトでインストールされている状態になる

<br>

- 後は必要に応じて autoprefixer などのプラグインを別途インストールする

    → tailwind はビルド時にベンダープレフィックスを付けないため

<br>

#### autoprefixer のインストール & 設定

1. インストールする

```bash
npm install -D autoprefixer
```

<br>

2. postcss.config.js に autopefixer を postcss のプラグインとして利用するよう追加する

```js
// postcss.config.(m)js
const config = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}, // ここ
  },
};

export default config;
```

---

### 参考になるサイト

入門系

- [Tailwind CSS 入門と実践](https://zenn.dev/yohei_watanabe/books/c0b573713734b9/viewer/fac5ab)

- [利用者爆増中 初めてでもわかるTailwind CSS入門 基礎編](https://reffect.co.jp/html/tailwindcss-for-beginners#Tailwind_CSS)

- [Tailwind CSS実践入門](https://gihyo.jp/article/2023/07/tailwindcss-practice-02)

---

### PostCSS

- 基本的なことは[こちら](https://github.com/MasaGt/html_css/blob/ae24d2a3aab9ca8c41164afd31b19407f460188c/css/PostCSS.md)で解説している

<br>

#### PostCSS の特徴

- css → css に変換するのが PostCSS の特徴

    - .sass / .scss を css に変換することはできない

<br>

- プラグインで自分の好きな機能を追加できる

<br>
<br>

参考サイト

[PostCSSとは？フロントエンドエンジニアが知っておくべきCSSの未来](https://humhum.co.jp/4984/)

[SassとPostCSSの違いを徹底比較！ フロントエンドエンジニアが知っておくべきこと](https://humhum.co.jp/4902/)