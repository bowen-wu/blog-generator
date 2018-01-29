---
title: jQuery系列（一） -- touch jQuery
date: 2018-01-12 19:01:20
tags: jQuery
---
# 封装函数
尽管 DOM 提供了 API ，可是当我们使用 DOM API 的时候却不尽人意，它提供的功能实在是有限，并且有些方法还没有，所以我们先来封装两个函数。第一个函数作用是获取一个元素节点的所有兄弟，第二个函数作用是可以对一个节点添加或者删除 class 。

#### 获取一个元素节点的所有兄弟
当调用 getSiblings 函数的时候只需要传入一个元素节点即可。
```
function getSiblings(node) {
        let allchild = node.parentNode.children
        let siblings = {
            length: 0
        }
        for (let i = 0, len = allchild.length; i < len; i++) {
            if (allchild[i] !== node) {
                siblings[siblings.length] = allchild[i]
                siblings.length += 1
            }
        }
        return siblings;
    }
```

#### 操纵一个节点的 class 可以进行添加和删除
调用 operationClass 时，需要传入一个节点和一个 classes 对象` operationClass( node, { className: true, className: false } )`
```
function operationClass(node,classes){
        for(let key in classes){
            let method = classes[key] ? 'add' : 'remove'
            node.classList[method](key)
        }
    }
```

# 命名空间
利用命名空间，从而对上述两个函数进行优化
调用方法：
` DOM.operationClass.call(undefined,node,{className:true,className:false})`
`console.log(DOM.getSiblings.call(undefined,node))`
```
let DOM = {}
    DOM.operationClass = (node,classes) => {
        for(let key in classes){
            let method = classes[key] ? 'add' : 'remove'
            node.classList[method](key)
        }
    }
    DOM.getSiblings = (node) => {
        let allchild = node.parentNode.children

        let siblings = {
            length: 0
        }

        for (let i = 0, len = allchild.length; i < len; i++) {
            if (allchild[i] !== node) {
                siblings[siblings.length] = allchild[i]
                siblings.length += 1
            }
        }
        return siblings;
    }
```

# 最终优化
#### 方案一
在 Node.prototype 上添加方法，调用方法是：
`node.operationClass.call(node,{className:true,className:false})`
`node.getSiblings.call(node)`
**缺点**：改变了 Node.prototype 
```
 Node.prototype.operationClass = function(classes){
        for(let key in classes){
            let method = classes[key] ? 'add' : 'remove'
            this.classList[method](key)
        }
    }
    
    Node.prototype.getSiblings = function(){
        let allchild = this.parentNode.children
        let siblings = {
            length: 0
        }

        for (let i = 0, len = allchild.length; i < len; i++) {
            if (allchild[i] !== this) {
                siblings[siblings.length] = allchild[i]
                siblings.length += 1
            }
        }
        return siblings;
    }
```
#### 方案二
创造一个新的接口，使用全局变量 otherNode ，无侵入式调用。
调用方法：

    let node = otherNode.call(undefined,node)
    node.operationClass.call(undefined,{red:true,blue:false})
    node.getSiblings.call(undefined))

```
    window.otherNode = (node) => {
        return {
            operationClass: (classes) => {
                for (let key in classes) {
                    let method = classes[key] ? 'add' : 'remove'
                    node.classList[method](key)
                }
            },
            getSiblings : () => {
                let allchild = node.parentNode.children
                let siblings = {
                    length: 0
                }

                for (let i = 0, len = allchild.length; i < len; i++) {
                    if (allchild[i] !== node) {
                        siblings[siblings.length] = allchild[i]
                        siblings.length += 1
                    }
                }
                return siblings;
            }
        }
    }
```
###### 方案二优化
优化方向：
- 允许用户传入一个 CSS 选择器
- 添加了操作文本的功能
调用方法：
    ```
    let node = otherNode.call(undefined,'#id')
    node.getSiblings.call(undefined)
    node.operationClass.call(undefined,{red:true,blue:false})
    ```
    
```
window.otherNode = (nodeOrSelector) => {
        let node
        if(typeof nodeOrSelector === 'string'){
            node = document.querySelector(nodeOrSelector)
        }else{
            node = nodeOrSelector
        }
        return {
            operationClass: (classes) => {
                for (let key in classes) {
                    let method = classes[key] ? 'add' : 'remove'
                    node.classList[method](key)
                }
            },
            setText:(text) => {
                node.textContent = text;
            },
            getSiblings : () => {
                let allchild = node.parentNode.children
                let siblings = {
                    length: 0
                }

                for (let i = 0, len = allchild.length; i < len; i++) {
                    if (allchild[i] !== node) {
                        siblings[siblings.length] = allchild[i]
                        siblings.length += 1
                    }
                }
                return siblings;
            }
        }
    }
```

###### 方案二再优化
优化方向：
- 满足更多的 CSS 选择器，例如 ` ul > li `
- 简化了 ` text ` 功能，不传参数，就是获取，传参数就是设置
使用方法：
    ```
    let node = otherNode.call(undefined, 'ul > li')
    node.text()
    node.text('hello world')
    node.operationClass.call(undefined, {
        className: false,
        className: true
    })
    ```
```
window.otherNode = (nodeOrSelector) => {
    let nodes = {}
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector)
        for (let i = 0, len = temp.length; i < len; i++) {
            nodes[i] = temp[i]
        }
        nodes.length = temp.length
    } else if (nodeOrSelector instanceof Node) {
        nodes = {
            0: nodeOrSelector,
            length: 1
        }
    }
    nodes.operationClass = (classes) => {
        for (let key in classes) {
            let method = classes[key] ? 'add' : 'remove'
            for (let i = 0, len = nodes.length; i < len; i++) {
                nodes[i].classList[method](key)
            }
        }
    },
    nodes.text = (text) => {
        if (text === undefined) {
            let texts = []
            for (let i = 0, len = nodes.length; i < len; i++) {
                texts.push(nodes[i].textContent)
            }
            return texts
        } else {
            for (let i = 0, len = nodes.length; i < len; i++) {
                nodes[i].textContent = text;
            }
        }
    }
    return nodes
}
```

# jQuery

jQuery 就是将 otherNode 更名为 jQuery，当然 jQuery 的兼容性更好、功能更加丰富（DOM 操作、动画、AJAX操作等等），并且它使用了 prototype 。