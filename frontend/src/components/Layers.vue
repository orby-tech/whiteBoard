<template>
  <div @mousedown="mouseDown" @mousemove="mouseMove" @mouseup="mouseUp" >
    <canvas ref="drawer" :width="wh[0]" :height="wh[1]" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Layers extends Vue {
  draw = false;
  ctx: any | null = null;
  coords = [0, 0];

  wh = [100, 100];
  delta = [ 100, 100 ]
  lines = []
  private mounted() {
    const tmp = this.$refs.drawer as HTMLCanvasElement;
    this.ctx = tmp.getContext("2d");
    this.ctx.lineWidth = 1;
    this.ctx.lineCap = "butt";
    this.ctx.strokeStyle = "black";
    this.wh = [window.innerWidth, window.innerHeight];
    window.addEventListener("keyup", (e)=>this.keyUp(e))
  }
  private mouseDown(e: any) {
    this.draw = true;
    this.coords = [e.clientX + this.delta[0], e.clientY + this.delta[1]];
    this.ctx.beginPath();
    this.lines.push(this.coords)
  }
  private mouseMove(e: any) {
    if (!this.draw) return;
    this.ctx.moveTo(this.coords[0] - this.delta[0], this.coords[1] - this.delta[1]);
    this.coords = [e.clientX + this.delta[0], e.clientY + this.delta[1]];
    this.lines.push(this.coords)
    this.ctx.lineTo(this.coords[0] - this.delta[0], this.coords[1] - this.delta[1]);
    this.ctx.stroke();
  }
  private mouseUp(e: any) {
    this.draw = false;
    this.ctx.closePath();
  }
  private keyUp(e:any) {
      if (e.key === "ArrowDown" ){
          this.changeDelta(1, 10)
      }
      if (e.key === "ArrowUp" ){
          this.changeDelta(1, -10)
      }
      if (e.key === "ArrowLeft" ){
          this.changeDelta(0, -10)
      }
      if (e.key === "ArrowRight" ){
          this.changeDelta(0, 10)
      }                  
  }
  private changeDelta( index: number, delta: number ) {
      this.delta[index] += delta
      this.redraw()
  }
  private redraw(){
      
      this.ctx.clearRect(0, 0, window.innerWidth, window.innerHeight)
      this.ctx.beginPath();
      console.log(this.delta)
      for ( let i = 1; i < this.lines.length; i++){
        
        this.ctx.moveTo(this.lines[i-1][0] - this.delta[0], this.lines[i-1][1] - this.delta[1]);
        this.ctx.lineTo(this.lines[i][0] - this.delta[0], this.lines[i][1] - this.delta[1]);
        this.ctx.stroke();
      }
      this.ctx.closePath();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
div {
  width: 100vw;
  height: 100vh;
}
canvas {
  position: fixed;
  top: 0;
  left: 0;
  image-rendering: optimizeSpeed; /* old WebKit, Safari, FireFox 3.6+ */
  image-rendering: -webkit-optimize-contrast; /* newer WebKit (CSS3 proposal) */
  image-rendering: -o-crisp-edges; /* Opera 11.6+ (CSS4 prefixed) */
  image-rendering: -moz-crisp-edges; /* FF 6+ (CSS4 prefixed) */
  image-rendering: crisp-edges; /* CSS4 */
}
</style>
