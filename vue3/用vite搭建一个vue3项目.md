# 用vite搭建一个vue3项目
>[开始 | Vite 官方中文文档](https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project)
1. 搭建项目框架：`npm create vite@latest`
	- 起项目名
	- 选择VUE
	- 选择JavaScript
2. 安装依赖：`npm i`
3. 在文件 `package.json` 中：可看出运行项目的命令是 `npm run dev`
	- 可自定义运行单个文件的命令：如运行根目录下的 `index.js` 文件，可运行 `npm run sh`，则文件内容会输出到终端
```json
"scripts": {
	"dev": "vite",
	"build": "vite build",
	"preview": "vite preview"
	//可自由定义运行命令
	"sh": "node index.js"
},
```
4. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDA0Mzg1MzUwXX0=
-->