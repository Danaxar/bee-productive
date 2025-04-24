<template>
  <div class="pomodoro" id="pomodoro" data-x="0" data-y="0">
    <div class="timer">{{ minutes }}:{{ seconds }}</div>
    <button @click="startTimer" :disabled="isRunning">Iniciar</button>
    <button @click="pauseTimer" :disabled="!isRunning">Pausar</button>
    <button @click="resetTimer">Reiniciar</button>
  </div>
</template>

<script>
import interact from 'interactjs';

export default {
  name: 'PomoDoro',
  data() {
    return {
      minutes: 25,
      seconds: '00',
      isRunning: false,
      timer: null
    };
  },
  mounted() {
    interact('#pomodoro')
      .draggable({
        // keep the element within the area of it's parent
        modifiers: [
          interact.modifiers.restrictRect({
            restriction: 'parent',
            endOnly: true
          })
        ],

        listeners: {
          // call this function on every dragmove event
          move: this.dragMoveListener,

          // call this function on every dragend event
          end(event) {
            var textEl = event.target.querySelector('p')

            textEl && (textEl.textContent =
              'moved a distance of ' +
              (Math.sqrt(Math.pow(event.pageX - event.x0, 2) +
                Math.pow(event.pageY - event.y0, 2) | 0))
                .toFixed(2) + 'px')
          }
        }
      })
  },
  methods: {
    dragMoveListener(event) {
      var target = event.target
      // console.log(target);
      // keep the dragged position in the data-x/data-y attributes
      var x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx
      var y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy

      // console.log('->', x, y)

      // translate the element
      target.style.transform = 'translate(' + x + 'px, ' + y + 'px)'

      // update the posiion attributes
      target.setAttribute('data-x', x)
      target.setAttribute('data-y', y)
    },
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
          this.seconds = this.seconds < 10 ? '0' + this.seconds : this.seconds;
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
      this.seconds = '00';
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
  /* background-color: red; */
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