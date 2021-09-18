<script>
import {drawOval,drawRectangle,drawTriangle,drawText} from './Draw'
import $ from 'jquery'

export const mouseUp = (options,canvas) => {
    canvas.color = $("#color").css("color");
    if(canvas.isDrawingShape){
        canvas.up[0] = options.e.clientX
        canvas.up[1] = options.e.clientY  
        canvas.count++
        if(canvas.count == 1){
            var rect = drawRectangle(canvas.up,canvas.down,'#A6AAB0')
            canvas.add(rect) 
            var toolPos = canvas.down
            if(canvas.up[0]-canvas.down[0]<0){toolPos=canvas.up}
            $(".options").css({left:toolPos[0]+5,top:toolPos[1]-40,background: '#eee'})
            $(".options").html("<div class=\"square\" @click=\"square\"></div><div class=\"circle\" @click=\"circle\"></div><div class=\"triangle-right\" @click=\"triangle\"></div><div class=\"text\" @click=\"text\">T</div>")
            $(".square").css({height: '20px', width: '20px', backgroundColor: '#555',marginRight: '2em'})
            $(".text").css({height: '20px', width: '20px', alignSelf:'end', marginLeft:'20px'})
            $(".circle").css({height: '20px', width: '20px',backgroundColor: '#555',borderRadius: '50%',marginRight: '2em'})
            $(".triangle-right").css({width: 0,height: 0,borderTop: "12px solid transparent",borderLeft: "20px solid #555",borderBottom: "12px solid transparent"})

            $(".square").click(()=>{
                canvas.remove(rect)
                canvas.add(drawRectangle(canvas.up,canvas.down,canvas.color).on('selected', ()=> {
                canvas.isDrawingShape = false
                $("#shape").removeClass("active")
                }))
                $(".options").css({background: 'none'})
                $(".options").empty()
                canvas.count = 0
            })

            $(".circle").click(()=>{
                canvas.remove(rect)
                canvas.add(drawOval(canvas.up, canvas.down,canvas.color).on('selected', ()=> {
                canvas.isDrawingShape = false
                $("#shape").removeClass("active")
                }))
                $(".options").css({background: 'none'})
                $(".options").empty()
                canvas.count = 0
            })

            $(".triangle-right").click(()=>{
                canvas.remove(rect)
                canvas.add(drawTriangle(canvas.up, canvas.down,canvas.color).on('selected', ()=> {
                canvas.isDrawingShape = false
                $("#shape").removeClass("active")
                }))
                $(".options").css({background: 'none'})
                $(".options").empty()
                canvas.count = 0
            })

            $(".text").click(()=>{
                canvas.remove(rect)
                canvas.add(drawText(canvas.up,canvas.down,canvas.color).on('selected', ()=> {
                canvas.isDrawingShape = false
                $("#shape").removeClass("active")
                }))
                $(".options").css({background: 'none'})
                $(".options").empty()
                canvas.count = 0
            })
        }
    }
}

export default {
    name:"mouseUp"
}
</script>
