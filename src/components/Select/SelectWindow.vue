<template>
  <div class="container window">
    <div class="row" v-if="information.type === 'magic'">
      <div
        class="col unit d-flex align-items-center justify-content-center"
        v-for="(unit,index) in information.units"
        :key="index"
        @click="useMagic(unit)"
      >{{ unit.name }}</div>
    </div>
    <div class="row" v-if="information.type === 'item'">
      <div
        class="col unit d-flex align-items-center justify-content-center"
        v-for="(unit,index) in information.units"
        :key="index"
      >{{ unit}}</div>
    </div>
    <div class="bottom text-right">
      <div @click="back" class="back">もどる</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelectWindow",
  props: ["information"],
  methods: {
    useMagic(magic) {
      //使用した魔法を通知する
      this.$emit("useMagic", magic);
    },
    back() {
      this.$emit("back");
    }
  }
};
</script>

<style lang="scss" scoped>
.window {
  height: 140px;
  background: rgba(0, 0, 0, 0.5);
}

.bottom {
  height: 20px;
}

.back {
  color: white;
}

.unit {
  height: 40px;
  color: white;
  border: 1px solid rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
}
</style>
