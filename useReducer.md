1. Define the Reducer Function

```js
const counterReducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT_BY_AMOUNT":
      return { count: state.count + action.payload }; // Use payload
    default:
      return state;
  }
};
```

2.  Initialize useReducer in a Component

```js
import { useReducer } from "react";

const initialState = { count: 0 };

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, initialState);

  return (
    <div>
      <h2>Count: {state.count}</h2>
      <button
        onClick={() => dispatch({ type: "INCREMENT_BY_AMOUNT", payload: 5 })}
      >
        🔼 Increment by 5
      </button>
    </div>
  );
}

export default Counter;
```
