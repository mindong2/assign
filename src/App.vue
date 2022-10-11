<template>
  <div id="app">
    <h1>assign</h1>

    <main class="cont">
      <section class="input-item">
        <input type="text" :value="todoItem" @keyup.enter="addToDo" @input="checkInput($event)" placeholder="할 일을 입력하세요.">
        <button @click="addToDo" > 등록 </button>
      </section>

      <section class="todo-list">
        <transition-group name="list" tag="ul">
          <li :class="{todo: true, editing: updateInfo.idx === index }" v-for="(todo, index) in todos" :key="index" >
            <span>{{ todo }}</span>
            <input type="text" :value="updateItem" :index="index" @input="checkInput2($event)">
            <div class="icons">
              <button :index="index" @click="updateToDo(todo, index)">수정</button>
              <!-- 반복문에서 가져온 index를 v-bind로 셋팅 -->
              <button :index="index" @click="deleteToDo($event)">삭제</button> 
              <button :index="index" @click="updateCheck($event,todo)"> V </button>
            </div>
          </li>
          <!-- <li>
            할일2
            <div class="icons">
              <a href="javascript:;">수정</a>
              <a href="javascript:;">삭제</a>
            </div>
          </li> -->
        </transition-group>
      </section>
    </main>

  </div>
</template>


<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      todos : [],
      todoItem : '',
      updateItem : '',
      updateInfo : {
        text : '',
        idx : 0
      }
    }
  },
  methods: {
    // 입력 인풋 내 value값 처리
    checkInput(e){
      this.todoItem = e.target.value;
    },
    checkInput2(e){
      this.updateItem = e.target.value;
    },
    // todos배열에 입력한 값 추가
    addToDo(){
      if(this.todoItem !== ''){
        this.todos.push(this.todoItem); 
        this.todoItem = '';
      }else{
        alert('내용을 입력해주세요.')
      }
    },
    deleteToDo(e){
      let todoIndex = e.target.getAttribute('index');
      this.todos.splice(todoIndex, 1);
    },
    updateToDo(item,idx) {
      this.updateInfo.text = item;
      this.updateInfo.idx = idx;
    },
    updateCheck(e,item){
      let todoIndex2 = e.target.getAttribute('index');
      this.todos[todoIndex2] = this.updateItem;
      console.log(todoIndex2 ,item)
  }
}
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
  margin-top: 60px;
}
#app .cont {max-width: 720px; margin: 0 auto;}
input, button {outline: none;}
li {list-style: none;}
a {color:#000; text-decoration: none;}

.todo-list ul li {display: flex; justify-content: space-between;}

.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
