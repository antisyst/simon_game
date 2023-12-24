<template>
  <div class="app_body">
    <h1>Simon Game</h1>
    <div class="main_body">
    <GameBoard ref="gameBoard" />
    <div class="level_container">
      <div class="l_item">
        <input type="radio" id="easy" value="easy" v-model="difficulty" @change="setDifficulty" />
        <label for="easy">легкий</label>
      </div>

     <div class="l_item">
        <input type="radio" id="normal" value="normal" v-model="difficulty" @change="setDifficulty" />
        <label for="normal">нормальный </label>
     </div>

      <div class="l_item">
        <input type="radio" id="hard" value="hard" v-model="difficulty" @change="setDifficulty" />
        <label for="hard">сложный</label>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import GameBoard from './components/GameBoard.vue';

export default {
  components: {
    GameBoard
  },
  data() {
    return {
      difficulty: 'easy'
    };
  },
  methods: {
    setDifficulty() {
      this.$refs.gameBoard.setDifficulty(this.difficulty);
    }
  }
};
</script>

<style scoped lang="scss">
  .app_body {
    padding: 30px 3%;
    h1 {
      text-align: center;
    }
  }
  .level_container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    gap: 5px 0;
    font-size: 17px;

    .l_item {
      display: flex;
      flex-direction: row;
      align-items: center;
      gap: 0 6px;
    }
  }
  .main_body {
    margin-top: 60px;
    display: flex;
    flex-direction: row;
    gap: 50px;
    justify-content: center;
  }
</style>
