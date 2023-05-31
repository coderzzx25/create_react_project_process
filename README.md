# 创建 react 项目 TS 版

```
create-react-app project_name --template typescript
```

保留文件

App.tsx index.tsx react-app-env.d.ts

## 配置项目别名

```
npm install @craco/craco@alpha -D
```

创建 craco.config.js 文件

```
const path = require('path')
const resolve = (dir) => path.resolve(__dirname, dir)

module.exports = {
    webpack: {
        alias: {
            '@': resolve('src'),
        },
    },
}
```

修改 tsconfig.json 文件

```
{
    "compilerOptions": {
        "target": "es5",
        "lib": ["dom", "dom.iterable", "esnext"],
        "allowJs": true,
        "skipLibCheck": true,
        "esModuleInterop": true,
        "allowSyntheticDefaultImports": true,
        "strict": true,
        "forceConsistentCasingInFileNames": true,
        "noFallthroughCasesInSwitch": true,
        "module": "esnext",
        "moduleResolution": "node",
        "resolveJsonModule": true,
        "isolatedModules": true,
        "noEmit": true,
        "jsx": "react-jsx",
        // 添加
        "baseUrl": ".",
        "paths": {
            "@/*": ["src/*"]
        }
        //添加
    },
    "include": ["src"]
}

```

修改 package.json 文件

```
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```

### 配置项目代码规范

创建.editorconfig 文件

```
# http://editorconfig.org

root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```

安装 prettier

```
npm install prettier -D
```

创建.prettierrc 文件

\* useTabs：使用 tab 缩进还是空格缩进，选择 false；

\* tabWidth：tab 是空格的情况下，是几个空格，选择 2 个；

\* printWidth：当行字符的长度，推荐 80，也有人喜欢 100 或者 120；

\* singleQuote：使用单引号还是双引号，选择 true，使用单引号；

\* trailingComma：在多行输入的尾逗号是否添加，设置为 `none`；

\* semi：语句末尾是否要加分号，默认值 true，选择 false 表示不加；

```
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "none",
  "semi": false
}
```

创建.prettierignore 忽略文件

```
/dist/*
.local
.output.js
/node_modules/**

**/*.svg
**/*.sh

/public/*
```

配置一次性修改命令

在 package.json 中配置

```
"prettier" : "prettier --write ."
```

安装 eslint

```
npm install eslint -D
```

配置 eslint

```
npx eslint --init
```

![image-20230530093800651](/eslint.config.png)

.eslintrc.js 报错

```
修改文件
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true // 添加
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended'
  ],
  overrides: [],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {}
}
```

连接 prettier+eslint

```
npm i eslint-plugin-prettier eslint-config-prettier -D
```

修改 eslinrc.js 文件

```
extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended'//加
  ],
```

### 使用 less

```
npm install craco-less@2.1.0-alpha.0
```

修改 craco.config.js 文件

```
const path = require('path')
const resolve = (dir) => path.resolve(__dirname, dir)

const cracoLess = require('craco-less')//添加

module.exports = {
		//添加
    plugins: [
      {
        plugin: cracoLess
      }
    ],
    webpack: {
        alias: {
            '@': resolve('src'),
        },
    },
}
```

### 项目文件结构划分

assets 静态文件资源

base-ui 二次封装 ui 框架组件

components 通用组件

hooks 封装的 hooks

router 项目路由

service 项目网络请求

store 项目全局状态数据

utils 项目工具

views 项目页面

### css in js

使用 styled-components

```
npm install styled-components -D
```

### 使用 mui 框架

```
npm install @mui/material @emotion/styled
```

### 样式重制

normailize.css

reset.less

### 连接远程仓库

```
git init
git add .
git commit -m '备注信息'
git remote add origin 地址
git push -u origin 默认分支
```

### 项目路由配置

```
npm install react-router-dom
```

在 router 创建 index.tsx 文件

```
import React,{ lazy } from 'react'
// 路由重定向
import { Navigate } from 'react-router-dom'
import type {RouteObject} from 'react-router-dom'
// 懒加载
const Test = lazy(()=> import('@/views/test'))

const routes: RouteObject[] = [
	{
		path:'/',
		element: <Navigate to='路由' />
	},
	{
		path:'/',
		element: <Test/>
	}
]
```

修改 index.ts 文件

```
import { HashRouter } from 'react-router-dom'

root.render(
		<HashRouter>
			<App/>
		</HashRouter>
)
```

修改 App.tsx 文件

```
import React,{ Suspense } from 'react'
import { useRoutes } from 'react-router-dom'
import routes from '@/router'

<div className='App'>
  // 防止路由懒加载白屏报错
	<Suspens fallback='loading...'>
	  {useRoutes(routes)}
  </Suspens>
</div>
```

### 项目状态管理

```
npm install @reduxjs/toolkit react-redux
```

在 store 中创建 index.ts 文件

```
import { configureStore } from '@reduxjs/tookit'

const store = configureStore({
	reducer:{}
})

export default store
```

修改 index.tsx 文件

```
import { Provider } from 'react-redux'
import store from '@/store'

root.render(
  <Provider store={store}>
		<HashRouter>
			<App/>
		</HashRouter>
  </Provider>
)
```

在 store 中创建 modules 文件夹

```
// test.ts
import { createSlice,PayloadAcion } from '@reduxjs/tookit'

const testSlice = createSlice({
	// 名字
	name:'test',
	// 默认数据
	initialState:{
		count:100
	},
	// 数据方法
	reducers:{
		ChangeCount(state,{ payload }:PayloadAcion<string>){
			state.count = payload
		}
	}
})

export default testSlice.reducer
```

修改 store 中 index.ts 文件

```
import { configureStore } from '@reduxjs/tookit'
import { useSelector,TypedUseSelectorHook,useDispatch,shallowEqual } from 'react-redux'
import testReducer from './modules/test'

const store = configureStore({
	reducer:{
		test:testReducer
	}
})

// 封装数据类型
type GetStateFnType = typeof store.getState
type IRootState = ReturnType<GetStateFnType>
type DispatchType = typeof store.dispatch
// 导出封装数据类型hook
export const useAppSelector:TypedUseSelectorHook<IRootState> = useSelector
// 导出封装方法hook
export const useAppDispatch:()=> DispatchType = useDispatch
// shallowEqual数据没有修改的情况下不更新
export const useAppShallowEqual = shallowEqual

export default store
```

在 App.tsx 中获取全局数据以及方法

```
import { useSelector } from 'react-redux'
import { useAppSelector,useAppDispatch,useAppShallowEqual } from '@/store'

function App(){
  // hooks 需要在函数顶层使用
  const {count} = useAppSelector((state)=>{
    count:state.test.count
  },useAppShallowEqual)
  const dispatch = useAppDispatch()
  // 修改count方法
  handleChangeCount(){
  	dispatch(ChangeCount(1000))
  }
	return(
		<>
			{count} //100
			<button onClick={()=>{handleChangeCount}}>修改count</button>
		</>
	)
}
```

### 网络请求

```
npm install axios
```

在 service 拉取代码

```
git clone https://github.com/coderzzx25/TS_Axios.git
```
