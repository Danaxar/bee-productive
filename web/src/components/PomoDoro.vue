<template>
  <WindowComponent idWindow="pomodoro" class="pomodoro">
    <div>
      <div class="timer">{{ formattedTime }}</div>
      <button @click="startTimer" :disabled="isRunning">Iniciar</button>
      <button @click="pauseTimer" :disabled="!isRunning">Pausar</button>
      <button @click="resetTimer">Reiniciar</button>
    </div>
  </WindowComponent>
</template>

<script>
import WindowComponent from './WindowComponent.vue';

export default {
  name: 'PomoDoro',
  components: {
    WindowComponent
  },
  data() {
    return {
      minutes: 25,
      seconds: 0,
      isRunning: false,
      timer: null
    };
  },
  computed: {
    formattedTime() {
      const formattedMinutes = this.minutes < 10 ? '0' + this.minutes : this.minutes;
      const formattedSeconds = this.seconds < 10 ? '0' + this.seconds : this.seconds;
      return `${formattedMinutes}:${formattedSeconds}`;
    }
  },
  methods: {
    startTimer() {
      if (!this.isRunning) {
        this.isRunning = true;
        this.timer = setInterval(() => {
          if (this.seconds === 0) {
            if (this.minutes === 0) {
              this.resetTimer();
              alert('¡Tiempo terminado!');
            } else {
              this.minutes--;
              this.seconds = 59;
            }
          } else {
            this.seconds--;
          }
        }, 1000);
      }
    },
    pauseTimer() {
      this.isRunning = false;
      clearInterval(this.timer);
    },
    resetTimer() {
      this.isRunning = false;
      clearInterval(this.timer);
      this.minutes = 25;
      this.seconds = 0;
    }
  }
};
</script>

<style scoped>
.pomodoro {
  text-align: center;
  margin-top: 20vh;
  font-family: Arial, sans-serif;
  background: white;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 20px;
  width: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  user-select: none;
}

.timer {
  font-size: 3rem;
  margin: 20px 0;
}

button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  background-color: #4facfe;
  color: white;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>