## useState

如果 useState 要接的是 number、string 這種純值的原始型別，  
是不需要特別加入型別聲明的，但如果是陣列或物件，  
就要使用泛型（尖括號）來聲明陣列與物件裡面是什麼東西：

```tsx
const [money, setMoney] = useState(0);
// 不用聲明型別

const [list, setList] = useState<ListType>([]);
// 要用泛型聲明型別

type ListType = Array<[string, number]>
// [[string, number]] 也是成立的
 ```

假如 setXXX 方法要傳給子元件時，此方法的定義為 SetStateAction：

```tsx
const Child = ({ value, setValue } : ChildPropsType) => {

	const handleClick = () => {
		setValue((prevState) => prevState + 1)
	}

	return (
		<button onClick={handleClick}></ button>
	)
}

type ChildPropsType = {
	value: number;
	setValue: React.Dispatch<SetStateAction<number>>;
}
```
---

## useRef

和 useState 一樣，綁定的東西是原始型別時不需要特別聲明，
但如果綁定在元素或元件上，就要使用泛型：

```tsx
function Component() {
	const ref = useRef<HTMLElement>(null);

	return <div ref={ref}></div>
}
```
---

## 事件

onClick 等等傳來的 e 需要在函式定義時給定型別，  
如果是像 input 需要取 value 出來的話要再寫泛型，
否則會報錯「不存在 value 這個屬性給你使用」之類的。

此外原本取出 e.target 現在要改寫成 e.currentTarget：

```tsx
const handleChange = (e: React.MouseEvent<HTMLInputElement>) => {
	console.log(e.currentTarget.value);
}

return <input type="text" onChange={() => handleChange()}/>
```
---

## 元件參數 props

在 TypeScript 裡面會嚴格定義元件參數 props，所以沒辦法像以前一樣想傳就傳，
這邊除了寫好 type 再傳入之外，參數很少的狀況下也可以用解構的寫法：

```tsx
const SubComponent = ({ name, gender }: { name: string, gender:string}) => {
	return <div>{name}：{gender}</div>
}

const ParentComponent = () => {
	const name = "Vincent";
	const name = "Male";

	return <SubComponent name={name} gender={gender} />
}
```

---
關聯主題：[[TypeScript 型別定義]], [[主題索引/React]]