<template>
  <canvas id="canvas" width="500" height="500" @mousemove="mousemove" @mousedown="mouseevent(true)" @mouseup="mouseevent(false)"></canvas>
  <div>
    <h1>Choose color : {{ color }}</h1>

    <input type="radio" id="black" value="Black" v-model="color">
    <label for="black">Black</label>

    <input type="radio" id="white" value="White" v-model="color">
    <label for="white">White</label>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      width: 500,
      height: 500,
      color: 'black',
      canvas: null,
      ctx: null,
      mouse: {
        previous: {
          x: 0,
          y: 0
        },
        down: false
      }
    }
  },
  mounted() {
    this.canvas = document.getElementById('canvas'),
    this.ctx = this.canvas.getContext('2d');
  },
  methods: {
    scalePosition(x, y) {
        let posX = this.canvas.offsetLeft;
        let posY = this.canvas.offsetTop;
        let diffX = this.canvas.offsetWidth;
        let diffY = this.canvas.offsetHeight;
        let ratioX = this.width/diffX;
        let ratioY = this.height/diffY;
        return {x:(x-posX)*ratioX, y:(y-posY)*ratioY}
    },
    mouseevent(down) {
      this.mouse.down = down;
    },
    mousemove(event) {
      if (this.mouse.down) {
        
        this.ctx.beginPath();
        this.ctx.strokeStyle = this.color;
        console.log(this.ctx.fillStyle)
        let result = this.scalePosition(this.mouse.previous.x, this.mouse.previous.y);
        this.ctx.moveTo(result.x, result.y);
        result = this.scalePosition(event.clientX, event.clientY);
        this.ctx.lineTo(result.x, result.y);
        this.ctx.stroke();
      }
      this.mouse.previous.x = event.clientX;
      this.mouse.previous.y = event.clientY;
    }
  }
}

</script>

<style>
canvas {
  width: 500px;
  height: 500px;
  background-color: grey;
}
</style>
