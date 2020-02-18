<template>
  <div>
    <div
      id="typehead"
      style="height: 500px; width: 500px; border: 1px solid blue; position: absolute; top: 40px; left: 40px;"
    >
      <vue-draggable-resizable
        v-for="(item, key) in widgetsOnDisplay"
        :key="key"
        :parent="true"
        :resizable="false"
        :grid="[100,100]"
        :x="item.position.x"
        :y="item.position.y"
        :h="item.dimensions.height"
        :w="item.dimensions.width"
        @dragstop="onDragStop"
        @activated="onActivated(item.position.x, item.position.y, key)"
      >
        Widget {{ key }} en {{ item.position.x }} {{ item.position.y }}
      </vue-draggable-resizable>
    </div>

    <div
      v-if="toggle"
      style="height: 500px; width: 500px; border: 1px solid blue; position: absolute; top: 40px; left: 580px;"
    >
      <v-btn @click="addWidget()">
        Add widget
      </v-btn>
    </div>

    <v-btn
      @click="toggleBox()"
    >
      Show/Hide
    </v-btn>
  </div>
</template>

<script>
import VueDraggableResizable from 'vue-draggable-resizable'
import 'vue-draggable-resizable/dist/VueDraggableResizable.css'

export default {
  components: {
    VueDraggableResizable
  },
  data () {
    return {
      toggle: true,
      selectedWidget: {},
      lastPosition: {
        x: 0,
        y: 0
      },
      widgetsOnDisplay: [
        {
          id: 0,
          dimensions: {
            width: 100,
            height: 100
          },
          position: {
            x: 0,
            y: 0
          },
          content: {
            type: '',
            text: 'Widget 1'
          }
        },
        {
          id: 1,
          dimensions: {
            width: 100,
            height: 100
          },
          position: {
            x: 0,
            y: 100
          },
          content: {
            type: '',
            text: 'Widget 2'
          }
        },
        {
          id: 2,
          dimensions: {
            width: 100,
            height: 100
          },
          position: {
            x: 0,
            y: 200
          },
          content: {
            type: '',
            text: 'Widget 3'
          }
        }
      ]
    }
  },
  methods: {
    onDragStop (x, y) {
      this.selectedWidget.position.x = x
      this.selectedWidget.position.y = y
      this.widgetsOnDisplay[this.selectedWidget.id] = this.selectedWidget
    },
    onActivated (x, y, key) {
      console.log(this.widgetsOnDisplay)
      console.log(this.selectedWidget.id)
      this.selectedWidget = this.widgetsOnDisplay[key]
      this.lastPosition = {
        x,
        y
      }
    },
    toggleBox () {
      this.toggle ? this.toggle = false : this.toggle = true
    },
    addWidget () {
      this.widgetsOnDisplay.push(
        {
          id: this.widgetsOnDisplay.length,
          dimensions: {
            width: 100,
            height: 100
          },
          position: {
            x: 0,
            y: this.widgetsOnDisplay[this.widgetsOnDisplay.length - 1].position.y + 100
          },
          content: {
            type: '',
            text: 'Widget ' + this.widgetsOnDisplay.length - 1
          }
        }
      )
    }
  }
}
</script>
