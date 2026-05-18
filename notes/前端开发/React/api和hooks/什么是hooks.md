# 什么是hooks

### 发展
- 在react16.8之后，react新增了hooks
- vue3.0相比于vue2.0，使用了Composition API，也是为了更好的支持hooks

### hook函数和普通函数的区别
普通函数通常是一些纯函数，如果需要保存状态，需要依赖外部变量或者使用闭包手动维护，而hooks方法借用了react或者vue的响应式机制，可以自动保存状态，并且参与到组件生命周期中

### 实现一个鼠标位置的hooks，就叫useMousePosition

- react实现

```javascript
import { useState, useEffect } from 'react'

function useMousePosition() {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  useEffect(() => {
    const handleMouseMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY })
    }

    document.addEventListener('mousemove', handleMouseMove)

    return () => {
      document.removeEventListener('mousemove', handleMouseMove)
    }
  }, [])
  return position
}

export default useMousePosition

```

- vue实现

```javascript
import { ref, onMounted, onUnmounted } from 'vue'

export default function useMousePosition() {
  const x = ref(0)
  const y = ref(0)

  onMounted(() => {
    const handleMouseMove = (e) => {
      x.value = e.clientX
      y.value = e.clientY
    }

    document.addEventListener('mousemove', handleMouseMove)

    onUnmounted(() => {
      document.removeEventListener('mousemove', handleMouseMove)
    })
  })

  return { x, y }
}

```

### hooks的使用
- hooks只能在组件中使用
- hooks只能在组件的最顶层使用，不能在循环、条件判断或者嵌套函数中使用
- hooks方法中要及时清理副作用（react使用useEffect的return，vue使用onUnmounted），否则会造成内存泄漏
