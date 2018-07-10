---
title: getNodeById的实现方式，即实现树的遍历查找
date: 2013-05-21 23:28:51
categories: "开发"
tags:
	- Node

---

两种方式查找树节点：

递归和非递归-->

``````````
var node_ = {
    id:'1',
    text:'第一层',
    children:[{
        id:'2',
        text:'第二层1'
    },{
        id:'3',
        text:'第二层2',
        children:[{
            id:'4',
            text:'第三层1'
        },{
            id:'5',
            text:'第三层2',
            children:[{
                id:'6',
                text:'第四层1'
            }]
        }]
    }]
};

var getNodeHash = function(treeData){
    var root = treeData;
    var NodeHash  = {};//创建一个以节点id为键，节点信息为值的键值对
    NodeHash[root.id] = root;
    var iterator = root.children;
    var biter = [];
    while (iterator.length != 0) {
        for (var i = 0, len = iterator.length; i < len; i++) {
            var node = iterator[i];
            if(node){
                NodeHash[node.id] = node;
                biter = biter.concat(node.children);
            }
        }
        iterator = biter;
        biter = [];
    }
    return NodeHash;
};



var getHashNodes = {
    /**
     * node为对象{} 递归方式
     * @param node
     * @return {*}
     */
    create:function(node){
        var hashNode = {};
        hashNode[node.id] = node;
        node = node.children;
        function createHashNode(node){
            for(var i= 0,len=node.length;i<len;i++){
                hashNode[node[i].id] = node[i];
                if(node[i].children){
                    arguments.callee(node[i].children);
                }
            }
            return hashNode;
        }
        return createHashNode(node);
    }
};

console.log(getHashNodes.create(node_));
var hashNode_ = getNodeHash(node_);
console.log(hashNode_);
        var getNodeById = function(node,id){
            //先维护出nodeHash 创建nodeHash方法
            var hashNode = getHashNodes.create(node);
            return hashNode[id];
        }
     // console.log(getNodeById(node,'6').text);
``````````


这个是递归，之前写的：

``````````
var root = [{
        id:1,
        name:'根',
        getName:function(){
            return this.name;
        },
        items:[ {
                id:2,
                name:'c',
                getName:function(){
                    return this.name;
                },
                items:[{
                        id:4,
                        name:'44',
                        getName:function(){
                            return this.name;
                        }
                    }]
            },{
                id:3,
                name:'cc',
                getName:function(){
                    return this.name;
                }
            }]
    }];
//树形结构---递归方式
var obj = null;
function test(root, id) {
    for (var i = 0, len = root.length; i < len; i++) {
            if (root[i].id == id) {
                obj = root[i];
                break;
            }else{
                if (root[i].items) {
                    arguments.callee(root[i].items, id);
                }
            }
    }
   return obj;
}
//var s = test(root,1);//得到id为4的组件对象
//console.log(s.getName());//获取对象的name
``````````

