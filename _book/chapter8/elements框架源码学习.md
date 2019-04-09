package.json
编译入口
来看看编译的入口
 "scripts": {
    # 安装依赖
    "bootstrap": "yarn || npm i",
    # 构建文件
    "build:file": "node build/bin/iconInit.js & node build/bin/build-entry.js & node build/bin/i18n.js & node build/bin/version.js",
    # 构建样式
    "build:theme": "node build/bin/gen-cssfile && gulp build --gulpfile packages/theme-chalk/gulpfile.js && cp-cli packages/theme-chalk/lib lib/theme-chalk",
    # 构建工具
    "build:utils": "cross-env BABEL_ENV=utils babel src --out-dir lib --ignore src/index.js",
    # 构建umd
    "build:umd": "node build/bin/build-locale.js"
  }

  这里我们以看源码的角度，先了解构建文件命令。其实就是 
  node 执行了几个 js 脚本。我们深入看下 iconInit、 build-entry、 i18n、 version 
  这些脚本文件。

// build/bin/build-all.js
'use strict';

const components = require('../../components.json');
const execSync = require('child_process').execSync;
const existsSync = require('fs').existsSync;
const path = require('path');

let componentPaths = [];

delete components.index;
delete components.font;

// 遍历 components 的 key，找到相应 component 的路径，将路径保存到 componentPaths 数组
Object.keys(components).forEach(key => {
  const filePath = path.join(__dirname, `../../packages/${key}/cooking.conf.js`);

  if (existsSync(filePath)) {
    componentPaths.push(`packages/${key}/cooking.conf.js`);
  }
});

// pathA,pathB,pathC
const paths = componentPaths.join(',');
// 拼接为 shell 命令，并调用 execSync 方法执行。
const cli = path.join('node_modules', '.bin', 'cooking') + ` build -c ${paths} -p`;

execSync(cli, {
  stdio: 'inherit'
});