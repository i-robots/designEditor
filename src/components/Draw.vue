<script>
import { fabric } from 'fabric';

export const drawOval = (up,down,color) => {
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

export const drawRectangle = (up,down,color) => {
    return new fabric.Rect({
        left: down[0],
        top: down[1],
        fill:  color,
        width: up[0]-down[0],
        height: up[1]-down[1],     
    })
}

export const drawTriangle = (up,down,color) => {
    return new fabric.Triangle({
        left: down[0], 
        top: down[1],
        width: up[0]-down[0], 
        height: up[1]-down[1], 
        fill: color
    });
}

export const drawText = (up,down,color) => {
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

export const setMode = (canvas,draw,shape) => {
    canvas.defaultCursor = 'crosshair'
    canvas.isDrawingShape = shape
    canvas.isDrawingMode = draw
    canvas.down = []
    canvas.up = []
    canvas.count = 0
}

export default{name:"Draw"}

</script>
