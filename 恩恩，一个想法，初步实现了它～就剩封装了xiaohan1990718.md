---
title: 恩恩，一个想法，初步实现了它～就剩封装了
date: 2013-07-25 22:37:48
categories: "开发"
tags:
	- 技术
	- javascript

---

``````````
var US = {};
        US.randomId = function(){
            var word = ['a','b','c','d','e','f','g'];
            var num = [1,2,3,4,5,6,7,8,9,0];
            var result = '';
            //16位iD
            while(result.length<16){
                var wordRandom = Base.Number.random4Int(0-word.length,word.length-1);
                var numRandom = Base.Number.random4Int(0-num.length,num.length-1)
                if(wordRandom>=0){
                    result += word[wordRandom];
                }
                if(numRandom>=0){
                    result += num[numRandom];
                }
            }
            return result;
        }

        var map = {};
        window.onload = function(){
            var names = document.getElementsByName("showed");
            for(var i= 0,len=names.length;i<len;i++){
                var _map = {};
                _map['id'] = names[i].id = US.randomId();
                var htmlObj = document.getElementById(_map['id']);
                var itemsName = null ,itemsId=null;
                for(var j= 0,len_ = htmlObj.attributes.length;j<len_;j++){
                    if(htmlObj.attributes[j].nodeName=='itemsname'){
                         itemsName =htmlObj.attributes[j].value;
                        continue;
                    }
                    if(htmlObj.attributes[j].nodeName=='itemsid'){
                        itemsId =htmlObj.attributes[j].value;
                        continue;
                    }
                    if(itemsName!=null&&itemsId!=null){
                        break;
                    }
                }
                _map['itemsname'] = itemsName;
                map[itemsId] = _map;
            }
            console.log(map);
            document.getElementById(map['one'].id).value='one Text...';
            console.log(document.getElementById(map['one'].id).value)
        }
``````````

差个封装就完美了～～～就是不直接通过ID来获取dom节点，这样的好处是防止ID重复～～如果优化+思路改进，会不会跟ext/jquery等框架的方式一样呢，思索中....