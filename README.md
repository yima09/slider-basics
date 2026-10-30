## ユーザガイド 簡易メモ

### 基本的な使い方 Getting Started

#### [インストール方法４種](https://ja.splidejs.com/guides/getting-started/)

- NPM(推奨)
- ホスティング(ダウンロードファイルを直接配置してインストール)
- CDN(jsDelivr が記載されている(他に cdnjs や unpkg などがある))
- React Vue Svelte 専用のインテグレーション用パッケージ

#### CSS のインポートの読み込むファイルの選択

- dist/css か dist/css/themes の中の splide.min.css が全てのスタイル、splide-core が最低限、他テーマのスタイル。

#### スタイルシートへのリンク

- html の linkタグや CSS の import で上記読み込むファイルを指定する。

#### [HTML](https://ja.splidejs.com/guides/getting-started/#html)

- 記述方法と注意点
  - 通常 root は section で記述するが、装飾等のスライダーなら div で `role="group"` 属性を記述する。
  - root に label を付ける。
  - 詳細は [Structure](https://ja.splidejs.com/guides/structure/) (HTML構造)ページを確認。

#### [Splideの適用](https://ja.splidejs.com/guides/getting-started/#splide%E3%81%AE%E9%81%A9%E7%94%A8)

- JS の初期化で、記述した HTML に対して Splide 適用する。
  - ESM での import での Splide クラスを読み込んでのインスタンス化する方法と HTML `<script>` での方法が記載されている。
    - コンストラクタの引数には CSS セレクタかエレメントを渡し、`mount()` メソッドを呼ぶ。
- `<head>` で Splide の「**初期化処理**」まで行うなら HTML がロードされるタイミングを待つ(監視する)記述が必要になる。
  - イベントリスナーで DOMContentLoaded イベントで指定する。
  - `<head>`で Splide を読み込むだけならここは該当しない(初期化は body 閉じの前で行った場合)

#### [複数のスライダー](https://ja.splidejs.com/guides/getting-started/#%E8%A4%87%E6%95%B0%E3%81%AE%E3%82%B9%E3%83%A9%E3%82%A4%E3%83%80%E3%83%BC)

- インスタンス１つにスライダー１つ。スライダーを複数用意するならそのインスタンスを作成する。
  ```js
  new Splide( '#slider1' ).mount();
  new Splide( '#slider2' ).mount();
  ```

---

### テーマ

- パーツのスタイルのテーマ。
  - 色が変わる程度。

---

### オプション

#### オプションの変更方法

- JS での変更は、Splide コンストラクタの第２引数にオプションを指定。
- HTML 側で指定するなら `data-splide` 属性を用いて JSON 形式でオプションを記述。
- 複数のスライダーに共通の変更を加えたい場合は `Splide.defaults = { オプション名: 値 }` でデフォ 値を設定できる。

#### [オプション一覧](https://ja.splidejs.com/guides/options/#%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3)

#### よく使われるオプション
  - `type` スライダーの種類指定。初期値：`'slide'` 両端止まる。
    - `'loop'` 両端ループ。 `'fade'` 1枚ふわっと切り替え。
  - `perPage` 1ページに表示するスライド数。初期値：`1`
  - `perMove` 1回の操作で移動するスライド数。初期値：`perPageの値`
  - `gap` スライド間の余白。初期値：`0`
  - `arrows` 前後の矢印を表示。初期値：`true`
  - `pagination` ページネーションを表示。初期値：`true`
  - `autoplay` 自動再生。初期値：`false`
  - `interval` 自動再生の間隔（ミリ秒）。初期値：`5000`
  - `speed` スライドの移動速度（ミリ秒）。初期値：`400`
  - `breakpoints` 画面幅ごとにオプションを変更。初期値：`{}`
    - max-width（デスクトップファースト、画面幅未満数値指定。`mediaQuery: 'min'` で変更できる）
    - `breakpoints: { ブレークポイントの値: { オプション名: 値 } }` で指定。

#### 次によく使われるオプション
  - `rewind` 最後まで行ったら先頭へ戻る(loopで無効)初期値：`false`
  - `width` スライダー全体の幅。初期値：`'100%'`
  - `fixedWidth` スライドの幅を指定(perPageが無効)初期値：`null`
    - `fixedWidth: '300px'` スライド1枚 = 300px になり、CSS で `.splide__slide { width: 300px; }` とするのとは少し違い **Splideが「1枚300px」と認識してスライドの表示枚数や移動量を計算する**。
      > 例えば、横幅の広いPCでは「3枚＋少し次のスライドが見える」
      >
      > スマホでは「1枚＋少し次が見える」みたいなカルーセルを作る際に使う。
    - perPage が無効。幅を揃えて余りを見切れさせるので、何枚表示の指定が効かない。
  - `autoWidth` CSS 等指定の各スライド毎幅を Splide に反映する。初期値：`false`
  - `autoHeight` スライドの高さに合わせて高さを変更。初期値：`false`
  - `start` 最初に表示するスライドを指定。初期値：`0`
  - `focus` スライドの表示位置を指定。初期値：`0`
    `focus: 0` は左端、`focus: 'center'` は中央。それ以外の数値は、`perPage` や `autoWidth`、`loop` など他の設定によって見え方が変わるため、実際のレイアウトを確認しながら指定する。
  - `padding` スライダー左右の余白。初期値：`0`
  - `drag` ドラッグ操作の設定。初期値：`'free'`
  - `keyboard` キーボード操作の設定。初期値：`'global'`
  - `lazyLoad` 画像を遅延読み込み。初期値：`false`
    - `false` lazy loadしない(画像が少ないならこれで十分)
    - `true` 必要になった画像をロード(細かいロード方式は気にしない)
    - `'nearby'` 現在位置の周辺をロード(表示付近だけ先読み、画像が多い場)
    - `'sequential'` 1枚ずつ順番にロードしていく(順番に全画像をロードしたい特殊なケース)
  - `direction` スライドの方向。初期値：`'ltr'`
    - `rtl` right to left `ttb` top to bottom
  - `cover` スライド内の画像を領域一杯に表示(bgi) 初期値：`false` 高さ系のオプション指定がいる。
#### その他
  - `destroy` 初期値 `false`。`true` スライダー破棄するがブレークポイントを継続監視しブレークポイントから外れると再有効化する。`completely` 完全破棄。
  - `rewindSpeed` 巻き戻す時間をミリ秒単位で指定(数値記述) 初期値：`speedの値`
  - `paginationDirection` pagination 自体の方向を指定 初期値 `direction` の値。
  - `easing` スライド移動時の加減速(タイミング関数)指定。
    - `cubic-bezier(0.25, 1, 0.5, 1)` 初期値。 最初に速く進み、終わりに向かって減速する(ease-out に似てる)
    - キーワード値(string `''`)
      - `linear` 一定の速度。
      - `ease` ゆっくり始まり、途中で速くなり、最後にゆっくり。
      - `ease-in` ゆっくり始まり、徐々に速くなる。
      - `ease-out` 最初は速く、徐々に減速する。
      - `ease-in-out` ゆっくり始まり、途中で速くなり、終わりに向かって減速する。
      - `step-start` 開始直後に最終状態へ切り替わる。
      - `step-end` 終了時に最終状態へ切り替わる。

---

HTML 構造 Structure

- `section.splide` `.splide__track` `ul.splide__list` `li.slide__slide` の基本構造。
  - W3C は aria-roledescription に carousel の文言があるので label にスライダーの文言を入れるのは非推奨としているが、説明を無視するブラウザもあるので Splide 的にはラベルにスライダーと書く方が親切としている。
- ルート要素に `<div>` を用いた場合 Splide は自動的に`role="region"`を挿入する。
  - 重要でない(メインコンテンツと直接関係ない)スライダーは div で `role="group"` を記述する(自動の region を上書きする)
    - 通常のスライダーは div より section にする。
- `.splide__track` `ul.splide__list` `li.splide__slide` は直接親子関係にする(直下に配置しないと動作しない)
  - `.splide__track` はルート直下でなくてもいい(間にタグを挟める)

スライドコンテナ

- `li.splide__slide` の直下に `.splide__slide__container` を記述すると Splide はスライドコンテナとして認識して `height` / `heightRatio` / `fixedHeight` の適用が `.splide__slide` から `.splide__slide__container` になる。
  - 画像と説明文のカード型スライダーで、画像は固定でカードの高さを文言の多いものに揃える時に使う。

---

矢印の調整 Arrows

- 矢印のデフォは SVG で `arrowPath` オプションで矢印 SVG の形を変更できる。
  - 矢印 SVG の `<path>` の `d` 値を直接指定して、矢印の形を変更する。
  ```svg
  <svg width="40" height="40" viewBox="0 0 40 40">
    <path d="M10 20 L30 5 L30 35 Z"></path>
  </svg>
  ```
  `M10 20 L30 5 L30 35 Z` が **SVGパス** で以下のように渡す。
  ```jsx
  new Splide('.splide', {
    arrowPath: 'M10 20 L30 5 L30 35 Z',
  }).mount();
  ```
- 矢印のタグやクラスは Splide が HTML を自動生成して、それに独自のクラスを追加したい場合は classes オプションで該当のクラスの位置に追加登録する(`splide__` 接頭子を持ったクラスは Splide 自体が参照するので削除しない)
  ```html
  <!-- 自動生成の矢印の html -->
  <div class="splide__arrows splide__arrows--ltr">
    <button
      class="splide__arrow splide__arrow--prev"
      type="button"
      aria-label="Previous slide"
      aria-controls="splide01-track"
    >
      <svg>...</svg>
    </button>
    <button
      class="splide__arrow splide__arrow--next"
      type="button"
      aria-label="Next slide"
      aria-controls="splide01-track"
    >
      <svg>...</svg>
    </button>
  </div>
  ```
  ```jsx
  // classes オプションで登録(your の方が自分のクラス名)
  new Splide('.splide', {
    classes: {
      arrows: 'splide__arrows your-class-arrows',
      arrow: 'splide__arrow your-class-arrow',
      prev: 'splide__arrow--prev your-class-prev',
      next: 'splide__arrow--next your-class-next',
    },
  });
  ```
- 矢印は `.splide__track` の前に自動生成されるが、プレースホルダ `<div class="splide__arrows"></div>` で位置指定できる。
- 矢印のカスタマイズは、以下の構成で直接 HTML を与えることで独自の矢印(前後移動UI)を作成できる。
  ```jsx
  <div class='splide__arrows'>
    <button class='splide__arrow splide__arrow--prev'>Prev</button>
    <button class='splide__arrow splide__arrow--next'>Next</button>
  </div>
  ```

---

**[ページネーションの調整 pagination](https://ja.splidejs.com/guides/pagination/)**

- ページネーションも HTML を生成する。クラスの追加 も clases オプションで行う。
- 自動生成されるコードは `._track` の親に挿入(track 同階層の後ろに生成)されプレースホルダの `<ul class="splide__pagination"></ul>` で記述位置を指定できる。
- スライドごとにページネーションを表示 する場合に、全スライド分か **ページ移動分の pagination** を表示できる。
  - `perPage` 指定でページ数分 pagination を生成(６スライド３表示なら pagination は２で３づつ移動)
  - `focus: 0` を追加指定するとページングを無効化して**１枚ずつ移動＋全スライド分の pagination** に(６スライド３表示なら pagination ６で１づつ移動、５と６はページは動かない)
  - `omitEnd: true` をさらに有効にすると、**１枚ずつ移動＋ページ分の pagination** に(６スライド３表示１づつ移動で pagination ４(５・６でページが動かないのでなくなる))
    - **omit** = 省く・除外する **end** = 終端・最後
    - 最後のほうにできる「移動できない終端側のページネーション」を省く
- ページ番号は、CSS カウンターで番号を振る。
- ページネーションのカスタマイズは、`splide.on('イベント名', callback)` (イベント登録メソッド)で指定する。
  - `pagination:mounted` ページネーション生成を知らせるためのイベント名。
  - `pagination:mounted` が発生すると、イベントハンドラ(callback)は `list`と`items` プロパティを持った `data` オブジェクトを受け取る。
    - `data.list` は `<ul class="splide__pagination">` のこと。
      - 見本コードの classList は、この ul にただクラス名を追加しただけ。
    - `data.items` は各ページネーション項目(li)の情報を持つオブジェクトの配列で `li` `button` `page` 等のプロパティを保持している。
      - `data.items[0].button.textcontent = 1` のようにして変更する。

---

**自動再生 Autoplay**

- 自動再生を使う場合は、アクセシビリティのため再生・停止ボタンを設置することを検討する(基本的に用意する)
- プログレスバーは、**ルート要素(**`.splide`)か**スライダー要素(**`.splide__track`)の直下に配置する。

---

**幅・高さの自動決定 Auto Width**

- autowidth を有効にすると異なる幅を持ったスライドを一つのスライダーに格納できる。
- autoWidth では幅の指定が必要。
  - スライド自体に width の指定がなくても中の(画像等)要素に幅指定してあればいい。
- autoWidth ＋ lazyLoad では、スライド自体に width の指定が必須(中の要素で指定の省略不可。画像がまだ読み込まれていない状態でもスライド幅を確定できるように)

---

**アクセシビリティ Accessibility**

- Splideはアクセシビリティを考慮して設計されており、スクリーンリーダー・キーボード操作などにも対応している(ランドマーク/キーボード操作/ライブリージョン/自動再生の制御/フォーカスの可視化/`aria-*` 属性など)
- Splide は、ページネーションを持たないスライダーを Basic(基本型)、持つものは Tabbed(タブ型)としてタブUIのように見なしてスライドを`tabpanel`（タブパネル）、ページネーションを`tab`（タブ）として実装する必要がある。
- ランドマーク化(**スライダー領域を支援技術に示す**)
  - landmark 言葉の意味
    `landmark` は英語で、
    - **land** = 土地・場所
    - **mark** = 印・目印
      で、直訳に近い感覚では、
      > **「土地上の目印」→「場所を示す目印」**
      > という言葉で、Web では、
      > **ページの中で「ここは○○の場所」と認識するための目印**
      > という意味で使われていて、Splide なら、
      > **「このスライダーという場所・領域を、支援技術から見つけられる目印にする」**
      > という感じ。
  - Splide は初期設定でスライダーをランドマークにするので、装飾目的スライダーの場合は `<div class="splide" role="group" ...` とするか、オプションで `new Splide( '.splide', { role: 'group' } );` とする。
- `keyboard` のスライダー自体のキーボード操作はデフォ `false` で無効、 `paginationKeyboard` のページネーションにフォーカスがあるときの操作はデフォ `true` でフォーカスがあるときは矢印キーなどで操作でき対応するスライドも表示される。
- ロービング・タブインデックス：ページネーションの `tabindex` をSplideが自動管理し、キーボード操作しやすくしてくれる。
- ライブリージョン：スライド変更時の内容をスクリーンリーダーなどの支援技術に適切に伝える処理を、Splideが自動で行ってくれる。
- 自動再生の制御は`pauseOnHover`と`pauseOnFocus`が初期状態で有効になっているため、特別な実装は必要ない。
- 視覚効果の低減：ユーザーの「動きを減らしたい」という設定（`prefers-reduced-motion`）を検知した場合 Splide が自動的にアニメーションや自動再生を抑制する。
- フォーカスの可視化は、`:focus-visible` 疑似クラスを利用してスタイルを変更する。
  - `:focus-visible` 非対応ブラウザや、`:focus-visible` と判定されない場合に備えて、`.is-focus-in` + `:focus` をフォールバックとして使う(`focus` は発生していても、`focus-visible` の表示対象になるとは限らない)
- **キーボード操作でフォーカスを当てる対象**は、**矢印(`.splide__arrow`)** Prev / Next 、**ページネーションボタン(`.splide__pagination__page`)、スライド内に記述したフォーカス可能な要素**(`<a>` `<button>` `<input>` など)
- ARIA属性：`aria-〇〇` の属性で Splide が**支援技術に構造や役割の情報を自動で伝える**。
  - **ルート** → `.splide`
  - **トラック** → `.splide__track`
  - **スライド** → `.splide__slide`

---

**i18n**：Splideがアクセシビリティ用に使用するデフォルトの英語テキストを、対象ユーザーやコンテンツに合わせて変更する。

---

Overflow：**スライドが表示領域を超えている（＝スライダーとして動かす必要がある）状態を扱う。**

- **スライド全体が表示領域に収まっているか、はみ出しているか**という**状態を** Splide が判定して `is-overflow` クラスを付け `overflow` イベントを発生させる。
- 表示領域を超えて**スライドが十分ある状態では** `.splide` に `class="splide is-overflow"` を付ける。逆にスライドが少なくて動かす必要がないときは付かない。
  - **スライドが足りないときは矢印を消す** 記述ができる。
    ```css
    .splide:not(.is-overflow) .splide__arrows {
      display: none;
    }
    ```
- コンテンツの幅が親であるリスト要素の幅を超えたとき、あるいは下回ったとき `overflow`イベントが発生 `on()`メソッドとコールバック関数が受け取る引数(真偽値)を利用すれば、このタイミングで任意の処理を実行できる。

---

**API：JavaScriptからSplideを操作・情報取得するための機能。**

`root` → ルート要素(.splide)を取得

`length` → スライド枚数を取得(枚数によって処理を変えるなど)

`index` → 現在のスライド番号を取得(現在位置表示・スライドで他要素変更など)

`go()` → スライドを移動

`on()` → イベントに処理を登録

---

イベントリファレンス Events

**よく使う**

- `move`：スライドが動き始める直前
- `moved`：スライドの移動が完了した後
- `active`：スライドがアクティブになったとき
- `inactive`：スライドが非アクティブになったとき
- `click`：スライドがクリックされたとき
- `resize`：ウィンドウサイズが変わったとき
- `overflow`：スライドが表示領域に収まる／収まらない状態が変化したとき

**たまに使う**

- `mounted`：Splideのコンポーネントがマウントされたとき
- `ready`：Splideの準備が完了したとき
- `visible`：スライドが表示領域に入ったとき
- `hidden`：スライドが表示領域から出たとき
- `resized`：Splideのリサイズ処理が完了したとき
- `drag`：ドラッグが開始されたとき
- `dragged`：ドラッグが終了したとき
- `refresh`：Splideが更新されたとき
- `updated`：オプションが更新されたとき
- `destroy`：Splideが破棄されたとき
- `arrows:mounted`：矢印が生成・マウントされたとき
- `pagination:mounted`：ページネーションが生成・マウントされたとき
- `autoplay:play`：自動再生が開始されたとき
- `autoplay:pause`：自動再生が停止・一時停止されたとき

---

エクステンション(拡張機能) Extension

- 「Splideに標準ではない自分独自の機能を、拡張機能（Extension）として追加する方法」
  例
  - **現在のスライド番号を表示する**
    - `1 / 5` → `2 / 5` → `3 / 5` …
  - **スライドが変わったら別の要素も変更する**
    - 現在のスライドに合わせて外側のテキストを変更
  - **特定の条件で独自の処理をする**
    - 最後のスライドに来たら別の処理を実行
  - **Splideのイベントと組み合わせて独自UIを作る**
    - プログレス表示、カウンター、外部ナビゲーションなど

---

トランジション Transition

「スライドが切り替わるときのアニメーション方法を、自分で作るための仕組み」

---

## チュートリアル

**画像スライダー**

- `object-fit: cover` を使う場合、画像を収めるための親(`.splide__slide`)に高さが必要 Splide オプション `height` `heightRatio` `fixedHeight` のどれかで指定する。

**サムネイルスライダー**(2つの Splide 同士の連動)

- メインとサムネイルの**2つのSplideを用意**
- `sync()` で連動
- `isNavigation: true` でサムネイルをナビゲーション化

**ギャラリー**(Splide × 自作のサムネイルUI)

- メイン → **Splide**
- サムネイル → **普通のHTMLの`ul`**
- サムネイルをクリック
  → `splide.go(index)` でメインを移動
- `move` イベント
  → 現在のサムネイルに `is-active` を付ける

**スライダーの現在地表示** Carousel Progress

- 現在位置をバーで視覚的に表示する方法。

---

## エクステンション

- Splide に後付けできる追加機能で読み込みが別途必要。

**自動スクロール** Auto Scroll

- Auto Scroll：拡張機能の読み込み方と、スクロール速度などの設定方法

**インターセクション**

- Intersection：スライダーが画面内にあるかを検知し、Autoplayなどを画面内/外で制御できる。
- Intersection 「交差」「重なり」「交わること」スライダーが画面の表示領域と重なっているか監視する。
- **inView** → 画面内に入った状態
- **outView** → 画面外に出た状態

動画 Video

- スライドにHTML動画・YouTube・Vimeoを組み込み、スライド移動と動画再生を連動させる拡張機能

**グリッド** Grid

- Grid：1つのスライド内を行・列のグリッドにして複数コンテンツを配置する拡張機能。

URL ハッシュナビゲーション

- URLのハッシュとスライダーを同期させる拡張機能(まぁ使わない)

---

## コンポーネント

Components：Splide内部の各機能を担当するコンポーネントのリファレンス。

- **Arrows** → 前後の矢印を生成・管理する
- **Autoplay** → 自動再生を管理する
- **Controller** → スライド操作を制御する
- **Direction** → スライド方向を管理する
- **Drag** → ドラッグ操作を管理する
- **Pagination** → ページネーションを管理する
- **Move** → スライド移動を管理する
- **Slides** → スライドを管理する
