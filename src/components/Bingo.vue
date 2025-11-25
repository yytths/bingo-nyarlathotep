<template>
  <div class="bingo-container">
    <h1 class="bingo-title">
      ニャルラトホテプの化身<br>ビンゴ
    </h1>
    <div class="bingo-grid">
      <div
        v-for="(cell, index) in bingoCells"
        :key="index"
        :class="['bingo-cell', { center: index === 4, active: activeCells.includes(index) }]"
        @click="toggleCell(index)"
      >
        {{ cell === "" ? index + 1 : cell }}
      </div>
    </div>
    <div class="controls">
      <label for="tote-select">トート: </label>
      <select id="tote-select" v-model="selectedToteIndex">
        <option value="" disabled>1～9の数字を選択してください</option>
        <option
          v-for="(index, idx) in availableToteIndices"
          :key="idx"
          :value="index"
        >
          {{ index + 1 }}
        </option>
      </select>
      {{  }}
      <button @click="fillTote">入力</button>
    </div>
    <div class="controls">
      <button @click="fillRandomCell">ランダムに埋める</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BingoBoard',
  data() {
    return {
      bingoCells: ["", "", "", "", "這い寄る霧(Clawling Mist)", "", "", "", ""], // 初期状態
      nyarList: [
        "赤の女王(Queen in Red)",
        "悪心影(Aku-Shin Kage)",
        "アトゥ(Ahtu)",
        "暗黒の男(Black Man)",
        "暗黒のファラオ(Black Pharaoh)",
        "ウィッカーマン(Wicker Man)",
        "浮き上がる恐怖 (Floating Horror)",
        "オールド・ワンの使者(Messenger of the Old One)",
        "影だまり(Pool of Shadow)",
        "クルーシュチャ方程式(Kruschtya Equation)",
        "口にするのもはばかられる大司祭(High Priest Not to be Described)",
        "黒い雄牛(Black Bull)",
        "黒い風(Black Winds)",
        "黒いライオン(Black Lion)",
        "ココペリ(Kokopelli)",
        "骨格の恐怖 (Skeletal Horror)",
        "ジャック・オー・ランタン(Jack OLantern)",
        "宿主(Host)",
        "シュゴーラン(Shugoran)",
        "セト(Set)",
        "チクタクマン(Tick Tock Man)",
        "小さき違うもの(Small Crawler)",
        "血塗られた舌(Bloody Tongue)",
        "角を持つ男 (Horned Man)",
        "テスカトリボカ (Tezcatolipoca)",
        "嘆きもだえるもの(Wailing Writher)",
        "憎しみの像(Effigy of Hate)",
        "ニャルラトフィス (Nyarlatophis)",
        "パズズ (Pazzuzu)",
        "バロン・サムディ(Baron Samedi)",
        "皮膚なきもの(Skinless One)",
        "膨らんだ女 (Bloated Woman)",
        "緑の男(Green Man)",
        "無貌の神(Faceless God)",
        "野獸 (Beast)",
        "闇に棲みつくもの(Dweller in Darkness)",
        "闇の魔神 (Dark Demon)",
        "闇をさまようもの(Haunter of the Dark)",
        "ルログ(Lrogg)"
      ], // nyar.list の内容を直接埋め込み
      selectedToteIndex: null, // トートを埋めるセルのインデックス
      activeCells: [4], // クリックされたセルのインデックスを保持
    };
  },
  computed: {
    // トートを埋めるための利用可能なインデックスを計算
    availableToteIndices() {
      return this.bingoCells
        .map((cell, index) => (index !== 4 && cell === "" ? index : null))
        .filter((index) => index !== null);
    },
  },
  methods: {
    // セルの色をトグル
    toggleCell(index) {
      if (index === 4) return; // 中央セルは無視
      if (this.activeCells.includes(index)) {
        this.activeCells = this.activeCells.filter((i) => i !== index); // クリック済みなら削除
      } else {
        this.activeCells.push(index); // 未クリックなら追加
      }
      this.checkBingo(); // ビンゴチェックを実行
    },
    // ビンゴのチェック
    checkBingo() {
      const bingoLines = [
        // 横のライン
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        // 縦のライン
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        // 斜めのライン
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (const line of bingoLines) {
        if (line.every((index) => this.activeCells.includes(index))) {
          alert("ビンゴ!!!");
          return;
        }
      }
    },
    // トートを埋める
    fillTote() {
      const existingToteIndex = this.bingoCells.indexOf("トート(Thoth)");
      if (existingToteIndex !== -1) {
        alert("トートは既に埋められています！更新して最初からやり直してください。");
        return;
      }

      if (this.selectedToteIndex !== null) {
        this.bingoCells[this.selectedToteIndex] = "トート(Thoth)";
        this.selectedToteIndex = null; // 選択をリセット
      } else {
        alert("1~9を選択してください！");
      }
    },
    // ランダムにセルを埋める
    fillRandomCell() {
      const availableIndices = this.bingoCells
        .map((cell, index) => (cell === "" && index !== 4 ? index : null)) // 空セルかつ中央以外
        .filter((index) => index !== null);

      if (availableIndices.length === 0 || this.nyarList.length === 0) {
        return;
      }

      const randomIndex = availableIndices[Math.floor(Math.random() * availableIndices.length)];
      const randomWordIndex = Math.floor(Math.random() * this.nyarList.length);
      const randomWord = this.nyarList.splice(randomWordIndex, 1)[0];

      this.bingoCells[randomIndex] = randomWord;
    },
  },
};
</script>

<style scoped>
.bingo-container {
  text-align: center;
  font-family: Arial, sans-serif;
}
.bingo-title {
  font-size: 18px; /* タイトルの文字サイズを調整 */
  line-height: 1.5; /* 行間を調整 */
  margin-bottom: 20px;
}
.bingo-grid {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 5px;
  justify-content: center;
  margin: 20px auto;
}
.bingo-cell {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #000;
  font-size: 12px; /* 文字サイズを小さく変更 */
  font-weight: bold;
  text-align: center; /* テキストを中央揃え */
  word-wrap: break-word; /* 長い文字列を折り返し */
  cursor: pointer; /* クリック可能に見せる */
  transition: background-color 0.3s ease; /* 色変更のアニメーション */
}
.bingo-cell.active {
  background-color: #fd9eab; /* クリック時の色 */
}
.bingo-cell.center {
  background-color: #fd9eab;
}
.controls {
  margin: 20px 0;
}
</style>