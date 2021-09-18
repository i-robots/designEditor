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
        <ColorPicker theme="light" :color="color" :sucker-hide="true" @changeColor="changeColor"/>
      </div>
    </div>
  </div>
</template>

<script>
import { ColorPicker } from 'vue-color-kit'
import 'vue-color-kit/dist/vue-color-kit.css'
import { fabric } from 'fabric';
import $ from 'jquery'

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
    this.canvas = new fabric.Canvas(this.$refs.can,{ isDrawingMode: false },
          {isDrawingShape: false, up:[],down:[],count: 0,color:''});
  },
  methods:{
    changeColor(color) {
        const { r, g, b, a } = color.rgba
        this.color = `rgba(${r}, ${g}, ${b}, ${a})`
    },
    setMode(draw,shape){
      this.canvas.defaultCursor = 'crosshair'
      this.canvas.isDrawingShape = shape
      this.canvas.isDrawingMode = draw
      this.canvas.down = []
      this.canvas.up = []
      this.canvas.count = 0
    },
    draw(){
      this.setMode(true,false)
      $("#draw").addClass("active")
      $("#shape").removeClass("active")
    },
    clear(){
      this.canvas.clear()
    },
    drawShapes(){
      this.setMode(false,true)
      this.canvas.color = this.color
      $("#shape").addClass("active")
      $("#draw").removeClass("active")
      this.canvas.on('mouse:up', function(options) {
          this.color = $("#color").css("color");
          if(this.isDrawingShape){
            this.up[0] = options.e.clientX
            this.up[1] = options.e.clientY  
            this.count++
            if(this.count == 1){
              var rect = drawRectangle(this.up,this.down,'#A6AAB0')
              this.add(rect) 
              var toolPos = this.down
              if(this.up[0]-this.down[0]<0){toolPos=this.up}
              $(".options").css({left:toolPos[0]+5,top:toolPos[1]-40,background: '#eee'})
              $(".options").html("<div class=\"square\" @click=\"square\"></div><div class=\"circle\" @click=\"circle\"></div><div class=\"triangle-right\" @click=\"triangle\"></div><div class=\"text\" @click=\"text\">T</div>")
              $(".square").css({height: '20px', width: '20px', backgroundColor: '#555',marginRight: '2em'})
              $(".text").css({height: '20px', width: '20px', alignSelf:'end', marginLeft:'20px'})
              $(".circle").css({height: '20px', width: '20px',backgroundColor: '#555',borderRadius: '50%',marginRight: '2em'})
              $(".triangle-right").css({width: 0,height: 0,borderTop: "12px solid transparent",borderLeft: "20px solid #555",borderBottom: "12px solid transparent"})

              $(".square").click(()=>{
                  this.remove(rect)
                  this.add(drawRectangle(this.up,this.down,this.color).on('selected', ()=> {
                    this.isDrawingShape = false
                    $("#shape").removeClass("active")
                  }))
                  $(".options").css({background: 'none'})
                  $(".options").empty()
                  this.count = 0
              })

              $(".circle").click(()=>{
                  this.remove(rect)
                  this.add(drawOval(this.up, this.down,this.color).on('selected', ()=> {
                    this.isDrawingShape = false
                    $("#shape").removeClass("active")
                  }))
                  $(".options").css({background: 'none'})
                  $(".options").empty()
                  this.count = 0
              })

              $(".triangle-right").click(()=>{
                  this.remove(rect)
                  this.add(drawTriangle(this.up, this.down,this.color).on('selected', ()=> {
                    this.isDrawingShape = false
                    $("#shape").removeClass("active")
                  }))
                  $(".options").css({background: 'none'})
                  $(".options").empty()
                  this.count = 0
              })

              $(".text").click(()=>{
                  this.remove(rect)
                  this.add(drawText(this.up,this.down,this.color).on('selected', ()=> {
                    this.isDrawingShape = false
                    $("#shape").removeClass("active")
                  }))
                  $(".options").css({background: 'none'})
                  $(".options").empty()
                  this.count = 0
              })
            }
          }
      });

      this.canvas.on('mouse:down', function(options) {
          if(this.isDrawingShape){
            this.down[0] = options.e.clientX
            this.down[1] = options.e.clientY
          }
      });

      function drawText(up,down,color){
        var pos = down
        if(up[0]-down[0]<0){
          pos = up
        }
        return new fabric.Text('Text', { 
          left: pos[0], 
          top: pos[1] ,
          fill: color
        });
      }

      function drawOval(up,down,color){
          var pos = down
          if(up[0]-down[0]<0){
            pos = up
          }
          return new fabric.Ellipse({
            left: pos[0],
            top: pos[1],
            rx: Math.abs(up[0]-down[0])/2,
            ry: Math.abs(up[1]-down[1])/2,
            fill: color,
          })
      }

      function drawRectangle(up,down,color){
          return new fabric.Rect({
            left: down[0],
            top: down[1],
            fill:  color,
            width: up[0]-down[0],
            height: up[1]-down[1],     
          })
      }

      function drawTriangle(up,down,color){
        return new fabric.Triangle({
            left: down[0], 
            top: down[1],
            width: up[0]-down[0], 
            height: up[1]-down[1], 
            fill: color
        });
      }
    }
  }
}
</script>
<style scoped>
.container{
  display: flex;
  width: fit-content;
}
.canvas{
  width: fit-content;
  background: #ddd;
}
.tools{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.options{
    position: fixed;
    display: flex;
    cursor: pointer;
    padding: 0.3em;
    border-radius: 0.5em;
}

.drawtools{
  background-color: #BCBCBC;
  height: 100%;
  padding-top: 10px;
  padding-left: 5px;
}

button{
    color: white;
    background-image: linear-gradient(to right, #296550, #58927e);
    padding: 6px 12px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    margin-right: 10px;
    appearance: none;
    font-weight: 700; 
    box-shadow: 3px 3px rgba(54, 52, 52, 0.4);
    transition: 0.4s ease-out;
}

button:hover {
    box-shadow: 1px 1px rgba(87, 81, 81, 0.6);
}

.active{
  box-shadow: none;
  background-color: #255846;
}
</style>