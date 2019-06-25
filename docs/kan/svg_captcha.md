# 关于 svg-captcha


在公司的代码中需要使用验证码. 所以在github里找了一个关注高的试了一波。
使用简单，功能也不错。
于是决定看一看他的代码.

1. 主函数

```
function create (width, height, options) {
    //默认值options
	const userOption = getOption(options);
	
	//生成text
	const text = random.captchaText(userOption);
	
	//获取svg 数据
	const data = createCaptcha(text, userOption);

	return {text, data};
};
```

主函数很简单，最神奇的问题就是 `width` 和 `height` 根本没用，是 `options` 传入的参数。
```
function getOption(width, height, userOption) {
	userOption = userOption || {};

	return Object.assign({}, defaultOption, userOption, {width, height});
}

const defaultOption = {
	background: '',
	charPreset: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789',
	size: 4,
	noise: 1
};
```

也就是说 传入`width` 和 `height` 根本无用。
接下来就是 `random.captchaText(userOption);`，生成文字.

```
exports.captchaText = function (options) {
	const size = options.size;
	const chars = options.charPreset;
	let i = -1;
	let out = '';
	const len = chars.length - 1;

	while (++i < size) {
		out += chars[randomInt(0, len)];
	}

	return out;
};
```
很简单，就是根据 `size`来随机选择 `chars` 里面的文字，然后返回.
如果要修改的话。

1. 比如需要中文.  这个很简单，只需要把中文传入 `options.charPreset` 不用改代码都能做到.
2. 需要连续的中文，比如成语。  这个可以把 `chars` 改为中文数组即可。
3. 比如有些倒着的中文呢？ 类似知乎.  这个目前还木有想到，需要用到后面的opentype.js与原生js结合

接下来就是生成图片
```
const createCaptcha = function (text, options) {
	const width = options.width;
	const height = options.height;
	const noise = options.noise;
	const background = options.background;

	const bgRect = background ?
		`<rect width="100%" height="100%" fill="${background}"/>` : '';
	const paths = [].concat(getLineNoise(width, height, noise, background))
			.concat(getText(text, width, height, options))
			.join('');
	const start = `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}">`;
	const xml = `${start}${bgRect}${paths}</svg>`;

	return xml;
};
```
首先根据 `options` 来生成基本的参数。
然后创建 **svg** 基本的架子。开始创建干扰的横线 `getLineNoise(width, height, noise, background)`

```
function getLineNoise (width, height, noise, background) {
	const noiseLines = [];
	let i = -1;
	while (++i < noise) {
		const start = `${random.int(1, 21)} ${random.int(1, height - 1)}`;
		const end = `${random.int(width - 21, width - 1)} ${random.int(1, height - 1)}`;
		const mid1 = `${random.int((width / 2) - 21, (width / 2) + 21)} ${random.int(1, height - 1)}`;
		const mid2 = `${random.int((width / 2) - 21, (width / 2) + 21)} ${random.int(1, height - 1)}`;
		const color = random.invertColor(background);
		noiseLines.push(`<path d="M${start} C${mid1},${mid2},${end}" stroke="${color}" fill="none"/>`);
	}

	return noiseLines;
};
```
这个干扰比较简单。
创建 `svg path` 使用贝塞尔弧线。 其实原有代码的弧线其实弧度比较小。 这个可以优化下。
`svg path` 最多支持3阶贝塞尔弧线。
当然**贝塞尔弧线**的算法其实不是很懂的. 
有兴趣可以看一下 [贝塞尔弧线](https://www.cnblogs.com/hnfxs/p/3148483.html)


现在加入了干扰弧线以后，接下来就是生成字体。
这里 `svg-captcha` 使用了 `opentype.js`. 他可以将字体读出来并且转换为 **svg path** 

```
app.get("/img/:txt", (req, res) => {

	if(!req.params.txt) {
		res.send("param 404");
		return;
	}

    let font = opentype.loadSync("./Comismsh.ttf");
    let path = font.getPath(req.params.txt, 45, 45, 72);

    let svg = `<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    	<path d="${path.toPathData()}" />
    	</svg>
    `;

    res.type("svg");
    res.send(svg);
});

```
我在测试的过程中写了一个及其简单的方法。 
可以实现一个你传入文字，他输出`svg`的方法, 不过没有那么简单。 
`svg-captcha` 验证码肯定不能像我这样直接方正的排布，所以要东倒西歪。

```
const createCaptcha = function (text, options) {
	const width = options.width;
	const height = options.height;
	const noise = options.noise;
	const background = options.background;

	const bgRect = background ?
		`<rect width="100%" height="100%" fill="${background}"/>` : '';
	const paths = [].concat(getLineNoise(width, height, noise, background))
			.concat(getText(text, width, height, options))
			.join('');
	const start = `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}">`;
	const xml = `${start}${bgRect}${paths}</svg>`;

	return xml;
};



function rndPathCmd(cmd) {
	const r = (Math.random() * 0.2) - 0.1;

	switch (cmd.type) {
		case 'M': case 'L':
			cmd.x += r;
			cmd.y += r;
			break;
		case 'Q': case 'C':
			cmd.x += r;
			cmd.y += r;
			cmd.x1 += r;
			cmd.y1 += r;
			break;
		default:
			// Close path cmd
			break;
	}

	return cmd;
}

module.exports = function (char, x, y, fontSize, font) {
	const fontScale = fontSize / font.unitsPerEm;

	const glyph = font.charToGlyph(char);
	const width = glyph.advanceWidth ? glyph.advanceWidth * fontScale : 0;
	const left = x - (width / 2);

	const height = (font.	 + font.descender) * fontScale;
	const top = y + (height / 2);
	const path = glyph.getPath(left, top, fontSize);
	// Randomize path commands
	path.commands.forEach(rndPathCmd);

	const pathData = path.toPathData();

	return pathData;
};

```

算出间距，然后加入参数东倒西歪。加上之前的干扰的**贝塞尔弧线**，加上一些额外参数，比如背景颜色，加载其他字体等等，就完成了这个`svg-captcha`.


以上



