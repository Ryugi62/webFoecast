<template>
  <div class="hello">
    <canvas id="drawCanvas" width="1224" height="768" @mousedown="mosueDownCanvas($event)"
      @mousemove="mouseOverCanvas($event)" @mouseup="mouseUpCanvas()"></canvas>
    <div class="modeBox">
      <div class="lineModeBox" :class="{ online: mode === 'line' }" @click="changeMode('line')">line</div>
      <div class="curveModeBox" :class="{ online: mode === 'curve' }" @click="changeMode('curve')">curve</div>
      <div class="textModeBox" :class="{ online: mode === 'text' }" @click="changeMode('text')">text</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',

  data() {
    return {
      mode: "line",
      canvas: null,
      isDraw: false,
      startPosition: {
        x: 0,
        y: 0
      },
      endPosition: {
        x: 0,
        y: 0
      },
      lineSave: []
    }
  },

  mounted() {
    this.init()
  },

  methods: {
    init() {
      console.log(new Date(), "HelloWorld is mounted");
      this.createCanvas()
    },

    createCanvas() {
      this.canvas = document.getElementById("drawCanvas")
      this.canvas.width = 900;
      this.canvas.height = 700;
      this.canvas.style.border = "1px solid black";

      this.drawBgImage();
    },

    drawBgImage() {
      const ctx = this.canvas.getContext("2d");
      const background = new Image();
      background.src = "https://contents.creators.mypetlife.co.kr/content/uploads/2020/06/20005826/1838_3986_3444.jpg";
      background.onload = function () {
        ctx.drawImage(background, 150, 0);
      }
    },

    mosueDownCanvas(event) {
      this.isDraw = true;
      this.startPosition.x = event.layerX;
      this.startPosition.y = event.layerY;
    },

    async mouseOverCanvas(event) {
      if (this.isDraw) {
        this.endPosition.x = event.layerX
        this.endPosition.y = event.layerY

        switch (this.mode) {
          case "line":
            this.clearCanvas();
            await this.drawBgImage();
            this.getBeforeLine();
            this.drawLine(this.startPosition.x, this.startPosition.y, this.endPosition.x, this.endPosition.y)
            break;
          case "curve":

            break;
          case "text":

            break;
        }
      }
    },

    clearCanvas() {
      this.canvas.getContext("2d").clearRect(0, 0, this.canvas.width, this.canvas.height);
    },

    getBeforeLine() {
      this.lineSave.forEach(line => {
        this.drawLine(line.startX, line.startY, line.endX, line.endY)
      });
    },

    drawLine(startX, startY, endX, endY) {
      const ctx = this.canvas.getContext("2d")

      ctx.beginPath();
      ctx.moveTo(startX, startY);
      ctx.lineTo(endX, endY);
      ctx.stroke();
      ctx.closePath();
    },

    mouseUpCanvas() {
      const line = { startX: this.startPosition.x, startY: this.startPosition.y, endX: this.endPosition.x, endY: this.endPosition.y }

      this.lineSave.push(line)
      this.isDraw = false

      console.log(this.lineSave);
    },

    changeMode(mode) {
      this.mode = mode
    }
  }
}
</script>

<style scoped>
.hello {
  display: flex;
}

canvas {
  margin: auto;
}

.modeBox *:hover {
  cursor: pointer;
  background: gray;
  ;
}

.online {
  background: pink;
}
</style>