<template>
  <div class="window" @click="readMessage">
    <div v-if="information.type === 'attack'">
      <p>{{information.player.name}} のこうげき！</p>
      <p v-for="message in messages" :key="message">{{message}}</p>
    </div>
    <div v-if="information.type === 'escape'">
      <p>{{ information.player.name }} は {{ information.enemy.name}}から逃げようとした！</p>
      <p v-for="message in messages" :key="message">{{message}}</p>
    </div>
    <div v-if="information.type === 'magic'">
      <p>{{information.player.name}} は {{ information.magic.name }} を唱えた！</p>
      <p v-for="message in messages" :key="message">{{message}}</p>
    </div>
    <div v-if="information.type === 'mpNotEnough'">
      <p>MPが足りない！</p>
    </div>
    <div v-if="information.type === 'enemyAttack'">
      <p>{{ information.enemy.name }} のこうげき！</p>
      <p v-for="message in messages" :key="message">{{message}}</p>
    </div>
    <span class="arrow">▼</span>
  </div>
</template>

<script>
export default {
  name: "MessageWindow",
  props: ["information"],
  components: {},
  data() {
    return {
      isAllRead: false,
      messages: []
    };
  },
  methods: {
    readMessage() {
      if (this.isAllRead || this.isReadImmidiate(this.messageType)) {
        this.isAllRead = false;
        this.messages = [];
        this.$emit("read");
      } else {
        switch (this.messageType) {
          case "attack":
            this.messages.push(
              `${this.information.enemy.name} に ${
                this.information.player.power
              }のダメージ！`
            );
            this.enemyHpCheck(this.information.enemy.hp);
            this.isAllRead = true;
            break;
          case "magic":
            this.messages.push(
              `${this.information.enemy.name} に ${
                this.information.magic.power
              }のダメージ！`
            );

            this.enemyHpCheck(this.information.enemy.hp);
            this.isAllRead = true;
            break;
          case "escape":
            this.messages.push("だが逃げられなかった！");
            this.isAllRead = true;
            break;
          case "enemyAttack":
            this.messages.push(
              `${this.information.player.name} に ${
                this.information.enemy.power
              }のダメージ！`
            );

            this.isAllRead = true;
            break;
          default:
            break;
        }
      }
    },
    enemyHpCheck(enemyHp) {
      if (enemyHp <= 0) {
        this.messages.push(`${this.information.enemy.name}を倒した！`);
      }
    },
    isReadImmidiate(type) {
      if (type === "mpNotEnough") {
        return true;
      }
      return false;
    }
  },
  computed: {
    messageType() {
      return this.information.type;
    }
  }
};
</script>

<style lang="scss" scoped>
.window {
  position: relative;
  color: white;
  height: 140px;
  background: rgba(0, 0, 0, 0.5);
  padding: 10px;
}

.arrow {
  position: absolute;
  right: 10px;
  bottom: 10px;
}
</style>
