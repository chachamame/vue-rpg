<template>
  <div :class="c" @click="emitCommand">{{ title }}</div>
</template>

<script>
export default {
  name: "CommandButton",
  props: ["title", "type"],
  data() {
    return {};
  },
  computed: {
    c() {
      let c = "";
      c += "col ";
      c += "d-flex ";
      c += "align-items-center ";
      c += "justify-content-center ";
      c += "cbutton ";
      return c;
    }
  },
  methods: {
    emitCommand() {
      this.$emit("command", this.type);
    }
  }
};
</script>

<style lang="scss" scoped>
.cbutton {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: antialiased;
  font-size: 20px;
  border: 1px solid rgba(1, 1, 1, 0.1);
  background: rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
  color: rgba(255, 255, 255, 0.8);
  height: 70px;
}
</style>
