<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" alt="" class="logo">
    <todo-list msg="番茄清单"></todo-list>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TodoList from './components/TodoList'
export default {
  name: 'app',
  components: {
    TodoList
  }
}
</script>

<style lang="less">
*{
  box-sizing: border-box;
}
.container{
  max-width: 600px;
  margin:0 auto;
  text-align: center;
  .logo{
  vertical-align: top;
  font-size: 24px;
  }
}

</style>
