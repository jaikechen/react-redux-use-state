# react-redux-use-state
It implements redux version of useState hook, leveraging redux functionalities, the state can be shared between components 

# Why use this hook?

The useState hook can not share state among multiple components.
The useContext hook has performance issue.
So redux seems to be a good choice for sharing state between components.

# Installation via NPM
```npm i react-redux-use-state```
## if you have redux in your project, 
import globalStateReducer , then added them to your createStore function
```typescript
import { globalStateReducer} from 'react-redux-use-state';
export const rootStore = createStore(
    combineReducers({globalStateReducer}),
    compose(applyMiddleware())
)

```
## if you don't have redux in your project 
import rootStore, then wrap you component in the redux provider
```typescript
import { rootStore } from 'react-redux-use-state';
import { Provider } from 'react-redux';

ReactDOM.render(
     <Provider store={rootStore}>
        <App />
     </Provider> ,
  document.getElementById('root')
);
```
# hook definition
``` typescript
export function useGlobalState<T>(key: string, initValue: any = undefined) 
```
the hook returns an array, the first element is the state of the hook, the second element is a function, call the function to set the state.
``` typescript
[T, (val: T) => {}]
```
you can set many global state, the hook use keys to find the correspond state

# Examples
```typescript
export function CatEdit (){
    const [items, setItems]= useGlobalState<string[]>('cat',[])
    function addCat(){
        setItems([...items, 'cat' + items.length])
    }
    return <Fragment>
        {
            <button onClick={addCat}>add cat</button>
        }
    </Fragment>
}

export function CatList (){
    const [items]= useGlobalState<string[]>('cat')
    return <Fragment>
        {
            items?.map(x=> <div key={x}>{x}</div>)
        }
    </Fragment>

```