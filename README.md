# react-native-webview-messager

> react-native-webview-messager provides a useful API using async / await for easier communicating with react native webview. 

Jusk like:

``` javascript
// one side
const answer = await messager.send('ask', question) 

// another side
messager.on('ask', question => 'I don\'t know')
```

![rnwm](https://cloud.githubusercontent.com/assets/5719833/20641896/1fb6431c-b43d-11e6-83ec-3fe78e49220f.gif)

## Installation

```
$ npm install react-native-webview-messager --save
```

Require

- React Native >= 0.37

## Basic Usage

There are two side of the lib

### React Native Side Import

Import the `createMessager` function

``` javascript
import createMessager from 'react-native-webview-messager/native'
```

Init the `messager`

``` javascript
class SomePage extends React.Component {
    webview: WebView
    messager = createMessager(() => this.webview)
    render() {
        return <Webview
            ref={webview => this.webview = webview}
            onMessage={this.messager.listener}
            source={require('./index.html')}
        />	
  }
}

```

### Web Side Import 

Import the `messager`

``` javascript
import messager from 'react-native-webview-messager/browser'
```

Or 

``` html
<script src="./node_modules/react-native-webview-messager/browser.js"></script>
<script>
var messager = window.WebViewMessager
</script>
```


### Start

One side

``` javascript
// registry a listener
const me = { name: 'NO.19', nickname: 'Jack' }
const thinking = () => new Promise(resolve => setTimeout(resolve, 2000))
messager.on('what is your', async (q) => {
    await thinking()
    return me[q] || 'I don\'t know'
}) 
```

Another side

``` javascript
const name = await messager.send('what is your', 'name')
// 'NO.19'
```


## API

### on(command, callback)

> bind a command handler

| arg | type | description |
| -- | -- | -- |
| command | `string` | command |
| callback | `(payload: any) => any | Promise<any>` | command handler |


### off(command)

> unbind a command

| arg | type | description |
| -- | -- | -- |
| command | `string` | command |

### send(command, payload)

> send a message to another side

| arg | type | description |
| -- | -- | -- |
| command | `string` | command |
| payload | `any` | payload data |

### listener(evt)

> onMessage handler for WebView component (only at the native side)

| arg | type | description |
| -- | -- | -- |
| evt | `nativeEvent` | event |

Example:

``` javascript
<WebView onMessage={messager.listener} />
```

## Custom Usage (TODO)

`react-native-webview-messager` also provide a factory function for creating custom messager in other webview bridge lib like `react-native-webview-bridge`










