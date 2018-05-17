# rxLoop

rxLoop = Redux + redux-observable.

Predictable state container for JavaScript apps based on RxJS， like Redux with redux-observable middleware.

1. Using RxJS instead of Redux.
2. Easy study, only four apis: app.model、app.dispatch、app.getState、app.stream.

## Installation
```bash
$ npm i rxloop
```

## Hello rxloop
```javascript
import { Observable } from 'rxjs';
import { mergeMap, map } from 'rxjs/operators';
import rxLoop from 'rxloop';

const counterModel = {
  name: 'counter',
  state: {
    counter: 0,
  },
  reducers: {
    increment(state) {
      return {
        ...state,
        counter: state.counter + 1
      };
    },
  },
  epics: {
    getData(action$) {
      return action$.pipe(
        mergeMap(() => {
          return Observable.fromPromise(
            // Promise
            api().catch((error) => {
              return { error };
            }),
          );
        }),
        map((data) => {
          return {
            type: 'increment',
          };
        }),
      );
    }
  }
};

const app = rxLoop();
app.model(counterModel);

app.stream('counter').subscribe((state) => {
  // this.setState(state);
});

// sync update
app.dispatch({
  type: 'counter/increment',
});

// async update
app.dispatch({
  type: 'counter/getData',
});
```

## Documentation

[rxloop](https://talkingdata.github.io/rxloop/)

## Examples

[examples](https://github.com/TalkingData/rxloop/tree/master/examples)

## Releases

[releases](https://github.com/TalkingData/rxloop/releases)

## License
MIT
