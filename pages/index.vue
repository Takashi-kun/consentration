<template>
  <div class="consentration-box">
    <div
      v-for="(trump, i) in trumps"
      :key="i"
      class="trumpShape"
      @click="clickTrump(trump)"
    >
      <!-- ①trumpsの配列を引っ張ってきている。 -->
      <!-- ②trumpで①の配列の中身（連想配列）を引っ張っている -->
      <!-- ③i は代入するもの。（配列の番号）（文字は何でも良い） -->
      <!-- ④@clickでクリックしたらisSurfaceをmethodsのisSurfaceに渡している -->
      <div v-if="trump.surface === true">
        <img
          class="trumpShape"
          :src="require('@/assets/images/' + trump.trumpFileName)"
        />
      </div>
      <div v-else>
        <img class="trumpShape" src="@/assets/backcard/card_back.png" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const mark = ["h", "d", "c", "s"];
    const trumps = [];
    for (let i = 0; i < 4; i++) {
      for (let k = 1; k <= 13; k++) {
        trumps.push({
          trumpMark: mark[i],
          trumpNum: k,
          surface: false,
          trumpFileName: mark[i] + ("00" + k).slice(-2) + ".gif",
        });
      }
    }
    for (let h = trumps.length - 1; h >= 0; h--) {
      const m = Math.floor(Math.random() * (h + 1));
      const tmp = trumps[h];
      trumps[h] = trumps[m];
      trumps[m] = tmp;
    }
    return {
      trumps: trumps, // trumpsプロパティをもつ連想配列
      lastSelected: null,
      block: false,
    };
  },
  methods: {
    clickTrump(trump) {
      if (trump.surface === true) {
        return;
      }
      if (this.block === true) {
        // blocked for flipping
        return;
      }

      trump.surface = true;
      if (this.lastSelected === null) {
        this.lastSelected = trump;
        return;
      }

      this.block = true;
      if (this.lastSelected.trumpNum === trump.trumpNum) {
        // matched
        console.log(
          "matched: lastSelectd:{mark: %s, num: %s}, current:{mark: %s, num:%s}",
          this.lastSelected.trumpMark,
          this.lastSelected.trumpNum,
          trump.trumpMark,
          trump.trumpNum
        );
        this.block = false;
      } else {
        setTimeout(
          (scope) => {
            scope.curTrump.surface = false;
            scope.lastTrump.surface = false;
            this.block = false;
          },
          1000,
          {
            lastTrump: this.lastSelected,
            curTrump: trump,
          }
        );
      }
      this.lastSelected = null;
    },
  },
};
</script>

<style>
.consentration-box {
  overflow: hidden;
  width: 1190px;
  background-color: green;
  margin: 50px auto;
  padding: 10px;
}
.trumpShape {
  height: 120px;
  width: 80px;
  margin: 5px;
  box-sizing: border-box;
  float: left;
}
</style>
