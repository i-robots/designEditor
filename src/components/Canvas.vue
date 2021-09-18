<template>
  <div class="container">
    <div class="canvas">
      <canvas ref="can" width="800" height="530"></canvas>
      <div class="options"></div>
    </div>
    <div class="tools">
      <div class="drawtools">
        <button @click="drawShapes" id="shape">Shapes</button>
        <button @click="draw" id="draw">Draw</button>
        <button @click="clear">Clear</button>
      </div>
      <span id="color" :style="{color:color}"></span>
      <div class="color-picker">
        <ColorPicker theme="dark" :color="color" :sucker-hide="true" @changeColor="changeColor"/>
      </div>
    </div>
  </div>
</template>

<script>
import { ColorPicker } from 'vue-color-kit'
import 'vue-color-kit/dist/vue-color-kit.css'
import { fabric } from 'fabric';
import $ from 'jquery'
import {mouseUp} from './Mouseup'
import {setMode} from './Draw'

export default {
  name:"Canvas",
  data(){
    return{
      canvas: null,
      color: '#BCBCBC'
    }
  },
  components:{
    ColorPicker
  },
  mounted() {
    this.canvas = new fabric.Canvas(this.$refs.can,{ 
      isDrawingMode: false,
      isDrawingShape: false, 
      up:[],
      down:[],
      count: 0,
      color:''
    });
  },
  methods:{
    changeColor(color) {
        const { r, g, b, a } = color.rgba
        this.color = `rgba(${r}, ${g}, ${b}, ${a})`
    },
    draw(){
      setMode(this.canvas,true,false)
      $("#draw").addClass("active")
      $("#shape").removeClass("active")
    },
    clear(){
      this.canvas.clear()
    },
    drawShapes(){
      setMode(this.canvas,false,true)
      this.canvas.color = this.color
      $("#shape").addClass("active")
      $("#draw").removeClass("active")
      this.canvas.on('mouse:up', function(options){mouseUp(options,this)});
      this.canvas.on('mouse:down', function(options) {
          if(this.isDrawingShape){
            this.down[0] = options.e.clientX
            this.down[1] = options.e.clientY
          }
      });
    }
  }
}
</script>
<style  scoped src="@/assets/css/style.css">
</style>