

# Draw-a-pie-chart-by-d3.js-v4


<iframe src="https://kyshel.github.io/GistLive/raw/?id=8cb1aaffc6b7111a9dcf00785e9eb175" scrolling="no" style="height: 300px;width:300px"></iframe>

## Code
    <canvas width="300" height="300" id="pad"></canvas>
    <script type="text/javascript">

    var data = [1, 1, 2, 3, 5, 8, 13];

    var canvas = document.getElementById("pad"),
        context = canvas.getContext("2d");

    var width = canvas.width,
        height = canvas.height,
        radius = Math.min(width, height) / 2;

    var colors = function(){
        return "hsl(" + Math.random() * 360 + ",100%,50%)";
    } 

    var arc = d3.arc()
        .outerRadius(radius - 10)
        .innerRadius(0)
        .context(context);

    var labelArc = d3.arc()
        .outerRadius(radius - 20)
        .innerRadius(radius - 20)
        .context(context);

    var pie = d3.pie();
    var arcs = pie(data);

    // move to center
    context.translate(width / 2, height / 2);

    // pie
    context.globalAlpha = 0.5;
    arcs.forEach(function(d, i) {
      context.beginPath();
      arc(d);
      context.fillStyle = colors();
      context.fill();
    });

    // num
    context.textAlign = "center";
    context.textBaseline = "middle";
    context.fillStyle = "#000";
    arcs.forEach(function(d, i) {
        var c = labelArc.centroid(d);
        context.fillText(data[i], c[0], c[1]);
    });

    </script>
    <style>
        canvas {
            padding: 0;
            margin: auto;
            display: block;
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
        }
    </style>


