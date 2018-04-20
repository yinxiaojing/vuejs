<template>
   <div id="app">
     <h1>{{title}}</h1>
     <input v-model="newItem" @keyup.enter="addNew" class="newItem"/>
     <ul>
         <li v-for="(item,index) in items"  >
            <input type="checkbox" v-on:change="toggleFinish(item)" v-bind:checked="item.isFinished"/>
            <span>{{index+1}}.</span>
            <p v-bind:class="{finished:item.isFinished}">{{item.label}}</p>
            <button v-on:click="deleteItem(item)">delete</button>
         </li>
     </ul>
   </div>
</template>

<script>
import Store from './store'; //调用store.js方法
export default {
  data () {
    return {
       title: 'Todos -Vuejs',
       items: Store.fetch(), //存储记事的数组,从window.localStorage里取出存储的数据
       newItem:'' //绑定input输入的值
    }
  },
  methods:{
     //点击记事完成的事件，isFinished取反，添加下划线
     toggleFinish:function(item){
        item.isFinished=!item.isFinished;
     },
     //新建记事
     addNew:function(){
        this.items.push({
           label:this.newItem,
           isFinished:false
        });
        this.newItem="";//清空重置,双向绑定
     },
     //删除记事
     deleteItem:function(item){
        this.items.splice(this.index,1);
     }
  },
  watch:{
     //监听数组更改
     items:{
        handler:function(items){
             Store.save(items);//存储window.localStorage
        },
        deep:true //深层更改
     }
  }
}
</script>

<style>
ul,li,p{list-style:none; padding:0; margin:0;}
#app{width:800px; margin:20px auto; font-family:"微软雅黑"}
#app h1{font-size:30px;}
#app .newItem{width:800px; height:30px; border-radius:5px; border:1px solid #ccc;}
#app li span{float:left; margin:0 10px;}
#app li input{float:left;margin:14px 0}
#app li p{float:left;}
#app li{overflow:hidden; height:40px; line-height:40px;}
.finished{text-decoration:line-through;}
#app li button{float:right; background:#2c81ba; color:#fff; border:0;border-radius:3px; height:20px; margin:10px 0}
</style>
