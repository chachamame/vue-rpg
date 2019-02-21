<template>
  <div class="status" :class="{isTurn: player.isTurn}">
    <div class="name">{{ player.name }}</div>
    <div class="hp">H:{{ player.hp }}</div>
    <div class="mp">M:{{ player.mp }}</div>
  </div>
</template>

<script>
export default {
  name: "Status",
  props: ["player"]
};
</script>

<style lang="scss" scoped>
.status {
  width: 80px;
  color: white;
  font-size: 1.1rem;
  padding: 2px;
}

.status:not(:first-child) {
  margin-left: 10px;
}

.isTurn {
  box-sizing: border-box;
  border: 1px solid rgba(255, 100, 100, 0.7);
}
</style>
