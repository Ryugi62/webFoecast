<template>
  <div class="hello">
    <cavnas id="drawCanvas" width="1224" height="768"></cavnas>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',

  mounted() {
    this.init()
  },

  methods: {
    init() {
      this.createCanvas()
    },

    createCanvas() {
      const canvas = document.getElementById('drawCanvas');

      console.log(canvas)

      // canvas.style.border = "1px solid black";

      // const ctx = canvas.getContext("2d");
      // ctx.fillStyle = "rgba(255, 0, 0, 0.2)";
      // ctx.fillRect(100, 100, 200, 200);
      // ctx.fillStyle = "rgba(0, 255, 0, 0.2)";
      // ctx.fillRect(150, 150, 200, 200);
      // ctx.fillStyle = "rgba(0, 0, 255, 0.2)";
      // ctx.fillRect(200, 50, 200, 200);
    }
  },

}
</script>

<style scoped>
</style>
