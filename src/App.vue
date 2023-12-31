<script setup>
import { ref, computed } from "vue";
import Card from "./components/Card.vue";

const items = ref([
  {
    id: 1,
    name: "アボカドディップバケット",
    description:
      "刻んだ野菜をアボカドと混ぜてディップに。こんがり焼いたバゲットとお召し上がりください。",
    price: 480,
    image: "/images/item1.jpg",
    soldOut: false,
    selected: false,
  },
  {
    id: 2,
    name: "あの日夢見たホットケーキ",
    description:
      "子供のころに食べたかった、あのホットケーキを再現しました。素朴でどこか懐かしい味をどうぞ。",
    price: 1180,
    image: "/images/item2.jpg",
    soldOut: false,
    selected: false,
  },
  {
    id: 3,
    name: "HOP WTR",
    description:
      "ロサンゼルス生まれのスパークリングウォーター。ノンカロリー、ノンアルコールの新感覚飲料です。",
    price: 320,
    image: "/images/item3.jpg",
    soldOut: true,
    selected: false,
  },
  {
    id: 4,
    name: "チーズフレンチフライ",
    description:
      "イタリア産チーズをたっぷりかけたアツアツのフレンチフライ。みんな大好きな一品です。",
    price: 670,
    image: "/images/item4.jpg",
    soldOut: false,
    selected: false,
  },
]);

/**
 * 価格を3桁ごとのカンマ付きで返す
 * @param {number} price 価格
 */
function pricePrefix(price) {
  return price.toLocaleString();
}

/**
 * 在庫のある商品数を返す
 */
function stockQuantity() {
  return items.value.filter((item) => item.soldOut === false).length;
}

/**
 * 商品の在庫状況を変更する
 * @param {object} 商品情報
 */
function stockItem(item) {
  item.soldOut = false;
}

/**
 * 現在時刻を取得する
 */
function getDate() {
  return Date.now();
}

const getDateComputed = computed(function () {
  return Date.now();
});

function changeSoldOut(id) {
  const pickElm = items.value.find((item) => item.id == id);
  pickElm.soldOut = true;
}
</script>

<template>
  <header class="header">
    <img src="/images/logo.svg" alt="" />
    <h1>Vue.js ハンズオン</h1>
  </header>
  <div>現在時刻：{{ getDate() }}</div>
  <div>現在時刻(computed){{ getDateComputed }}</div>
  <div>商品数：{{ stockQuantity() }}</div>
  <main class="main">
    <template v-for="(item, index) in items" :key="item.id">
      <div
        v-if="!item.soldOut"
        class="item"
        :class="{ 'selected-item': item.selected }"
        @click="item.selected = !item.selected"
      >
        <Card
          :id="item.id"
          :image="item.image"
          :name="item.name"
          :price="item.price"
          @sold-out="changeSoldOut"
        >
          <template #body>
            <p>{{ item.description }}</p>
          </template>
        </Card>
      </div>
      <div v-else>
        売り切れです<button type="button" @click="stockItem(item)">入荷</button>
      </div>
    </template>
  </main>
</template>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin-top: 40px;
  margin-bottom: 40px;
}

.header > img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header > h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin-top: 0;
  margin-bottom: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

.item > div.thumbnail > img {
  width: 100%;
  height: calc(100%);
  object-fit: cover;
}

.item > div.description {
  text-align: left;
  margin-top: 20px;
}

.item > div.description > p {
  margin-top: 0px;
  margin-bottom: 0px;
  font-size: 18px;
  line-height: 25px;
}

.item > div.description > span {
  display: block;
  margin-top: 10px;
  font-size: 20px;
}

.item > div.description > span > .price {
  font-size: 28px;
  font-weight: bold;
}

.selected-item {
  background-color: #e3f2fd;
}
</style>

<!-- const tasks = ref({
  taskA: 'タスクA',
  taskB: 'タスクB',
  taskC: 'タスクC',
})
オブジェクトも使える(key,valueもとれる) -->

<!-- v-if は表示を切り替えるコストが高く、
v-show は初期描画のコストが高いです。
そのため、頻繁に表示を切り替えるのであれば v-show を使い、それ以外では v-if を使うとよいでしょう。 -->

<!-- 関数と算出プロパティの違い
関数でも算出プロパティでも最終的に同じ値を取得することが可能ですが、2 つの違いはどこにあるのでしょうか。

キャッシュの違いについて
大きな違いとして 算出プロパティにはキャッシュ機能があるため、値の更新タイミングが異なります。

関数 : template が更新された時
算出プロパティ : items が更新された時 -->

<!-- templteタグなんぞ？ divで書いた時にDOMをふやしたくないなどのとき -->
