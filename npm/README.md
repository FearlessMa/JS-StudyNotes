# NPM 

## npm script 执行多条

* 串联执行：&& 连接多条语，执行过程中一个出错就会抛出错误，后续不会继续执行。

```json
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"lint:js": "eslint *.js",
"lint:css": "stylelint *.less",
"lint:json": "jsonlint --quiet *.json",
"lint:markdown": "markdownlint --config .markdownlint.json *.md",
"test": "mocha tests/",
"testAll": "npm run lint:js && npm run lint:css && npm run lint:json && npm run lint:markdown && mocha /test"
},
```

* 并行执行： & 连接多条语句，执行过程中的错误等到所有script脚本执行完毕后抛出。

```json
{
    "testAll1": "npm run lint:js & npm run lint:css & npm run lint:json & npm run lint:markdown & mocha /test"
}
```

* 串并行共同运行： 在lint检查完毕后运行test测试，使用&并行多个lint，最后&&串行test。

```json
{
    "testAll1": "npm run lint:js & npm run lint:css & npm run lint:json & npm run lint:markdown && mocha /test"
}
```

* 如果使用-watch参数持续运行，不能串并行混用

```json
{
    "testAll1": "npm run lint:js & npm run lint:css & npm run lint:json & npm run lint:markdown & mocha -w /test"
}
```

## npm-run-all 包

## npm script 传递参数

* 传递额外参数： --fix 参数前面的 -- 分隔符，意指要给 npm run lint:js 实际指向的命令传递额外的参数。

```json
{
    "lint:js": "eslint *.js",
    "lint:js:fix": "npm run lint:js -- --fix",
}
```

## npm script 添加注释

* 方法1："//":"注释说明"

```json
{
    "//": "运行eslint检查js代码",
    "lint:js": "eslint *.js",
    "lint:js:fix": "npm run lint:js -- --fix",
}
```

* 方法2： 另外一种方式是直接在 script 声明中做手脚，因为命令的本质是 shell 命令（适用于 linux 平台），我们可以在命令前面加上注释

```json
{
    "lint:js": "# 运行eslint检查js代码 \n eslint *.js",
    "lint:js:fix": "npm run lint:js -- --fix",
}
```