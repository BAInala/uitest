<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
    <el-button>hahahfhaufh</el-button>
    <el-button>2222</el-button>
    <canvas id="c">当前浏览器不支持canvas 请升级！</canvas>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  mounted() {
    debugger
     this.$nextTick(() => {
        this.process();

      });
  },
  methods:{
     process:function(){
       let canvas = document.getElementById("c")
        let ctx = canvas.getContext("2d");
        let M = Math
        let Sin = M.sin
        let Cos = M.cos
        let Sqrt = M.sqrt
        let Pow = M.pow
        let PI = M.PI
        let Round = M.round
        let oW = canvas.width = 250
        let oH = canvas.height = 250
        let lineWidth = 2 // 线宽
        let r = (oW / 2) // 大半径
        let cR = r - 10 * lineWidth
        ctx.beginPath()
        ctx.lineWidth = lineWidth

        // 水波动画初始参数
        let axisLength = 2 * r - 16 * lineWidth // Sin 图形长度
        let unit = axisLength / 9 // 波浪宽
        let range = .4 // 浪幅
        let nowrange = range
        let xoffset = 8 * lineWidth // x 轴偏移量
        let data = ~~(80) / 100 // 数据量
        let sp = 0 // 周期偏移量
        let nowdata = 0
        let waveupsp = 0.006 // 水波上涨速度

        // 圆动画初始参数
        let arcStack = [] // 圆栈
        let bR = r - 8 * lineWidth
        let soffset = -(PI / 2) // 圆动画起始位置
        let circleLock = true // 起始动画锁

        // 获取圆动画轨迹点集
        for (var i = soffset; i < soffset + 2 * PI; i += 1 / (8 * PI)) {
            arcStack.push([
                r + bR * Cos(i),
                r + bR * Sin(i)
            ])
        }
        // 圆起始点
        let cStartPoint = arcStack.shift();
        ctx.strokeStyle = "#1c86d1";
        ctx.moveTo(cStartPoint[0], cStartPoint[1]);
        // 开始渲染
        render();

function drawSine() {
    ctx.beginPath();
    ctx.save();
    var Stack = []; // 记录起始点和终点坐标
    for (var i = xoffset; i <= xoffset + axisLength; i += 20 / axisLength) {
        var x = sp + (xoffset + i) / unit;
        var y = Sin(x) * nowrange;
        var dx = i;
        var dy = 2 * cR * (1 - nowdata) + (r - cR) - (unit * y);
        ctx.lineTo(dx, dy);
        Stack.push([dx, dy])
    }
    // 获取初始点和结束点
    var startP = Stack[0]
    var endP = Stack[Stack.length - 1]
    ctx.lineTo(xoffset + axisLength, oW);
    ctx.lineTo(xoffset, oW);
    ctx.lineTo(startP[0], startP[1])
    ctx.fillStyle = "#24CB31";// 填充色
    ctx.fill();
    ctx.restore();
}

function drawText() {
    ctx.globalCompositeOperation = 'source-over';
    var size = 0.4 * cR;
    ctx.font = 'bold ' + size + 'px Microsoft Yahei';
    let txt = (nowdata.toFixed(2) * 100).toFixed(0) + '`%';
    var fonty = r + size / 2;
    var fontx = r - size * 0.8;
    ctx.fillStyle = "#fff"; // 字的颜色 
    ctx.textAlign = 'center';
    ctx.fillText(txt, r + 5, r + 20)
}
//最外面淡黄色圈
function drawCircle() {
    ctx.beginPath();
    ctx.lineWidth = 2;
    ctx.strokeStyle = '#21CA2E';
    ctx.fillStyle = "#fff";
    ctx.arc(r, r, cR + 7, 0, 2 * Math.PI);
    ctx.stroke();
    ctx.restore();
}
//灰色圆圈
function grayCircle() {
    ctx.beginPath();
    ctx.lineWidth = 10;
    ctx.strokeStyle = '#eaeaea';
    ctx.arc(r, r, cR + 1, 0, 2 * Math.PI);
    // ctx.arc(r, r, cR - 5, 0, 2 * Math.PI);
    ctx.stroke();
    ctx.restore();
    ctx.beginPath();
}
//橘黄色进度圈
function orangeCircle() {
    ctx.beginPath();
    ctx.strokeStyle = '#24CB31';
    //使用这个使圆环两端是圆弧形状
    ctx.lineCap = 'round';
    ctx.arc(r, r, cR - 5, 0 * (Math.PI / 180.0) - (Math.PI / 2), (nowdata * 360) * (Math.PI / 180.0) - (Math.PI / 2));
    ctx.stroke();
    ctx.save()
}
//裁剪中间水圈
function clipCircle() {
    ctx.beginPath();
    ctx.arc(r, r, cR - 10, 0, 2 * Math.PI, false);
    ctx.clip();
}
//渲染canvas
function render() {
      ctx.clearRect(0, 0, oW, oH);
      //最外面淡黄色圈
      drawCircle();
      //灰色圆圈  
      grayCircle();
      //橘黄色进度圈
      // orangeCircle(); 
      //裁剪中间水圈  
      clipCircle();
      // 控制波幅
    
      if (data >= 0.85) {
          if (nowrange > range / 4) {
              var t = range * 0.01;
              nowrange -= t;
          }
      } else if (data <= 0.1) {
          if (nowrange < range * 1.5) {
              var t = range * 0.01;
              nowrange += t;
          }
      } else {
          if (nowrange <= range) {
              var t = range * 0.01;
              nowrange += t;                                                                                          
          }
          if (nowrange >= range) {
              var t = range * 0.01;
              nowrange -= t;
          }
      }
      if ((data - nowdata) > 0) {
          nowdata += waveupsp;
      }
      if ((data - nowdata) < 0) {
          nowdata -= waveupsp
      }
      sp += 0.07;
      // 开始水波动画
      drawSine();
      // 写字
      drawText();
      requestAnimationFrame(render)

}
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
