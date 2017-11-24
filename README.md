# video-color-picker
a tool to pick up the Color Value  within any frame from a HTML5 Video . support both Vue.js and jQuery

[view demo](https://wendychengc.github.io/video-color-picker/demo/demo.html)

video resource used in this demo comes from threejs.org/examples/
noticed that your video resource host need to allow CORS , otherwise you cannot use getPixelColor on your canvas, and the project won't work.

use as jQuery plugin
```javascript
var videoobj = $("#videodom").colorvideo({
	videoUrl: "https://threejs.org/examples/textures/sintel.mp4",
	videoWidth: 480,
	videoHeight: 204,
	onVideoClick: function(e, colorData) {
          $("#colorrgb").html(colorData.rgb.toString())
          $("#colorhex").html(colorData.hex)
          $("#colorbox").css("background", colorData.hex)
	},
	onVideoHover: function(e, colorData) {
          // if you need to get color data when mouseover, enable this
          // $("#colorrgb").html(colorData.rgb.toString())
          // $("#colorhex").html(colorData.hex)
          // $("#colorbox").css("background", colorData.hex)
	}
})

$("#playbtn").click(function() {
	videoobj.play()
})
$("#pausebtn").click(function() {
	videoobj.pause()
})
```

use as Vue component
```javascript
<color-video
      videoWidth=480
      videoHeight=204
      ref="colorvideo"
      videoUrl='https://threejs.org/examples/textures/sintel.mp4'
      :autoplay=false
      @onVideoClick="myclickhandler"
      @onVideoHover="myhoverhandler">
</color-video>
```
