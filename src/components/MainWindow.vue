<template>
  <div class="container main">
    <BattleWindow :playerStatuses="playerStatuses"></BattleWindow>
    <CommandWindow v-if="isCommand" @command="actionCommand"></CommandWindow>
    <SelectWindow
      v-else-if="isSelect"
      @back="windowShift"
      :information="selectInformation"
      @useMagic="useMagic"
    ></SelectWindow>
    <MessageWindow v-else @read="readMessage" :information="messageInformation"></MessageWindow>
  </div>
</template>

<script>
import BattleWindow from "./Battle/BattleWindow.vue";
import CommandWindow from "./Command/CommandWindow.vue";
import SelectWindow from "./Select/SelectWindow.vue";
import MessageWindow from "./Message/MessageWindow.vue";

export default {
  name: "MainWindow",
  components: {
    BattleWindow,
    CommandWindow,
    SelectWindow,
    MessageWindow
  },
  data() {
    return {
      isPlayerTurn: true,
      currentId: 0,
      playerStatuses: [
        {
          id: 1,
          name: "taro",
          hp: 200,
          mp: 30,
          power: 20,
          magics: [
            {
              id: 1,
              name: "fire",
              type: "attack",
              power: 20,
              cost: 10
            },
            {
              id: 2,
              name: "thunder",
              type: "attack",
              power: 30,
              cost: 15
            },
            {
              id: 3,
              name: "freeze",
              type: "attack",
              power: 20,
              cost: 10
            }
          ],
          isTurn: true
        },
        {
          id: 2,
          name: "hanako",
          hp: 100,
          mp: 999,
          power: 4,
          magics: [
            {
              id: 1,
              name: "heal",
              type: "heal",
              power: 10,
              cost: 100
            },
            {
              id: 2,
              name: "thunder",
              type: "attack",
              power: 30,
              cost: 15
            }
          ],
          isTurn: false
        },
        {
          id: 3,
          name: "jiro",
          hp: 150,
          mp: 0,
          power: 50,
          magics: [],
          isTurn: false
        }
      ],
      enemy: {
        name: "スライム",
        hp: 2000,
        power: 50
      },
      items: ["草", "草", "草", "草", "草", "花", "草"],
      isCommand: true,
      isSelect: false,
      messageInformation: {},
      selectInformation: {},
      isReadyEnemyAttack: false
    };
  },
  methods: {
    actionCommand(type) {
      if (this.isPlayerTurn) {
        switch (type) {
          case "attack":
            this.enemy.hp -= this.currentPlayer.power;

            this.messageInformation = {
              type: "attack",
              player: this.currentPlayer,
              enemy: this.enemy
            };

            this.playerShift();
            this.windowShift("attack");
            break;
          case "magic":
            //表示させるデータの作成
            this.selectInformation = {
              type: "magic",
              units: this.currentPlayer.magics
            };
            this.windowShift("magic");
            break;
          case "item":
            this.selectInformation = {
              type: "item",
              units: this.items
            };
            this.windowShift("item");
            break;
          case "escape":
            this.messageInformation = {
              type: "escape",
              player: this.currentPlayer,
              enemy: this.enemy
            };
            this.windowShift("escape");
            break;
          default:
            break;
        }
      }
    },
    useMagic(magic) {
      if (this.currentPlayer.mp < magic.cost) {
        this.messageInformation = {
          type: "mpNotEnough",
          magic: magic,
          player: this.currentPlayer
        };
        this.windowShift("attack");
        return;
      }
      switch (magic.type) {
        case "attack":
          //攻撃と同じ処理を行う
          this.enemy.hp -= magic.power;
          this.currentPlayer.mp -= magic.cost;
          this.messageInformation = {
            type: "magic",
            magic: magic,
            player: this.currentPlayer,
            enemy: this.enemy
          };

          this.playerShift();
          this.windowShift("attack");
          break;
        default:
          break;
      }
    },
    windowShift(type) {
      //画面が増えたので拡張する
      switch (type) {
        case "attack":
        case "escape":
          this.isCommand = false;
          this.isSelect = false;
          break;
        case "magic":
        case "item":
          this.isCommand = false;
          this.isSelect = true;
          break;
        default:
          this.isCommand = true;
          this.isSelect = false;
          break;
      }
    },
    readMessage() {
      this.windowShift();

      this.turnShift();

      if (this.isReadyEnemyAttack) {
        this.enemyAttack();
        this.isReadyEnemyAttack = false;
      }
    },
    enemyAttack() {
      let idx = Math.floor(Math.random() * this.playerStatuses.length);
      let targetPlayer = this.playerStatuses[idx];
      targetPlayer.hp -= this.enemy.power;
      this.messageInformation = {
        type: "enemyAttack",
        player: targetPlayer,
        enemy: this.enemy
      };
      this.windowShift("attack");
    },
    playerShift() {
      this.currentId++;
      if (this.currentId >= this.playerStatuses.length) {
        this.currentId = 0;

        //エネミーの攻撃
        this.isReadyEnemyAttack = true;
      }
    },
    turnShift() {
      //ターンをリセット
      this.playerStatuses.forEach(status => {
        status.isTurn = false;
      });
      this.currentPlayer.isTurn = true;
    }
  },
  computed: {
    currentPlayer() {
      return this.playerStatuses[this.currentId];
    }
  }
};
</script>

<style lang="scss" scoped>
.main {
  padding-top: 20px;
}
</style>
