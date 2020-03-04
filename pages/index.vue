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
    Height: {{ dashHeight }}
    Width: {{ dashWidth }}
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
      loaded: false,
      toggle: true,
      dashHeight: 0,
      dashWidth: 0,
      selectedWidget: {},
      widgetsOnDisplay: [
        {
          id: 0,
          dimensions: {
            height: 100,
            width: 100
          },
          position: {
            x: 0,
            y: 0
          },
          content: {
            type: '',
            text: 'Widget ' + 0
          }
        }
      ],
      occupiedX: [],
      occupiedY: []
    }
  },
  mounted () {
    this.dashHeight = this.$refs.dashboard.offsetHeight
    this.dashWidth = this.$refs.dashboard.offsetWidth
    this.loaded = true
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
    },
    toggleBox () {
      this.toggle ? this.toggle = false : this.toggle = true
    },
    checkPosition () {
      const xPos = []
      const yPos = []
      this.widgetsOnDisplay.forEach((element) => {
        xPos.push(element.position.x)
        if (element.dimensions.width === 200) {
          xPos.push(element.position.x + 100)
        }
        yPos.push(element.position.y)
        if (element.dimensions.height === 200) {
          yPos.push(element.position.y + 100)
        }
      })
      this.occupiedX = this.uniq(xPos)
      this.occupiedY = this.uniq(yPos)
    },
    getPositionX (width) {
      let x = 0
      for (; x < 400; x = x + 100) {
        if (this.occupiedX.includes(x) || (width === 200 && this.occupiedX.includes(x + 100))) {
          continue
        } else {
          return x
        }
      }
      throw new Error('No cabe en X')
    },
    getPositionY (height) {
      let y = 0
      for (; y < 500; y = y + 100) {
        if (this.occupiedY.includes(y) || (height === 200 && this.occupiedY.includes(y + 100))) {
          continue
        } else {
          return y
        }
      }
      throw new Error('No cabe en Y')
    },
    uniq (a) {
      return a.sort().filter(function (item, pos, ary) {
        return !pos || item !== ary[pos - 1]
      })
    },
    addWidget (width, height) {
      this.calculateDimensions()
      try {
        const xPos = this.getPositionX(width)
        const yPos = this.getPositionY(height)
        // eslint-disable-next-line no-console
        console.log(xPos)
        // eslint-disable-next-line no-console
        console.log(yPos)
        this.widgetsOnDisplay.push(
          {
            id: this.widgetsOnDisplay.length,
            dimensions: {
              width,
              height
            },
            position: {
              xPos,
              yPos
            },
            content: {
              type: '',
              text: 'Widget ' + this.widgetsOnDisplay.length - 1
            }
          }
        )
      } catch (error) {
        alert(error)
      }
      this.checkPosition()
    }
  }
}
</script>
