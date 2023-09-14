---
layout: post
title: 修复使用unplugin-auto-import和unplugin-vue-components后tsc-vue报错的问题
---
在使用NaiveUI的过程中，引入了unplugin-auto-import和unplugin-vue-components。
这两个组件能自动引入vue方法和vue组件，提升了开发者体验。

但是在vscode中，源码里未手动引用而直接用的方法和组件还是被标红，提示找不到，  
在build的时候，tsc-vue报错，
```
error TS2304: Cannot find name 'NButton'.
error TS2304: Cannot find name 'ref'.
```

## 解决方法

在tsconfig.json的include属性里加上 auto-imports.d.ts ，就可以解决不能识别vue方法的问题

```javascript
{
  "include": ["env.d.ts", "src/**/*", "src/**/*.vue", "auto-imports.d.ts"],
}

```

但是vue组件不识别的问题，解决起来就没这么直接了  
截止2023-09-14，unplugin-vue-components对在vue sfc中使用tsx的支持还是有问题  
具体讨论可以看下这个合并请求：

[fix: tsx component type declaration #673](https://github.com/unplugin/unplugin-vue-components/pull/673)

通过参考上面这个合并请求里的讨论，我想如果手工创建一个 components-tsx.d.ts 文件，在里面加上全局引用，
并把这个文件配置到 tsconfig.json 的include中，是不就能解决报错问题

试了一下，果然可以。之所以不在原来的 components.d.ts 中加，是因为那个文件在使用的组件变化时会自动重新生成，
手动加入的内容会被覆盖掉。

components.d.ts 文件内容

```javascript
export {}

declare global {
    const NButton: typeof import('naive-ui')['NButton']
}
```

最终的 tsconfig.json

```javascript
{
  "include": ["env.d.ts", "src/**/*", "src/**/*.vue", "auto-imports.d.ts", "components-tsx.d.ts"],
}
```
