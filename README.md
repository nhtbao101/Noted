Redux follow

1. Create action type: makeActionTypes
2. Create action
```javascript
    export const getSomething = (param: number, body?: any) => ({
      type: ACTION_TYPES.GET_SOMETHING,
      payload: { param, body }
    });

```
3. Create middleware

```javascript
export function* getSomething({ payload }: AnyAction) {
  try {
    const http = new ApiService();
    const { param, body } = payload;
   const requestBody = {};
    const res = yield http.get(
      [],
      requestBody,
      { headers }
    );
    yield put(getSuccess(res.data));
  } catch (error) {
    yield put(getError(error));
  }
}

```
```javascript
export function* watchSomething() {
  yield takeLatest(ACTION_TYPES.GET_SOMETHING, getSomething);
}
```

4. Create reducer initState 
```javascript
const initialState = {
  isLoading: false,
  isProcessing: false,
  hasError: false,
  data: null,
  error: null,
};
```

5. Create reducer function
```javascript
const getSomething = (state) => ({
  ...state,
  isLoading: true,
});

const getSomethingSuccess = (state, payload) => ({
  ...state,
  isLoading: false,
  data: payload.data,
});

const getSomethingError = (state, payload) => ({
  ...state,
  isLoading: false,
  hasError: true,
  error: payload,
});
```

```javascript
import { all } from 'redux-saga/effects';
export default function* appMiddleware() {
  //in middleware  
  yield all([
  	watchSomething()
  ])
}

```

```javascript
export const getForSomething = createReducer(
  strategiesGetSomething,
  initialState
);
```

6.
```javascript
import { combineReducers } from 'redux'
const appReducer = combineReducers({
  getForSomething
})
```

