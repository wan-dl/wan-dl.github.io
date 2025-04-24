---
title: 使用jest-image-snapshot对比两张图片或进行截图测试
date: 2025-4-24
categories: NodeJS
desc: 日常使用笔记
---

### 关于jest-image-snapshot

jest-image-snapshot 是一个用于图像快照测试的 Node.js 库，它是基于 Jest 的一个扩展插件。

它可以帮助开发人员对比图像文件，检测在 UI 或图像生成过程中是否发生了意外的变化。

通常用于在以下场景中检测意外的视觉回归：

- 前端组件库的 UI 测试。
- 生成图表、报表或其他图像的应用程序。
- Web 页面截图的自动化测试。

### 主要功能

1. **图像快照对比：**
   - 将当前生成的图像与之前保存的基准图像（snapshot）进行对比。
   - 如果两者不匹配，会生成差异图（diff image），并提示测试失败。

2. **自定义配置：**
   - 可以自定义允许的像素差异阈值（pixel threshold）或百分比阈值（percentage threshold）。
   - 支持设置自定义的快照路径和名称。

3. **与 Jest 的无缝集成：**
   - 像 Jest 的其他快照功能一样使用，利用 Jest 的测试框架和断言语法。

### 安装

```bash
npm install --save-dev jest-image-snapshot
```

### toMatchImageSnapshot 配置选项

`toMatchImageSnapshot` 提供了丰富的配置选项，可通过参数传递：

```javascript
expect(image).toMatchImageSnapshot({
  customSnapshotsDir: './snapshots', // 自定义快照目录
  customDiffDir: './diffs',         // 自定义差异图保存目录
  failureThreshold: 0.01,           // 容许的失败阈值
  failureThresholdType: 'percent',  // 阈值类型：'pixel' 或 'percent'
  diffDirection: 'horizontal',      // 差异图方向：'horizontal' 或 'vertical'
});
```

### 工具集成

jest-image-snapshot 常与以下工具一起使用：

- Puppeteer：用于生成网页截图。
- Playwright：用于跨浏览器的自动化测试。
- Cypress：用于端到端测试中的截图比较。

### 示例：结合 Puppeteer 进行网页截图测试

```javascript
const puppeteer = require('puppeteer');
const { toMatchImageSnapshot } = require('jest-image-snapshot');

expect.extend({ toMatchImageSnapshot });

test('homepage screenshot matches snapshot', async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.goto('https://example.com');

    const screenshot = await page.screenshot();
    expect(screenshot).toMatchImageSnapshot();

    await browser.close();
});
```


### 示例：编写js脚本，传入图片参数，对比两个图片

```
const path = require('path');
const fs = require('fs');
const yargs = require('yargs');
const {
    configureToMatchImageSnapshot
} = require('jest-image-snapshot');

let outputDir = path.join(__dirname, '__diff__');


// 解析命令行参数
let argv = yargs.options({
    imgPath1: { type: 'string', demandOption: false, describe: '原图路径', default: ""  },
    imgPath2: { type: 'string', demandOption: false, describe: '目标图目录', default: ""  },
    outputDir: { type: 'string', demandOption: false, describe: '差异图输出目录', default: outputDir}
}).argv;
console.log("[Args]: ", argv);

expect.extend({
    toMatchImageSnapshot: configureToMatchImageSnapshot({
        customSnapshotIdentifier(args) {
            return args.imgPath2
        },
        // customSnapshotsDir: argv.baseDir,
        customDiffDir: argv.outputDir
    }),
});

/**
 * @description 将图片路径转换为base64
 * @param {Object} imagePath
 * @return {String}
 */
async function imageToBase64(imagePath) {
    try {
        const imageBuffer = fs.readFileSync(imagePath);
        return imageBuffer.toString('base64');
    } catch (error) {
        console.error('Error converting image to Base64:', error.message);
        return null;
    }
};

let rawImageName = path.basename(argv.imgPath, path.extname(argv.imgPath));
// 测试用例执行
describe('', () => {
    it(rawImageName, async () => {
        const image = await imageToBase64(argv.imgPath);
        expect(image).toMatchImageSnapshot();
    })
});
```


调用：

```shell
jest main.test.js --imgPath1=<imgPath1> --imgPath2=<imgPath2> --outputDir=<outputDir>
```