1. npm install @reduxjs/toolkit react-redux

2. Create a Redux Store

```js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./counterSlice"; // Import the reducer

export const store = configureStore({
  reducer: {
    counter: counterReducer, // Register the reducer
  },
});

export default store;
```

3. Create a Slice (State + Reducers) (counterSlice.js)

```js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1; // No need for return (Immer handles immutability)
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

// Export actions
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

// Export reducer
export default counterSlice.reducer;
```

4. Provide Store to the React App (index.js or main.jsx)

```js
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import App from "./App";
import { store } from "./store"; // Import the store

ReactDOM.createRoot(document.getElementById("root")).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

5. Use Redux in Components (Counter.js)

```js
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement, incrementByAmount } from "./counterSlice";

export default function Counter() {
  const count = useSelector((state) => state.counter.value); // Get state
  const dispatch = useDispatch(); // Get dispatch function

  return (
    <div>
      <h2>Counter: {count}</h2>
      <button onClick={() => dispatch(increment())}>➕ Increment</button>
      <button onClick={() => dispatch(decrement())}>➖ Decrement</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>
        🔼 Increment by 5
      </button>
    </div>
  );
}
```

## Note -

    ** dispatch(incrementByAmount(5)) passes 5 as the payload to the reducer.**
    ** state.value += action.payload updates the state with the given amount.**

🎯 Bonus: Redux with API Calls (RTK Query)
Redux Toolkit also provides RTK Query, which simplifies API requests.

1. Install RTK Query

`npm install @reduxjs/toolkit react-redux`

2. Create an API Slice (apiSlice.js)

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: "https://jsonplaceholder.typicode.com",
  }),
  endpoints: (builder) => ({
    getUsers: builder.query({
      query: () => "/users",
    }),
  }),
});

export const { useGetUsersQuery } = apiSlice;
```

3. Add API Slice to Redux Store (store.js)

```js
import { configureStore } from "@reduxjs/toolkit";
import { apiSlice } from "./apiSlice";
import counterReducer from "./counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware),
});
```

4. Fetch Data in Component (Users.js)

```js
import { useGetUsersQuery } from "./apiSlice";

export default function Users() {
  const { data: users, error, isLoading } = useGetUsersQuery();

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error fetching users.</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```
