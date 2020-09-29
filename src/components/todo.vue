<template>
  <div id="toDo">
    <h1>ToDoリスト</h1>
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
        <tr v-for="(toDo, index) of toDoListFilter" v-bind:key="index" :value="toDo">
          <td>{{index}}</td>
          <td>{{toDo.comment}}</td>
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
      toDoList: [],
    };
  },
  methods: {
    addToDo: function() {
      if (!this.comment.match(/\S+/)) {
        return alert('空白（半角スペース、全角スペース）のみの入力を禁止します。');
      }
        this.toDoList.push({ workingStatus: '作業中', comment: this.comment });
        this.comment = '';
    },
    cleanToDo: function(index){
      this.toDoList.splice(index, 1);
    },
    changeStatus: function(index){
      if(this.toDoList[index].workingStatus === '作業中'){
        return this.toDoList[index].workingStatus = '完了';
      }
      else {
        return this.toDoList[index].workingStatus = '作業中';
      }
    }
  },
  computed: {
    toDoListFilter: function(){
      //ラジオボタンの値（'作業中'、'完了'）と一致しているtodoを配列で返却する
      return this.toDoList.filter(function(el){
        if(el.workingStatus === this.toDoStatus){
          return el;
        }
      //全てのtodoを配列で返却する
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
