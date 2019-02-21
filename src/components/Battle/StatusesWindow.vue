<template>
  <div class="statuses d-flex">
    <Status v-for="status in statuses" :player="status" :key="status.id"></Status>
  </div>
</template>

<script>
import Status from "./Status.vue";

export default {
  name: "StatusesWindow",
  props: ["statuses"],
  components: {
    Status
  },
  data() {
    return {};
  }
};
</script>

<style lang="scss" scoped>
.statuses {
  height: 80px;
  background: rgba(0, 0, 0, 0.5);
}
</style>
