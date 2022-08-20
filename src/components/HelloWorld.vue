<template>
  <div class="hello">
    <canvas
      id="drawCanvas"
      width="1224"
      height="768"
      @click="clickedCanvas($event)"
      @dblclick="dblClickedCanavs($event)"
      @mousemove="mouseOverCanvas($event)"
    ></canvas>
    <div class="modeBox">
      <div
        class="lineModeBox"
        :class="{ online: mode === 'line' }"
        @click="changeMode('line')"
      >
        line
      </div>
      <div
        class="curveModeBox"
        :class="{ online: mode === 'curve' }"
        @click="changeMode('curve')"
      >
        curve
      </div>
      <div
        class="textModeBox"
        :class="{ online: mode === 'text' }"
        @click="changeMode('text')"
      >
        text
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
      mode: "line",
      canvas: null,
      isDraw: false,
      startPosition: {
        x: 0,
        y: 0,
      },
      endPosition: {
        x: 0,
        y: 0,
      },
      stickLineSave: [],
      curvedLineSave: [],
      curvedPosition: [],
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    init() {
      this.createCanvas();
    },

    createCanvas() {
      this.canvas = document.getElementById("drawCanvas");
      this.canvas.width = 900;
      this.canvas.height = 700;
      this.canvas.style.border = "1px solid black";

      const ctx = this.canvas.getContext("2d");
      const background = new Image();
      background.src =
        "https://contents.creators.mypetlife.co.kr/content/uploads/2020/06/20005826/1838_3986_3444.jpg";
      background.onload = function () {
        ctx.drawImage(background, 150, 0);
      };
    },

    clickedCanvas(event) {
      switch (this.mode) {
        case "line":
          if (this.isDraw) {
            const line = {
              startX: this.startPosition.x,
              startY: this.startPosition.y,
              endX: this.endPosition.x,
              endY: this.endPosition.y,
            };
            this.stickLineSave.push(line);
          } else {
            this.startPosition.x = event.layerX;
            this.startPosition.y = event.layerY;
          }
          this.isDraw = !this.isDraw;
          break;
        case "curve":
          if (this.isDraw) {
            this.curvedPosition.push({ x: event.layerX, y: event.layerY });
          } else {
            this.startPosition.x = event.layerX;
            this.startPosition.y = event.layerY;
            this.isDraw = true;
          }
          break;
        case "text":
          break;
      }
    },

    dblClickedCanavs(event) {
      event;
      const line = {
        startX: this.startPosition.x,
        startY: this.startPosition.y,
        endX: this.endPosition.x,
        endY: this.endPosition.y,
        curvedArr: this.curvedPosition,
      };
      switch (this.mode) {
        case "line":
          break;
        case "curve":
          this.curvedLineSave.push(line);
          this.isDraw = false;
          this.curvedPosition = [];
          break;
        case "text":
          break;
      }
    },

    mouseOverCanvas(event) {
      if (this.isDraw) {
        const ctx = this.canvas.getContext("2d");
        const background = new Image();

        this.endPosition.x = event.layerX;
        this.endPosition.y = event.layerY;
        background.src =
          "https://contents.creators.mypetlife.co.kr/content/uploads/2020/06/20005826/1838_3986_3444.jpg";
        background.onload = () => {
          ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
          ctx.drawImage(background, 150, 0);
          switch (this.mode) {
            case "line":
              this.stickLineSave.forEach((line) => {
                this.drawStickLine(
                  line.startX,
                  line.startY,
                  line.endX,
                  line.endY
                );
              });
              this.drawStickLine(
                this.startPosition.x,
                this.startPosition.y,
                this.endPosition.x,
                this.endPosition.y
              );
              break;
            case "curve":
              this.curvedLineSave.forEach((line) => {
                this.drawCurvedLine(
                  line.startX,
                  line.startY,
                  line.endX,
                  line.endY,
                  line.curvedArr
                );
              });
              this.drawCurvedLine(
                this.startPosition.x,
                this.startPosition.y,
                this.endPosition.x,
                this.endPosition.y,
                this.curvedPosition
              );
              break;
            case "text":
              break;
          }
        };
      }
    },

    drawStickLine(startX, startY, endX, endY) {
      const ctx = this.canvas.getContext("2d");

      ctx.beginPath();
      ctx.moveTo(startX, startY);
      ctx.lineTo(endX, endY);
      ctx.stroke();
      ctx.closePath();
    },

    drawCurvedLine(startX, startY, endX, endY, curvedArr) {
      let i = 0;
      const points = curvedArr;
      const ctx = this.canvas.getContext("2d");

      ctx.beginPath();
      ctx.moveTo(startX, startY);
      if (points[i]) {
        for (i = 0; i < curvedArr.length - 1; i++) {
          var xc = (points[i].x + points[i + 1].x) / 2;
          var yc = (points[i].y + points[i + 1].y) / 2;
          ctx.quadraticCurveTo(points[i].x, points[i].y, xc, yc);
        }
        ctx.quadraticCurveTo(points[i].x, points[i].y, endX, endY);
      } else {
        ctx.quadraticCurveTo(startX, startY, endX, endY);
      }
      ctx.stroke();
      ctx.closePath();
    },

    changeMode(mode) {
      this.mode = mode;
    },
  },
};
</script>

<style scoped>
.hello {
  display: flex;
}

.modeBox *:hover {
  cursor: pointer;
  background: gray;
}

.online {
  background: pink;
}
</style>
