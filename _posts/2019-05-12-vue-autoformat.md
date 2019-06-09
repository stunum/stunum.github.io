---
layout: post
title: vscode进行vue格式化时，会自动补分号和双引号的问题
date: 2019-05-12 18:52:18
tags:  奇技淫巧
---

用vscode来开发vue是一个很不错的选择，特别是安装了一些插件辅助之后，简直如虎添翼。但是由于vue的严格检查模式下，vetur插件的自动格式化会在代码尾部添加分号和把单引号变为双引号，导致出现错误提示！

解决问题的办法就是修改settings.json文件的配置,亲测有效！
```
"vetur.format.defaultFormatterOptions": {
  "prettier": {
    "semi": false,
    "singleQuote": true
  }
}
"javascript.format.insertSpaceBeforeFunctionParenthesis": true,
"vetur.format.defaultFormatter.js": "vscode-typescript",
"eslint.validate": [
  "javascript",
  "javascriptreact",
  "html",
    {
      "language": "vue",
      "autoFix": true
    }
]

```