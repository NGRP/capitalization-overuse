---
id: redux-saga-setup
title: Setting up redux-saga
sidebar_label: Redux Saga Setup
---
First you might want to code a saga, so here is an example of a classic saga
```javascript 1.8
// -- ./redux/saga/sagaSample.js
import { takeEvery, call, select, put } from 'redux-saga/effects';

const reducerSelector = state => state.myReducer.data;

const fncRef = () => {}; // just an example, to show how "call" works.

function* sagaExample() {
    const data = yield select(reducerSelector);
    yield call(fncRef);
}

export const saga = [
    takeEvery('ACTION_TYPE', sagaExample)
];

```
Then you will have your `rootSaga` which will concat every saga to expose them:
```javascript 1.8
// -- ./redux/saga/rootSaga.js
import { saga as sagaSample } from './sagaSample';

export default function* rootSaga() {
    yield all([...sagaSample]);
}

```
Then you have to instanciate sagas in your index.js or other entry point
```javascript 1.8
// -- ./index.js
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import { myReducer } from './reducers';
import rootSaga from './saga/rootSaga';

const sagaMiddleware = createSagaMiddleware();
export const store = createStore(myReducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);

```

That's a basic set up of redux-saga in a project
