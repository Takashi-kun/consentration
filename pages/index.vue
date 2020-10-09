<template>
  <div class="consentration-box">
    <div
      v-for="trump in trumps"
      :key="trump.mark + trump.num"
      @click="clickTrump(trump)"
    >
      <img
        v-if="trump.surface === true"
        class="trump-shape"
        :src="require('@/assets/images/' + trump.fileName)"
      />
      <img v-else class="trump-shape" src="@/assets/backcard/card_back.png" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const trumps = [];
    for (const [_, mark] of ["h", "d", "c", "s"].entries()) {
      for (let k = 1; k <= 13; k++) {
        trumps.push({
          mark: mark,
          num: k,
          surface: false,
          fileName: mark + ("00" + k).slice(-2) + ".gif",
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
      trumps: trumps,
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
      if (this.lastSelected.num === trump.num) {
        // matched
        console.log(
          "matched: lastSelectd:{mark: %s, num: %s}, current:{mark: %s, num:%s}",
          this.lastSelected.mark,
          this.lastSelected.num,
          trump.mark,
          trump.num
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
.trump-shape {
  height: 120px;
  width: 80px;
  margin: 5px;
  box-sizing: border-box;
  float: left;
}
</style>
