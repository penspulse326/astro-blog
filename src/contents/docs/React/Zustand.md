用過 zustand 後幾乎不會想要再使用 redux 了...  
開箱即用的語法和 pinia 相當類似，非常容易撰寫，  
也不需要繁瑣的前期設定！

## store

命名慣例是 `use[...]Store.ts`。

```ts
interface CounterState {
  count: number;
  increment: () => void;
  decrement: () => void;
}

const useCounterStore = create<CounterState>((set) => ({
  // states
  count: 0,

  // actions
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));
```

在元件中使用：

```ts
function App() {
  // 不建議取出整個 state 再解構
  const { count, increment, decrement } = useCounterStore();

  // 建議只針對需要的狀態做訂閱
  const count = useCounterStore(state => state.count);
  const increment = useCounterStore(state => state.increment);
  const decrement = useCounterStore(state => state.decrement);

  return (
    <>
      count: {count}
      <button type="button" onClick={increment}>increment</button>
      <button type="button" onClick={decrement}>decrement</button>
    </>
  )
}
```
---

## set

create 時傳入的 set 有 shallow merge 的機制，  
所以我們在更新特定的 state 時不需要展開整個 state：

```ts
setUserName: (newName: string) => set(() => ({ 
  name: newName 
}));

// 不用展開 state
// setUserName: (newName: string) => set((state) => ({
//   ...state,
//   name: newName 
// }));
```
---

## reset

可以在 store 外層宣告 `initialValue` 或是利用函式參數來重置整個 state：

```ts
interface CounterState {
  count: number;
}

const initialValue: CounterState = {
  count: 0;
}

const useCounterStore = create<CounterState>((set) => ({
  //...

  reset: () => {
    set(initailValue);
  }
}));
```

我在使用 AI 時跑出了謎之語法：`store.getState().reset()`，  
但這個方法與