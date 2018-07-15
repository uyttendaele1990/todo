<template>
 <div>
   <h1>Todo List</h1>
   <input 
          type='text'
          class='todo-input'
          placeholder="what needs to be done"
          v-model="newTodo"
          @keyup.enter="addTodo">
    <div v-for='(todo, index) in todos' :key='todo.id' class='todo-list'>
      <div 
          class='todo-title'
          v-if='!todo.editing'
          @dblclick=
                'todo.editing = !todo.editing;
                 todo.beforeEditCache= todo.title'>
      {{ todo.title }}
      </div>
      
        <input
            v-else type='text'
            v-focus
            @blur='todo.editing=false'
            @keyup.enter='todo.editing=false'
            @keyup.esc=
                        'todo.title=todo.beforeEditCache;
                         todo.editing=false' 
            v-model='todo.title' 
            class='todo-title-edit'>
      
      <div class='delete-task' @click='removeTodo(index)'>
          <i id="remove" class="fa fa-remove"></i>
      </div>
    </div>
</div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      newTodo: '',
      idTodo: 3,
      todos: [
      {
        'id': 1,
        'title': 'Make a todo list',
        'beforeEditCache': '',
        'completed': false,
        'editing': false
      },
      {
        'id': 2,
        'title': 'Make it better',
        'beforeEditCache': '',
        'completed': false,
        'editing': false
      }
      ]
    }
  },
  methods: {
    addTodo() {
      if(this.newTodo.trim().length !== 0){
        this.todos.push({
          id: this.idTodo,
          title: this.newTodo,
          beforeEditCache: '',
          completed: false,
          editing: false
        })
        this.newTodo= "";
        this.idTodo++;
      }
    },
    removeTodo(index){
      swal("Are you sure you want to delete this Task?", {
        buttons: {
          cancel: "Cancel!",
          proceed: {
            text: "Delete Task!",
            value: "proceed",
          },
        },
      })
      .then((value) => {
        switch (value) {
       
          case "proceed":
            swal("Deleted!", "Task was removed from list!", "success");
            this.todos.splice(index, 1);
            break;
       
          default:
            swal("Task not deleted!");
        }
      });
      // this.todos.splice(index, 1);
      // this.$dialog.confirm('Are you sure you want to delete this Task?')
      //     .then(function () {
      //        return this.todos.splice(index, 1);
      //     })
      //     .catch(function () {
      //         console.log('Clicked on cancel')
      //     });
    }
  },
  directives: {
    focus: {
      // directive definition
      inserted: function (el) {
        el.focus()
      }
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
body {
  background-color: lightblue;
}
h1 {
  text-align:center;
}
.swal-modal {
  background-color: lightblue;
  border: 3px solid white;
}
.swal-button {
  background-color:blue;
}
.swal-button--cancel {
  background-color:blue;
  color:white;
}
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 26px;
  margin-bottom: 15px;
}
.todo-list {
  margin-bottom:12px;
  margin-left:15px;
  display: flex;
  font-size: 20px;
  align-items:center;
  justify-content: space-between;
}
.todo-title{
  font-size: 20px;
}
.todo-title-edit{
  font-size: 20px;
  border:1px solid red;
}
#remove {
  cursor:pointer;
  font-size:25px;
  color:red;
  margin-right:15px;
}
@media only screen and (max-width: 330px) {
    .todo-input {
        font-size:20px;
    }
}
</style>
