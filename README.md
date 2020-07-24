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


<html>
   
<head>   
    <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
    <title>Code -by ZhenYu.Sha</title>
    <style type="text/css">
        html, body {
            width: 100%;
            height: 100%;
        }
        body {
            background: #000;
            overflow: hidden;
            margin: 0;
            padding: 0;
        }
    </style>
</head>
   
<body>  
<canvas id="cvs"></canvas>
<script type="text/javascript">
    var cvs = document.getElementById("cvs");
    var ctx = cvs.getContext("2d");
    var cw = cvs.width = document.body.clientWidth;
    var ch = cvs.height = document.body.clientHeight;
    //动画绘制对象
    var requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;
    var codeRainArr = []; //代码雨数组
    var cols = parseInt(cw / 14); //代码雨列数
    var step = 16;    //步长，每一列内部数字之间的上下间隔
    ctx.font = "bold 26px microsoft yahei"; //声明字体，个人喜欢微软雅黑
 
    function createColorCv() {
        //画布基本颜色
        ctx.fillStyle = "#242424";
        ctx.fillRect(0, 0, cw, ch);
    }
 
    //创建代码雨
    function createCodeRain() {
        for (var n = 0; n < cols; n++) {
            var col = [];
            //基础位置，为了列与列之间产生错位
            var basePos = parseInt(Math.random() * 300);
            //随机速度 3~13之间
            var speed = parseInt(Math.random() * 10) + 3;
            //每组的x轴位置随机产生
            var colx = parseInt(Math.random() * cw)
 
            //绿色随机
            var rgbr = 0;
            var rgbg = parseInt(Math.random() * 255);
            var rgbb = 0;
            //ctx.fillStyle = "rgb("+r+','+g+','+b+")"
 
            for (var i = 0; i < parseInt(ch / step) / 2; i++) {
                var code = {
                    x: colx,
                    y: -(step * i) - basePos,
                    speed: speed,
                    //  text : parseInt(Math.random()*10)%2 == 0 ? 0 : 1  //随机生成0或者1
                    text: ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "s", "t", "u", "v", "w", "x", "y", "z"][parseInt(Math.random() * 11)], //随机生成字母数组中的一个
                    color: "rgb(" + rgbr + ',' + rgbg + ',' + rgbb + ")"
                }
                col.push(code);
            }
            codeRainArr.push(col);
        }
    }
 
    //代码雨下起来
    function codeRaining() {
        //把画布擦干净
        ctx.clearRect(0, 0, cw, ch);
        //创建有颜色的画布
        //createColorCv();
        for (var n = 0; n < codeRainArr.length; n++) {
            //取出列
            col = codeRainArr[n];
            //遍历列，画出该列的代码
            for (var i = 0; i < col.length; i++) {
                var code = col[i];
                if (code.y > ch) {
                    //如果超出下边界则重置到顶部
                    code.y = 0;
                } else {
                    //匀速降落
                    code.y += code.speed;
                }
                
                //1 颜色也随机变化
                //ctx.fillStyle = "hsl("+(parseInt(Math.random()*359)+1)+",30%,"+(50-i*2)+"%)"; 
 
                //2 绿色逐渐变浅
                // ctx.fillStyle = "hsl(123,80%,"+(30-i*2)+"%)"; 
 
                //3 绿色随机
                // var r= 0;
                // var g= parseInt(Math.random()*255) + 3;
                // var b= 0;
                // ctx.fillStyle = "rgb("+r+','+g+','+b+")";
 
                //4 一致绿
                ctx.fillStyle = code.color;
 
 
                //把代码画出来
                ctx.fillText(code.text, code.x, code.y);
            }
        }
        requestAnimationFrame(codeRaining);
    }
 
    //创建代码雨
    createCodeRain();
    //开始下雨吧 GO>>
    requestAnimationFrame(codeRaining);
</script>
   
</body>
</html>
