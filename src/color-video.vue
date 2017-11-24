<style scoped>
.color-video {
    position: relative;
    display: inline-block;
}
</style>
<template>
<section class="color-video" :style="'height:'+videoHeight+'px;width:'+videoWidth+'px'" @click="clickCanvas">
    <canvas ref="mycanvas" :width="videoWidth" :height="videoHeight" v-on:mousemove="tranceCanvas"></canvas>
    <video ref="myvideo" crossOrigin="*" webkit-playsinline :src="videoUrl" @timeupdate="videoTimeupdate" style="display:none" :loop="loop" :autoplay="autoplay" :muted="muted"></video>
</section>
</template>
<script>
export default {
    name: 'video-colorpicker',
    props: {
        videoUrl: {
            type: String,
            default: ''
        },
        videoWidth: {
            type: Number,
            default: 0
        },
        videoHeight: {
            type: Number,
            default: 0
        },
        loop: {
            type: Boolean,
            default: false
        },
        autoplay: {
            type: Boolean,
            default: true
        },
        muted: {
            type: Boolean,
            default: true
        },
        validate: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            colorData: {},
            canvasOffset: {
                left: 0,
                top: 0
            },
            canvasMousePos: {
                x: 0,
                y: 0
            }
        }
    },
    created() {
        var that = this
        setTimeout(function() {
            that.playCanvas()
            var mycanvas = that.$refs.mycanvas
            that.canvasOffset = that.offset(mycanvas)
        }, 0)
    },
    methods: {
        playCanvas() {
            var mycanvas = this.$refs.mycanvas
            var myvideo = this.$refs.myvideo
            var context = mycanvas.getContext("2d")
            context.drawImage(myvideo, 0, 0, this.videoWidth, this.videoHeight)
            this.colorData = this.getPixelColor(mycanvas, this.canvasMousePos.x, this.canvasMousePos.y)
            this.$emit('onVideoHover', this.colorData)
            requestAnimationFrame(this.playCanvas)
        },
        tranceCanvas(e) {
            var canvasX = Math.floor(e.pageX - this.canvasOffset.left)
            var canvasY = Math.floor(e.pageY - this.canvasOffset.top)
            this.canvasMousePos.x = canvasX
            this.canvasMousePos.y = canvasY
        },
        clickCanvas(e) {
            this.$emit('onVideoClick', this.colorData)
        },
        offset(obj) {
            var top = 0,
                left = 0
            if (obj) {
                while (obj.offsetParent) {
                    top += obj.offsetTop
                    left += obj.offsetLeft
                    obj = obj.offsetParent
                }
            }
            return {
                top: top,
                left: left
            }
        },
        getPixelColor(canvas, x, y) {
            var thisContext = canvas.getContext("2d")
            var imageData = thisContext.getImageData(x, y, 1, 1)
            var pixel = imageData.data
            var r = pixel[0]
            var g = pixel[1]
            var b = pixel[2]
            var a = pixel[3] / 255
            a = Math.round(a * 100) / 100
            var rHex = r.toString(16)
            r < 16 && (rHex = "0" + rHex)
            var gHex = g.toString(16)
            g < 16 && (gHex = "0" + gHex)
            var bHex = b.toString(16)
            b < 16 && (bHex = "0" + bHex)
            var rgbaColor = "rgba(" + r + "," + g + "," + b + "," + a + ")"
            var rgbColor = "rgb(" + r + "," + g + "," + b + ")"
            var hexColor = "#" + rHex + gHex + bHex
            return {
                rgba: rgbaColor,
                rgb: rgbColor,
                hex: hexColor,
                r: r,
                g: g,
                b: b,
                a: a
            }
        },
    },
}
</script>
