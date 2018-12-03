<template>
  <div class="todo">
    <h1>{{msg}}</h1>
    <input type="text" class="todo-input" placeholder="what needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed" >
            <span :class="{done:todo.completed}" v-if="index!=editIndex" @dblclick="editTodo(index)" class="todo-item-label">{{todo.title}}</span>
            <input v-focus v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(index)" @keyup.enter="doneEdit(index)" @keyup.escape="cancelEdit(index)">
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>
    </transition-group>
    <div class="extra-container">
        <div>
            <button :class="{active:filter=='all'}" @click="filter='all'">All</button>
            <button :class="{active:filter=='active'}" @click="filter='active'">Active</button>
            <button :class="{active:filter=='completed'}" @click="filter='completed'">Completed</button>
        </div>
        <div>
            <label><input type="checkbox" :checked="notAnyRemaining" @change="checkAllTodos(event)">全选</label>
        </div>
        <div>{{remaining}}剩余</div>
        <button @click="clearAll">清除所有</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  props: {
    msg: String
  },
  data(){
      return {
          filter: 'all',
          idForTodo:3,
          newTodo:'',
          editIndex:-1,
          todos:[
              {
                  id:1,
                  title:'吃饭',
                  completed:false
              },
              {
                  id:2,
                  title:'睡觉',
                  completed:true
              },
              {
                  id:3,
                  title:'打豆豆',
                  completed:false
              }
          ],
          editCacheData:null
      }
  },
  computed:{
      remaining(){
          return this.todos.filter(item=>item.completed==false).length
      },
      notAnyRemaining(){
          return this.todos.filter(item=>item.completed==false).length==0
      },
      todosFiltered(){
          if (this.filter == 'all') {
            return this.todos
        } else if (this.filter == 'active') {
            return this.todos.filter(todo => !todo.completed)
        } else if (this.filter == 'completed') {
            return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      }
  },
  methods:{
      addTodo(){
          if(this.newTodo.trim() == "")return
          this.todos.unshift({
              id:++this.idForTodo,
              title:this.newTodo,
              completed:false
          })

          this.newTodo = ''
          
      },
      removeTodo(index){
          this.todos.splice(index,1)
      },
      editTodo(index){
          this.editCacheData = this.todos[index].title
          this.editIndex = index
      },
      doneEdit(index){
          if(this.todos[index].title==""){
              this.todos[index].title = this.editCacheData
          }
          this.editIndex = -1
      },
      cancelEdit(index){
          this.editIndex = -1
          this.todos[index].title = this.editCacheData
      },
      checkAllTodos(){
          this.todos.forEach(item=>item.completed = event.target.checked )
      },
      clearAll(){
          this.todos = []
      }
  },
  directives: {
        focus: {
            // 指令的定义
            inserted: function (el) {
            el.focus()
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
.todo{
    .todo-input{
        width:100%;
        padding:10px 10px;
        font-size: 18px;
        margin-bottom: 16px;
        &:focus{
            outline: 0;
        }
    }
    .todo-item{
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        .remove-item{
            cursor: pointer;
            margin-left: 14px;
            &:hover{
                color:black;
            }
        }
        .todo-item-left{
            .done{
                text-decoration: line-through;
            }
        }   
    }
    .extra-container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        .active{
            background-color: blue;
            color: #fff;
        }
    }
    .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
}
</style>
