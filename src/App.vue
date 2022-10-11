<template>
  <div id="app">

    <main class="cont">
      <h1>New Todo List</h1>
      <section class="input-item">
        <input type="text" class="top_input" :value="todoItem" @keyup.enter="addToDo" @input="checkInput($event)" placeholder="할 일을 입력하세요.">
        <button @click="addToDo"  style="margin-left:10px;"> 등록 </button>
      </section>

      <section class="todo-list">
        <transition-group name="list" tag="ul">
          <li :class="{todo: true, editing: updateInfo.idx === index }" v-for="(todo, index) in todos" :key="index" >
            <span>{{updateInfo.idx === index ? updateItem : todo }}</span>
            <input class="update_input" type="text" :value="updateItem" :index="index" @input="checkInput2($event)">
            <div class="icons">
              <button class="mr10" v-show="updateInfo.idx !== index" :index="index" @click="updateToDo(todo, index)">수정</button>
              <!-- 반복문에서 가져온 index를 v-bind로 셋팅 -->
              <button v-show="updateInfo.idx !== index" :index="index" @click="deleteToDo($event)">삭제</button> 
              <button  v-show="updateInfo.idx === index" :index="index" @click="updateCheck(index)"> V </button>
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

        <div class="no_list" v-if="todos.length === 0">
          <p key="item">리스트에 항목이 필요합니다.</p>
        </div>
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
        idx : -1
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
      this.updateItem = item;
    },
    updateCheck(idx){
      this.todos[idx] = this.updateItem;
      this.updateInfo.idx = -1;
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
  font-size: 24px;
  height: 100vh;
  background-color: rgb(186, 241, 200);
  overflow: hidden;
}
#app .cont {max-width: 480px; margin: 60px auto 0; background: #fff; border-radius: 20px; padding: 20px 0; min-height: 480px;}
.mr10 {margin-right: 10px;}
input, button {outline: none; border:none; padding: 5px 10px;}
button {border-radius: 10px;}
input {background: rgb(201, 178, 193); font-size: 24px;}
input.top_input {padding: 10px 15px; font-size: 24px;}
ul {padding: 0;}
li {list-style: none; background: rgb(245, 192, 192); padding: 5px 10px;}
a {color:#000; text-decoration: none;}

.todo-list {max-height: 400px; overflow: auto; padding: 0 15px;}
.todo-list {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
.todo-list::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera*/
}
.todo-list ul li {display: flex; justify-content: space-between; margin-top: 15px; border-radius: 10px; padding: 5px 10px;}

.todo-list .editing span,
.todo-list input,
.todo-list .btn_off {display: none;}
.todo-list span,
.todo-list .editing .update_input,
.todo-let .editing .btn_on {display: inline;}

.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
