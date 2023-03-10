# x6-graph

封装x6为组件使用

<a href="https://www.npmjs.com/package/x6-graph"><img alt="NPM Package" src="https://img.shields.io/npm/v/x6-graph.svg?style=flat-square"></a>
![npm bundle size](https://img.shields.io/bundlephobia/minzip/x6-graph?style=flat-square)
![npm](https://img.shields.io/npm/dm/x6-graph?style=flat-square)
<a href="/LICENSE"><img src="https://img.shields.io/github/license/lloydzhou/x6-graph?style=flat-square" alt="MIT License"></a>

## install
```
npm install x6-graph
yarn add x6-graph
```

## online demos
### react
[react demo](https://codesandbox.io/s/antv-x6-react-graph-demo-6ere13)

[mindmap demo](https://codesandbox.io/s/x6-hooks-react-mindmap-demo-2t6954?file=/src/App.js)

### vue
[vue demo](https://codesandbox.io/s/x6-graph-demo-qq5tds?file=/src/App.vue)


## examples
> x6-graph子组件的模式对业务逻辑做拆分组合（也可以用这个模式抽象一些常用的组件）例如：


### react
```
import { Graph, useGraphInstance } from 'x6-graph/react'

const GraphBehavior = () => {
  const graph = useGraphInstance()
  // TODO 这里拿到graph对象处理自己的逻辑（例如使用后端数据初始化画布，增加事件监听...）
  return null
}

function App() {
  const gRef = useRef<X6.Graph | null>(null)
  // 1. Graph组件支持多实例；2. 父组件传递ref
  return (
    <div className="App">
      <Graph grid width={800} height={600} ref={gRef}>
        <GraphBehavior />
      </Graph>
    </div>
  )
}
```

将x6-plugin-history抽象成组件使用
```
import { History } from "@antv/x6-plugin-history";
import { Graph, useGraphInstance } from 'x6-graph/react'

const HistoryPlugin = () => {
  const graph = useGraphInstance()
  useEffect(() => {
    const history = new History({
      enabled: true,
    })
    graph.use(history)
    return () => {
      history.dispose()
    }
  }, [])
  return null
}

// 使用
<Graph grid width={800} height={600} ref={gRef}>
  <HistoryPlugin />
</Graph>
```

### vue

```
import { Graph, useGraphInstance, contextSymbol } from 'x6-graph/vue'

const GraphBehavior = defineComponent({
  name: 'GraphBehavior',
  inject: [contextSymbol],
  setup() {
    const { graph } = useGraphInstance()
    onMounted(() => {
      // TODO 这里拿到graph对象处理自己的逻辑
      // （例如使用后端数据初始化画布，增加事件监听...）
    })
    onUnmounted(() => {
      // 组件卸载的时候，移除监听等
    })
    return () => null
  }
})

export default defineComponent({
  components: {
    Graph,
    // 可以将不同的业务逻辑拆分到不同的组件里面写成多个Behavior
    GraphBehavior,
  }
})


<template>
  <Graph grid snapline keyboard clipboard :width="600" :height="400">
    <GraphBehavior />
  </Graph>
</template>

```

