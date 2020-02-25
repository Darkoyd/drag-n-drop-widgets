<template>
  <div>
    <div
      id="dashboard"
      ref="dashboard"
      style="height: 500px; width: 500px; border: 1px solid blue; position: absolute; top: 40px; left: 40px;"
    >
      <vue-draggable-resizable
        v-for="(item, key) in widgetsOnDisplay"
        :key="key"
        :parent="true"
        :resizable="false"
        :grid="[100, 100]"
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
      <v-btn @click="addWidget(100,100)">
        Add widget 100x100
      </v-btn>
      <v-btn @click="addWidget(200,100)">
        Add widget 200x100
      </v-btn>
      <v-btn @click="addWidget(100,200)">
        Add widget 100x200
      </v-btn>
      <v-btn @click="addWidget(200,200)">
        Add widget 200x200
      </v-btn>
    </div>

    <v-btn
      @click="toggleBox()"
    >
      Show/Hide
    </v-btn>
    {{ dashHeight }}
    {{ dashWidth }}
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
      dashHeight: 0,
      dashWidth: 0,
      selectedWidget: {},
      lastPosition: {
        x: 0,
        y: 0
      },
      widgetsOnDisplay: []
    }
  },
  mounted () {
    this.dashHeight = document.getElementById('dashboard').offsetHeight
    this.dashWidth = this.$refs.dashboard.offsetWidth
  },
  methods: {
    calculateDimensions () {
      this.dashHeight = document.getElementById('dashboard').offsetHeight
      this.dashWidth = this.$refs.dashboard.offsetWidth
    },
    onDragStop (x, y) {
      this.selectedWidget.position.x = x
      this.selectedWidget.position.y = y
      this.widgetsOnDisplay[this.selectedWidget.id] = this.selectedWidget
    },
    onActivated (x, y, key) {
      this.selectedWidget = this.widgetsOnDisplay[key]
      this.lastPosition = {
        x,
        y
      }
    },
    toggleBox () {
      this.toggle ? this.toggle = false : this.toggle = true
    },
    addWidget (width, height) {
      this.calculateDimensions()
      this.widgetsOnDisplay.push(
        {
          id: this.widgetsOnDisplay.length,
          dimensions: {
            width,
            height
          },
          position: {
            x: 0,
            y: 0
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
