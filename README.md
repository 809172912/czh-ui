# czh-ui

## 使用官方的 vue-cli 初始化一个 Vue 项目
```
npm install -g @vue/cli
```

## 初始化项目以后修改package.json
```
将"private"字段改为false（意思是公开的npm包，true为私有的npm包，需要付费）
添加"main"字段为打包后引用的文件路径，例如"main": "./dist/czhui.common.js"
添加"files"字段，值为需要导出的文件，例如"files": ["dist/*", "src/*", "public/*", "*.json", "*.js"]
添加scripts打包命令，"build-bundle": "vue-cli-service build --target lib --name czhui ./src/components/index.js"
注意：scripts命令中czhui是自定义的打包后的文件名，./src/components/index.js是打包入口
```

## 打包项目
```
npm run build-bundle
```

## 发布项目到npm包
```
npm publish
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
