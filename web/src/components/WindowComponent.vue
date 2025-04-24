<template>
  <div id="window-comp">
    <!-- Header -->
    <div class="window-header">
    </div>

    <!-- Body -->
    <div class="window-body">
      <slot/>
    </div>

    <!-- Footer -->
    <div class="window-footer">
    </div>

  </div>
</template>

<script>
import interact from 'interactjs';

export default {
  name: 'WindowComponent',
  props: {
    idWindow: {
      type: String,
      required: true
    },
  },
  methods: {
    dragMoveListener(event) {
      var target = event.target
      // Actualizar la posición del elemento
      var x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx
      var y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy

      // Trasladar el elemento
      target.style.transform = 'translate(' + x + 'px, ' + y + 'px)'

      // Actualizar los atributos x e y
      target.setAttribute('data-x', x)
      target.setAttribute('data-y', y)
    },
  },
  mounted() {
    // Inicializar el componente interactivo con los atributos de posición
    const windowComp = document.getElementById('window-comp');
    const x = parseFloat(windowComp.getAttribute('data-x')) || 0;
    const y = parseFloat(windowComp.getAttribute('data-y')) || 0;
    windowComp.style.transform = `translate(${x}px, ${y}px)`;

    interact('#window-comp')
      .draggable({
        inertia: true,
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
          // end(event) {
            // var textEl = event.target.querySelector('p')

            // textEl && (textEl.textContent =
            //   'moved a distance of ' +
            //   (Math.sqrt(Math.pow(event.pageX - event.x0, 2) +
            //     Math.pow(event.pageY - event.y0, 2) | 0))
            //     .toFixed(2) + 'px')
          // }
        }
      })
  },
}
</script>

<style scoped>
.draggable{
  width: 25%;
  min-height: 6.5em;
  margin: 1rem 0 0 1rem;
  background-color: #29e;
  color: white;
  border-radius: 0.75em;
  padding: 4%;
  touch-action: none;
  user-select: none;
  transform: translate(0px, 0px);
}
</style>