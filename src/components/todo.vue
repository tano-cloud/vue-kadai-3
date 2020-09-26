<template>
  <div id="toDo">
    <h1>ToDoリスト</h1>
    <!--
      概要：toDoStatus(v-model)にマッチした状態のtoDoリスト一覧を出力する
      ロジック：
      １．各ラジオボタンで、toDoStatus(v-model)を変更する
      ２．toDoStatus(v-model)が変更されると、toDoListFilter(computedプロパティ)が呼ばれる
      ３．toDoStatus(v-model)にマッチした状態のtoDoリスト一覧を出力する
    -->
    <form>
      <label><input type="radio" value="すべて" v-model="toDoStatus" />すべて</label>
      <label><input type="radio" value="作業中" v-model="toDoStatus" />作業中</label>
      <label><input type="radio" value="完了" v-model="toDoStatus" />完了</label>
    </form>

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th colspan="1">状態</th>
        </tr>
      </thead>
      <tbody>
        <!--概要：toDoListFilter(computedプロパティ)のリスト一覧を出力する-->
        <tr v-for="(toDo, index) of toDoListFilter" v-bind:key="index" :value="toDo">
          <td>{{index}}</td>
          <td>{{toDo.comment}}</td>
          <!--概要：クリックすると'完了'と'作業中'のtoDoリストの状態が入れ替わる-->
          <button v-on:click="changeStatus(index)">{{toDo.workingStatus}}</button>
          <button v-on:click="cleanToDo(index)">削除</button>
        </tr>
      </tbody>
    </table>

    <h2>新規タスクの追加</h2>
    <input type="text" v-model="comment" maxlength='20'/>
    <button id="addBtn" v-on:click="addToDo">追加</button>
  </div>
</template>

<script>
export default {
  name: 'toDo',
  data() {
    return {
      toDoStatus: 'すべて',
      comment: '',
      workingStatus: '作業中',
      toDoList:[],
    };
  },
  methods: {
    addToDo: function() {
      if (this.comment) {
        this.toDoList.push({ workingStatus: '作業中', comment: this.comment });
        this.comment = '';
      }
    },
    cleanToDo: function(index){
      this.toDoList.splice(index, 1);
    },
    changeStatus:function(index){
      if(this.toDoList[index].workingStatus === '作業中'){
      this.toDoList[index].workingStatus = '完了';
      }
      else{
        this.toDoList[index].workingStatus = '作業中';
      }
    }
  },
  computed:{
    toDoListFilter:function(){
      return this.toDoList.filter(function(el){
        if(el.workingStatus === this.toDoStatus){
          return el;
        }
        else if(this.toDoStatus === 'すべて'){
          return el;
        }
      },this)
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#toDo{
  margin:0 auto;
  text-align:center;
  width:auto;
}

#toDo table{
  margin:0 auto;
}
</style>
