## Language
[简体中文](https://github.com/kszitt/react-native-sass-to-styleSheet/blob/master/README_CN.md)

## Description
sass and css file transform to react-native stylesheet

## install
``` javascript
npm install react-native-sass-to-stylesheet --save-dev
```

## use
#### 1. create`toStyles.js`, this content
``` javascript
const SassToStyles = require("react-native-sass-to-stylesheet");

SassToStyles.init(<path>);
```

##### .init(path[, options])
- path{string} watch folder paths，request
- options{object}
    - space{number} indent value，default`2`
    - postfix{string} jS file suffix generated by transformation，default`Style.js` 
      for example:`home.scss => homeStyle.js`
    - initTransform{boolean} Whether to automatically convert all CSS files after starting the service, default`false`
    - ignored{reg} ignored files, default`/\.(jsx?|png|jpe?g|gif|json)$/`
    - templatePath{string} automatic conversion of file template path

#### 2. `scripts` in `package.json`, add
``` json
"transition": "node toStyles.js"
```

#### 3. start
``` javascript
npm run transition
```

## effect
### example 1
``` scss
#header {
  font-size: 12px;
  .logo {
    width: 100px;
  }
}
.footer {
  background: rgba(255, 255, 255, .8);
}
```
↓ ↓ ↓ ↓ ↓ ↓
``` javascript
let styles = {
  header: {
    fontSize: 12,
  },
  header_logo: {
    width: 100,
  },
  footer: {
    backgroundColor: "rgba(255, 255, 255, .8)",
  }
};
```
### example 2
``` scss
/* note */
#header {
  // note
  background: #888;
  border: 1px solid #ccc;
  flex-direction: row;
  margin: 0 10px 10px;
  text-decoration: underline white solid;
}
```
↓ ↓ ↓ ↓ ↓ ↓
``` javascript
let styles = {
  header: {
    backgroundColor: "#888",
    borderWidth: 1,
    borderStyle: "solid",
    borderColor: "#ccc",
    flexDirection: "row",
    marginTop: 0,
    marginRight: 10,
    marginBottom: 10,
    marginLeft: 10,
    textDecorationLine: "underline",
    textDecorationColor: "white",
    textDecorationStyle: "solid",
  }
}
```
### example 3
``` scss
$size: 12px !global;  // global variables can be used directly in any file
$color: red;
#home {
  flex-direction: column;
  font-size: $size;
  background: $color;
}
.main {
  font-size: $size;
}
```
↓ ↓ ↓ ↓ ↓ ↓
``` javascript
let styles = {
  home: {
    flexDirection: "column",
    fontSize: 12,
    backgroundColor: "red",
  },
  main: {
    fontSize: 12,
  }
};
```
### example 4
``` scss
#header{
  font: italic bold 12px/24px "arial";
  transform: translateY(5px) scaleY(3) rotate(10deg) skewY(20deg);
  text-shadow: 10px 20px 5px #ccc;
}
```
↓ ↓ ↓ ↓ ↓ ↓
``` javascript
let styles = {
  header:{
    fontStyle: "italic",
    fontWeight: "bold",
    fontSize: 12,
    fontHeight: 24,
    fontFamily: "arial",
    transform: {
      translateY: 5,
      scaleY: 3,
      rotate: "10deg",
      skewY: "20deg",
    },
    textShadowOffset: {
      width: 10,
      height: 20
    },
    textShadowRadio: 5,
    textShadowColor: "#ccc",
  }
};
```

## Use Demo
#### 1. install
``` javascript
npm install
```
#### 2. start server
``` javascript
npm run temple
```
#### 3. create and modify sass and css files under "temple/src" folder
#### 4. the directory where the current CSS file is located automatically generates JS files

## Template
insert the transformed `styles`object into the template's `let styles = {};` in which it is directly referenced in `react-native`.

### default template
``` javascript
import {StyleSheet, PixelRatio} from 'react-native';

let styles = {};

const styleSheet = StyleSheet.create(styles);

export default styleSheet;
```
### Custom Template
create `template.js`file in the project root directory. `let styles = {};`no modification. for template
``` javascript
import {StyleSheet, PixelRatio} from 'react-native';
const pixelRatio = PixelRatio.get();

let styles = {};

for(let i in styles){
  for(let k in styles[i]){
    if(typeof styles[i][k] === "number"){
      if(k !== "flex"){
        styles[i][k] = parseFloat((styles[i][k] / pixelRatio).toFixed(2));
      }
    }
  }
}

const styleSheet = StyleSheet.create(styles);

export default styleSheet;
```

## Be careful
##### 1. Please write SCSS in the following form. Each style has a `;`end, The number of indented cells can be customized.
``` scss
#header {
  font-size: 12px;
}
```
##### 2. The following conversion is unsuccessful. Avoid using it
``` scss
.aa, .bb {

}
.cc .dd {

}
```