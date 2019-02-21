<template>
  <div class="container window">
    <div class="row">
      <CommandButton type="attack" @command="command" title="たたかう"></CommandButton>
      <CommandButton type="magic" @command="command" title="まほう"></CommandButton>
      <div class="w-100"></div>
      <CommandButton type="item" @command="command" title="どうぐ"></CommandButton>
      <CommandButton type="escape" @command="command" title="にげる"></CommandButton>
    </div>
  </div>
</template>

<script>
import CommandButton from "./CommandButton.vue";

export default {
  name: "CommandWindow",
  components: {
    CommandButton
  },
  methods: {
    command(type) {
      this.$emit("command", type);
    }
  }
};
</script>

<style lang="scss" scoped>
.window {
  height: 250px;
}
</style>
