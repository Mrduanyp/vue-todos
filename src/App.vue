<template>
  <div id="app">
    <h1>{{title}}</h1>
    <ul class="todos">
      <li>
        <input  v-model="newTodo"
                @keyup.13="addItem"
                placeholder="快写下让我记住的事情吧"
                autofocus="tue">
      </li>
      <li v-for="(todo,index) in todos"
          :class="{'checked':todo.done}">
          <input  type="checkbox"
                  @change="saveToStore"
                  v-model="todo.done">
        <label>{{ index + 1 }}.{{ todo.value }}</label>
        <time>{{todo.created|date}}</time>
        <button @click.prevent="delItem(todo)"></button>
      </li>
    </ul>
  </div>
</template>

<script>


// import './assets/site.less'
// import './assets/todos.less'

import moment from 'moment'

import 'moment/locale/zh-cn'

moment.locale('zh-cn')

export default {
  name: 'app',
  data () {
    return {
      newTodo:"",
      title: "vue-todos",
      todos: [

        {value:  "完善简历",done:true},
        {value:  "投递简历",done:true},
        {value:  "面试",done:false},
        {value:  "试用期",done:false},
        {value:  "正式入职",done:false},
      ]
    }
  },
  created (){
    if (this.is_initialiazed) {
        this.todos = JSON.parse(localeStorage.getItem('VUE-TODOS'))
    }
  },
  computed: {
    is_initialized (){
      return localStorage.getItem('VUE-TODOS') != null
    }
  },

  filters: {

    date(val){

      return moment(val).calendar()

    }

  },
  methods: {
    addItem() {
      this.todos.push({
        value: this.newTodo,
        created: Date.now(),
        done: false
      });
      this.saveToStore();
      this.newTodo = ''
    },
    delItem (todo) {
      this.todos = this.todos.filter((x) => x !== todo)
      this.saveToStore()
    },
    saveToStore(){
      localStorage.setItem('VUE-TODOS', JSON.stringify(this.todos))
    }
  }

}
</script>

<style type="text/css">
  @import './assets/site.less'
</style>


<style>

.todos {
  margin: 0;
  list-style-type: none;
  padding: 0 20px;
  border-top: 1px solid #ccc;
  background: #fff;
  border: 1px solid #ccc;
  box-shadow: 0 10px 30px #ccc;
  position: relative;
  transiton: .3s;
}
.todos input::-webkit-input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}
.todos input::-moz-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}
.todos input::-moz-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}
.todos input::-ms-input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}
.todos  > li {
  cursor: pointer;
  font-size: 24px;
  line-height: 36px;
  padding: 12px 0;
  border-bottom: 1px solid #ededed;
  position: relative;
}
.todos  > li:last-child {
  border-bottom: none;
}
.todos  > li:first-child > input {
  font-size: 24px;
  padding: 10px;
  border: none;
  width: 100%;
  background: transparent;
}
.todos  > li  > input[type=checkbox] {
  display: inline-block;
  vertical-align: middle;
  text-align: center;
  width: 40px;
  height: auto;
  position: absolute;
  top: 10px;
  bottom: 0;
  margin: auto 0;
  border: none;
  -webkit-appearance: none;
  appearance: none;
}
.todos  > li  > input[type=checkbox]:after {
  content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
}
.todos  > li  > time {
  position: absolute;
  right: 60px;
  top: 15px;
  font-size: 9pt;
}
.todos  > li  > label {
  display: block;
  vertical-align: middle;
  padding-left: 50px;
  letter-spacing: 2;
}
.todos  > li  > button {
  position: absolute;
  right: 5px;
  top: 15px;
  height: 30px;
  width: 30px;
  border: none;
  background: none;
  display: none;
}
.todos  > li  > button:after,
.todos  > li  > button:before {
  content: "";
  position: absolute;
  top: 15px;
  left: 0;
  transform: rotatez(45deg);
  height: 1px;
  width: 30px;
  background: #cc9a9a;
}
.todos  > li  > button:before {
  transform: rotatez(-45deg);
}
.todos  > li.checked  > input[type=checkbox]:after {
  content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}
.todos  > li.checked  > label {
  color: #d9d9d9;
  text-decoration: line-through;
}
.todos  > li:hover  > button {
  display: block;
}

</style>
