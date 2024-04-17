# useCallback

useCallback和useEffect有一个相似之处，就是需要传入一个依赖项，useCallback只有当依赖项发生变化的时候，就会返回一个新的函数，通常搭配React.memo使用

useCallback通常和React.memo一起使用来避免子组件不必要的重新渲染，在js中，两个代码一样的函数是不相等的，那么如果一个组件的其中一个参数是函数，即使这个组件被React.memo包裹起来，也依旧会被更新，这个时候就需要用到useCallback方法，当依赖项没有发生变化，就不会返回一个新的函数，这样在使用React.memo的时候就可以达到不重新渲染的效果，使用代码如下：

```
export default function App() {
  const [count2, setCount2] = useState(0);
  const handleClickButton2 = useCallback(() => {
    setCount2(count2 + 1);
  }, [count2]);
  return (
    <div>
      <div>
        <Button onClickButton={handleClickButton2}>Button2</Button>
      </div>
    </div>
  );
}
```

这里假设```Button```组件是自定义组件，使用```React.memo```方法包裹起来。

注意，在这段代码里，当点击按钮，调用```ClickButton```方法时候，具体的函数会被执行，也就是说```count2```的值会改变，如果```useCallback```的依赖项是一个空数组，在调用方法的时候，虽然```count2```的值依旧改变，但是```handleClickButton2```方法不会改变，也就是说不会触发重新渲染。