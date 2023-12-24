<template>
    <div>
        <div class="circle_container">
            <div class="buttons_container">
                <Button
                v-for="(button, index) in buttons"
                :key="index"
                :sound="button.sound"
                :color="button.color"
                :active="button.active"
                :class="{ active_color: button.active, [button.active_color]: button.active }"
                @button-click="handleButtonClick"
                :disabled="isPlaying"
                />
        </div>
        </div>
      <div class="round_c">
        <div>Round: {{ round }}</div>
        <button @click="startGame" :disabled="isPlaying">{{ isPlaying ? 'Playing...' : 'Start Game' }}</button>
      </div>
      <div v-if="isGameOver" class="wrong">Wrong sequence! Game over. Your final round is {{ round }}</div>
    </div>
  </template>
  
  <script>
  import Button from './Button.vue';
  import sound1 from '../assets/sounds/sound_1.mp3';
  import sound2 from '../assets/sounds/sound_2.mp3';
  import sound3 from '../assets/sounds/sound_3.mp3';
  import sound4 from '../assets/sounds/sound_4.mp3';
  
  export default {
    components: {
      Button,
    },
    data() {
      return {
        buttons: [
          { sound: new Audio(), color: 'rgba(255, 0, 0, 0.7)', active: false, active_color: '' },
          { sound: new Audio(), color: 'rgba(0, 255, 0, 0.7)', active: false, active_color: '' },
          { sound: new Audio(), color: 'rgba(0, 0, 255, 0.7)', active: false, active_color: '' },
          { sound: new Audio(), color: 'rgba(255, 255, 0, 0.7)', active: false, active_color: '' },
        ],
        sequence: [],
        userSequence: [],
        round: 1,
        isStrict: false,
        isPlaying: false,
        difficulty: 1500,
        difficulties: {
          easy: 1500,
          normal: 1000,
          hard: 400,
        },
        isGameOver: false,
      };
    },
    methods: {
      handleButtonClick(sound) {
        if (this.isPlaying) return;
        this.userSequence.push(sound);
        this.checkSequence();
      },
      startGame() {
        this.sequence = [];
        this.userSequence = [];
        this.round = 1;
        this.isGameOver = false;
        this.buttons.forEach((button) => {
          button.sound.src = ''; 
          button.sound.src = sound1;
        });
        this.sequence.push(this.getRandomButton().sound);
        this.playSequence();
      },
      async playSequence() {
  this.isPlaying = true;
  const totalRounds = this.sequence.length;

  for (let i = 0; i < totalRounds; i++) {
    const sound = this.sequence[i];

    await this.wait(500);

    await this.playSound(sound);
    this.highlightButton(sound);

    if (i === totalRounds - 1) {
      setTimeout(() => {
        this.isPlaying = false;
        this.userSequence = [];

        this.checkSequence();
      }, this.difficulty / 2);
    }
  }
},
      playSound(sound) {
        return new Promise((resolve) => {
          sound.play();
          sound.onended = resolve;
        });
      },
      wait(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
      },
      highlightButton(sound) {
        const button = this.buttons.find((b) => b.sound === sound);
        button.active = true;
        button.active_color = button.color;
        setTimeout(() => {
          button.active = false;
          button.active_color = '';
        }, this.difficulty / 2);
      },
      checkSequence() {
        for (let i = 0; i < this.userSequence.length; i++) {
          if (this.userSequence[i] !== this.sequence[i]) {
            this.handleWrongSequence();
            return;
          }
        }
        if (this.userSequence.length === this.sequence.length) {
          this.handleCorrectSequence();
        }
      },
      handleWrongSequence() {
        console.log('Wrong sequence! Game over.');
        this.isGameOver = true;
        this.isPlaying = false;
      },
      handleCorrectSequence() {
        if (this.round === 20) {
          console.log('Congratulations! You won!');
          this.isGameOver = true;
          this.isPlaying = false;
        } else {
          this.round++;
          this.sequence.push(this.getRandomButton().sound);
          this.playSequence();
        }
      },
      setDifficulty(difficulty) {
        this.difficulty = this.difficulties[difficulty];
      },
      getRandomButton() {
        return this.buttons[Math.floor(Math.random() * this.buttons.length)];
      },
    },
  };
  </script>
  <style lang="scss">
    .buttons_container {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        width: 400px;
        height: 400px;
        position: relative;
        border-radius: 50%;
    }
    .round_c {
        display: flex;
        flex-direction: row;
        gap: 0 30px;
        align-items: center;
        margin-top: 30px;
        justify-content: center;

        button {
            border: none;
            padding: 8px 13px;
            font-size: 18px;
            border-radius: 7px;
            cursor: pointer;
        }
    }
    .wrong {
        margin-top: 10px;
        text-align: center;
    }
 </style>