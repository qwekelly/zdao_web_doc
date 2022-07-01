### 背景

有时前端代码上线时，不确定前端代码是否最新的代码包。
导致一些问题明明本地已经修复，且提交到远端代码库，但是发版之后，问题仍复现。无法定位是前端包未更新导致还是对应bug修复方式有问题。
因此迫切需要一种方式，能够知道当前前端包是最新或者已经已经包含了对应bug的改动记录。

### 方案

居于此，想到拿git 的commit信息作为对应包的版本信息。将commit信息在网页上体现出来。

### 实现

脚本形式：在build前执行，可将此commit信息build进前端包里面。


```js

const commitHash = require('child_process')
  .execSync('git rev-parse --short HEAD')
  .toString().trimRight();

const { writeFileSync, readFileSync } = require('fs');

let indexFile = readFileSync('src/index.html');

/** 在 index.html头部添加 commit sha */
indexFile = `<!-- ${commitHash} -->\n` + indexFile;

writeFileSync('src/index.html', indexFile);

```
