# Tasks To Do

- Run linter / syntax checker.
- Check how Yjs websocket server and Socket.io websocket server integrate (ie, are the different versions compatible?).
- Refactor how 'ruby' is hardcoded into many different places.
- Specify node version in `package.json` and/or another file (if beneficial).
- Go through files and copy `@todo` comments to here or to the GitHub Project page.

## Fix `Repl.bufferWrite`
### Code
```js
return new Promise(async (resolve) => {
  debug('  `return new Promise(async (resolve = %s) => {`', resolve)

  debug('  `await this.untilCondIsMet(isDataReceived)`')
  await this.untilCondIsMet(isDataReceived);

  debug('`let currResult = result` //==> "%s"', result)
  let currResult = result;

  // @todo: Delete this function, since it's not being used anywhere.
  // const noNewDataReceived = () => currResult === result;

  // @todo: See if this can be refactored to avoid using an interval.
  const intervalId = setInterval(() => {
    debug('  [setInterval()]')

    // @todo: Check where currResult is being returned to.
    if (currResult !== result) {
      debug('    [currResult !== result --> return currResult = result] currResult: "%s", result: "%s"', currResult, result)
      return currResult = result;
    }

    debug('  clearInterval(intervalId = %s)', intervalId)
    clearInterval(intervalId);

    // @todo: Check if it's necessary to remove listener every time.
    debug("this.removeListener('data', concatResult)")
    this.removeListener('data', concatResult);

    debug('  resolve(result = "%s")', result)
    resolve(result);
  }, bufferInterval);
});

```

### Log
```js
Repl [bufferRead(bufferInterval = undefined)] +4ms
Repl [bufferWrite(string = , bufferInterval = 5, write = false)] +0ms
Repl   `return new Promise(async (resolve) => {` resolve: function () { [native code] } +1ms
Repl [untilCondIsMet(condFunc = () => {
    debug('  [isDataReceived()], result: %s', result)
    return result !== '';
  }, interval = 1, value = undefined)] +0ms
Repl   `return new Promise((resolve) => {` resolve: function () { [native code] } +0ms
(node:16) UnhandledPromiseRejectionWarning: TypeError: debug(...) is not a function
  at Promise (/app/repl/Repl.js:44:7)
  at new Promise (<anonymous>)
  at Object.untilCondIsMet (/app/repl/Repl.js:41:12)
  at Promise (/app/repl/Repl.js:79:18)
  at new Promise (<anonymous>)
  at Object.bufferWrite (/app/repl/Repl.js:77:12)
  at Object.bufferRead (/app/repl/Repl.js:108:17)
  at initRepl (/app/server.js:36:10)
  at io.of.clients (/app/server.js:56:7)
  at process.internalTickCallback (internal/process/next_tick.js:70:11)
(node:16) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .cat

(node:16) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
Repl   [concatResult(data = irb(main):001:0> )], result:  +110ms
```

### Information
https://stackoverflow.com/questions/39520930/proper-async-await-code-node-js