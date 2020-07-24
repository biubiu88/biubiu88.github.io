## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/biubiu88/biubiu88.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/biubiu88/biubiu88.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>HTML5仪表盘动画代码 - 源码之家</title>
<meta charset="utf-8" />

<style>
	html, body, div, canvas, a {
		margin: 0px;
		padding: 0px;
	}

	#app {
		width: 550px;
		height: 760px;
		margin: 0px auto;
		padding: 32px;
	}
</style>

</head>
<body>

<div id="app">
	<canvas id="canvas" width="262" height="262"></canvas>
</div>

<script type="text/javascript">
	function Dot() {
		this.x = 0;
		this.y = 0;
		this.draw = function (ctx) {
			ctx.save();
			ctx.beginPath();
			ctx.fillStyle = 'rgba(255, 255, 255, 1)';
			ctx.arc(this.x, this.y, 5, 0, Math.PI * 2, false);
			ctx.fill();
			ctx.lineWidth = 1;
			ctx.strokeStyle = "rgba(246, 5, 51, 1)";
			ctx.stroke();
			ctx.restore();

			ctx.save();
			ctx.beginPath();
			ctx.fillStyle = 'rgba(246, 5, 51, 1)';

			ctx.arc(this.x, this.y, 3, 0, Math.PI * 2, false);
			ctx.fill();

			ctx.restore();
		};
	}
	function innerDot() {
		this.x = 0;
		this.y = 0;

		this.draw = function (ctx) {
			ctx.save();
			ctx.beginPath();
			ctx.fillStyle = 'rgba(91,66,214,.12)';

			ctx.arc(this.x, this.y, 6, 0, Math.PI * 2, false);
			ctx.fill();

			ctx.restore();
		};
	}
	function setColorTick() {
		this.draw = function (ctx) {
			ctx.save();

			for (var i = 0; i <= this.index; i++) {
				var strokeLineWidth = 2

				if (i == this.index) {
					strokeLineWidth = 4
				}

				ctx.beginPath();
				ctx.lineWidth = strokeLineWidth;
				ctx.strokeStyle = this.gradientColorArr[i];
				ctx.moveTo(113, 0);
				ctx.lineTo(88, 0);
				ctx.stroke();
				ctx.rotate(this.deg1);
			}

			ctx.restore();
		}
	}
	var Gauge = function (options) {
		var properties = {
			canvas: null,
			percent: 0,
			score: 50,
			radius: 121, //外层圆半径
			lineNums: 50, //外环指针线数量
			innerLineNums: 50 * 3 / 2, //内环刻度线数量
			totalScore: 100,
			color: [[54, 63, 255], [134, 37, 168], [252, 3, 44]], //渐变色数组,渐变顺序从左到右
			opacity: 0.6,//渐变色透明度
			colorLineNums: 25, //外环彩色刻度数量 = (分数 / 总分数) * 刻度数 = (score / totalScore) * lineNums

		}
		this.mergeOptions(properties, options);
		this.canvas = options.canvas;
		this._percent = options.percent || 0;
		this.colorLineNums = this.score / 100 * this.lineNums;
		//弧长计算公式是一个数学公式，为L=n（圆心角度数）× π（1）× r（半径）/180（角度制），L=α（弧度）× r(半径) （弧度制）。其中n是圆心角度数，r是半径，L是圆心角弧长。
		//整个运动的角度是（360-120）度，转换成弧度就是12π/9，一共分成了50个分数段，那么每一个分数段就是12π/450 = 2π / 75
		//如需旋转 5 度，可规定下面的公式：5*Math.PI/180。
		this.deg1 = (Math.PI * 12) / (9 * this.lineNums);
		return this;
	}
	Gauge.prototype = {
		mergeOptions: function (defaultOpt, options) {
			var _this = this;
			var list = Object.keys(defaultOpt);

			list.forEach(function (key) {
				_this[key] = typeof options[key] === 'undefined' ? defaultOpt[key] : options[key];
			});
		},
		//彩色刻度线颜色数组
		gradientColorArr: function () {
			var colorArr = [],
			colorObj1 = this.getRGBDiff(this.color[0], this.color[2]),
			colorObj2 = this.getRGBDiff(this.color[1], this.color[2])
			for (var i = 0; i < this.colorLineNums; i++) {
				//计算每一步的hex值
				[sR, sG, sB, startR, startG, startB, hex] = [colorObj1.sR, colorObj1.sG, colorObj1.sB, colorObj1.startR, colorObj1.startG, colorObj1.startB, '']

				if (i > this.colorLineNums / 2) {
					[sR, sG, sB, startR, startG, startB] = [colorObj2.sR, colorObj2.sG, colorObj2.sB, colorObj2.startR, colorObj2.startG, colorObj2.startB]
				}
				hex = this.colorToHex('rgba(' + parseInt((sR * i + startR)) + ',' + parseInt((sG * i + startG)) + ',' + parseInt((sB * i + startB)) + ',' + this.opacity + ')');

				colorArr.push(hex);
			}
			return colorArr;
		},
		getRGBDiff: function (r1, r2) {
			var obj = {
				sR: (r2[0] - r1[0]) / 25,
				//总差值
				sG: (r2[1] - r1[1]) / 25,
				sB: (r2[2] - r1[2]) / 25,
				startR: r1[0],
				startG: r1[1],
				startB: r1[2]
			}
			return obj;
		},
		colorToHex: function (rgb) {
			var _this = rgb;
			var reg = /^#([0-9a-fA-f]{3}|[0-9a-fA-f]{6})$/;
			if (/^(rgb|RGB)/.test(_this)) {
				var aColor = _this.replace(/(?:(|)|rgb|RGB)*/g, "").split(",");
				var strHex = "#";
				for (var i = 0; i < aColor.length; i++) {
					var hex = Number(aColor[i]).toString(16);
					hex = hex < 10 ? 0 + '' + hex : hex; // 保证每个rgb的值为2位
					if (hex === "0") {
						hex += hex;
					}
					strHex += hex;
				}
				if (strHex.length !== 7) {
					strHex = _this;
				}
				return strHex;
			} else if (reg.test(_this)) {
				var aNum = _this.replace(/#/, "").split("");
				if (aNum.length === 6) {
					return _this;
				} else if (aNum.length === 3) {
					var numHex = "#";
					for (var i = 0; i < aNum.length; i += 1) {
						numHex += (aNum[i] + aNum[i]);
					}
					return numHex;
				}
			} else {
				return _this;
			}
		},
		//内部文本
		drawText: function (ctx, process) {
			ctx.save();
			ctx.rotate(210 * Math.PI / 180);
			ctx.fillStyle = '#000';
			ctx.font = '44px Microsoft yahei';
			ctx.textAlign = 'center';
			ctx.textBaseLine = 'top';
			ctx.fillText(process, 0, 10);
			var width = ctx.measureText(process).width;

			ctx.fillStyle = '#000';
			ctx.font = '20px Microsoft yahei';
			ctx.fillText('分', width / 2 + 10, 10);

			ctx.restore();
		},
		render: function () {
			var canvas = this.canvas,
			ctx = canvas.getContext('2d'),
			cWidth = canvas.width,
			cHeight = canvas.height,
			score = this.score,
			radius = this.radius,
			deg1 = this.deg1,
			$this = this;

			//外环动点
			var dot = new Dot(),
			//内环动点
			dot2 = new innerDot(),
			//数字增加速度
			dotSpeed = 0.04,
			//数字增加的值 : deg1:每旋转一个线的弧度,共50根线,数值为100,所以数字速度等于 旋转角度 * 2
			textSpeed = Math.round(dotSpeed * 2 / deg1),
			//外环动点旋转角度
			angle = 0,
			//内环动点旋转角度
			innerAngle = 0,
			//起始分数,数字递增用
			credit = 0,
			colorTick = new setColorTick(),
			colorIndex = 0,
			colorSpeed = dotSpeed / deg1; //彩色刻度速度: 动点旋转速度 / 弧度
			//色彩段数与彩色刻度条保持一致,线条无间隔,所以段数 * 2
			var gradient = ctx.createLinearGradient(0, 0, 100, 0);
			gradient.addColorStop("0", "rgba(252,3,44,.6)");
			gradient.addColorStop("0.5", "rgba(134,37,168,.6)");
			gradient.addColorStop("1.0", "rgba(54,63,255,.6)");
			(function drawFrame() {

				ctx.save();
				ctx.clearRect(0, 0, cWidth, cHeight);
				ctx.translate(cWidth / 2, cHeight / 2);

				//因圆本身缺口为120°,为了让缺口朝正下方,所以旋转角度为150°
				ctx.rotate(150 * Math.PI / 180);

				dot.x = radius * Math.cos(angle);
				dot.y = radius * Math.sin(angle);

				var aim = score * deg1 / 2;

				if (angle < aim) {
					angle += dotSpeed; //动点旋转速度
				}
				dot.draw(ctx);

				//内环动点坐标
				//dot2.x = 81 * Math.cos(innerAngle);
				//dot2.y = 81 * Math.sin(innerAngle);

				//内环 动点无限循环
				//if (true) {
				//    innerAngle += dotSpeed / 2
				//}
				//dot2.draw(ctx);

				if (credit < score - textSpeed) {
					credit += textSpeed;
				} else if (credit >= score - textSpeed && credit < score) {
					credit += 1;
				}
				$this.drawText(ctx, credit);

				//外环渐变线
				ctx.save();
				ctx.beginPath();
				ctx.lineWidth = 2;
				ctx.strokeStyle = gradient;
				ctx.arc(0, 0, radius, 0, angle, false);
				ctx.stroke();
				ctx.restore();
				//
				ctx.save();
				//外环灰色线
				for (var i = 0; i <= $this.lineNums; i++) {
					ctx.beginPath();
					ctx.lineWidth = 2;
					ctx.strokeStyle = 'rgba(155,157,183,1)';
					ctx.moveTo(113, 0);
					ctx.lineTo(88, 0);
					ctx.stroke();
					ctx.rotate(deg1);
				}
				ctx.restore();

				if (colorIndex < score / 2) {
					colorIndex += colorSpeed;
				}
				try {
					colorTick.gradientColorArr = $this.gradientColorArr();
					colorTick.deg1 = deg1;
					colorTick.index = colorIndex;
					colorTick.draw(ctx);
				} catch (e) { }

				if (colorIndex < score / 2) window.requestAnimationFrame(drawFrame);

				// 细分内环刻度线 :  圆线
				//ctx.save();
				//for (var i = 0; i <= $this.innerLineNums; i++) {
				//    ctx.beginPath();
				//    ctx.lineWidth = 2;
				//    ctx.strokeStyle = 'rgba(155,157,183,1)';
				//    ctx.moveTo(82, 0);
				//    ctx.lineTo(80, 0);
				//    ctx.stroke();

				//    //每个点的弧度,360°弧度为2π,即旋转弧度为 2π / 75
				//    ctx.rotate(2 * Math.PI / $this.innerLineNums);
				//}
				//ctx.restore();

				// 内环刻度线
				ctx.save();
				for (var i = 0; i < 6; i++) {
					ctx.beginPath();
					ctx.lineWidth = 2;
					ctx.strokeStyle = 'rgba(155,157,183,1)';
					ctx.moveTo(82, 0);
					ctx.lineTo(78, 0);
					ctx.stroke();

					//每10个点分一个刻度,共5个刻度,旋转角度为deg1 * 10
					ctx.rotate(deg1 * 10);
				}
				ctx.restore();

				//内环数量刻度
				ctx.save();
				ctx.rotate(Math.PI / 2);
				for (i = 0; i < 6; i++) {
					ctx.fillStyle = 'rgba(165,180,198, .4)';
					ctx.font = '10px Microsoft yahei';
					ctx.textAlign = 'center';
					ctx.fillText(20 * i, 0, -65);
					ctx.rotate(deg1 * 10);
				}
				ctx.restore();

				ctx.restore();

			})();
		},
		update: function (value) {
			this.score = value;
			this.render();
		}

	}
</script>
<script>

	var my_canvas = document.getElementById("canvas");
	var gauge = new Gauge({
		"canvas": my_canvas
	})

	// 绘制初始仪表盘初始值0%
	gauge.render();
	setTimeout(function () {
		gauge.update(85);
	}, 2000)
</script>

<div style="text-align:center;">
<p>更多源码：<a href="http://www.mycodes.net/" target="_blank">源码之家</a></p>
</div>
</body>
</html>
