<template>
  <div class="container window">
    <StatusesWindow :statuses="playerStatuses"></StatusesWindow>
    <MonsterWindow></MonsterWindow>
  </div>
</template>

<script>
import StatusesWindow from "./StatusesWindow.vue";
import MonsterWindow from "./MonsterWindow.vue";

export default {
  name: "BattleWindow",
  props: ["playerStatuses"],
  components: {
    StatusesWindow,
    MonsterWindow
  }
};
</script>

<style lang="scss" scoped>
.window {
  background-image: url("../../assets/battle-bg.png");
  height: 350px;
}
</style>
