<template>
    <div>
        <input type="text"  class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
        <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item">
            <div class="todo-item-left">
                <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label">{{ todo.title }}</div>
                <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
                @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
            </div>
            <div class="remove-item" @click="removeTodoItem(index)">
                &times;
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'TodoList',
    data() {
        return {
            idForTodo: 3,
            beforeEdit: '',
            newTodo: '',
            todos: [
                {
                    'id': 1,
                    'title': '1st task',
                    'iscompleted': false,
                    'editing': false,
                },
                {
                    'id': 2,
                    'title': '2nd task',
                    'iscompleted': false,
                    'editing': false,
                }
            ]
        }
    },
    directives: {
    focus: {
        inserted: function (el) {
        el.focus()
        }
    }
    },
    methods: {
        addTodo() {
            if (this.newTodo.trim().length ===0) {
                return
            }
            this.todos.push({
                id: this.idForTodo,
                title: this.newTodo,
                iscompleted: false,
            })

            this.newTodo = '';
            this.idForTodo++;
        },
        removeTodoItem(index) {
            this.todos.splice(index,1);
        },
        editTodo(todo) {
            this.beforeEdit = todo.title;
            todo.editing = true;
        },
        doneEdit(todo) {
            todo.editing = false;
        },
        cancelEdit(todo) {
            todo.title = this.beforeEdit;
            todo.editing = false;
        }
    }
}
</script>

<style lang="scss">
    .todo-input {
        width: 100%;
        padding: 20px 15px;
        margin-bottom: 40px;
        font-size: 1rem;
        &:focus {
        outline: 0;
        }
    }
    .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    &:hover {
      color: black;
    }
  }
  .todo-item-left { // later
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit {
    font-size: 1.2rem;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; //override defaults
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    &:focus {
      outline: none;
    }
  }
</style>

So, in order to add event listeners we use @click in the area where u wanna perform the event
Next we wanna add methods which are nothing but properties.. just after the array of objects 
Afer adding add and remove poroerty in todo we add edit option

Edit: 
create a input textbox same as title in a same div

Give some styling to the resp divs..
Next we wanna change the state depending if we are editing the todo or not..
So we write a manual state as editing: false [ intially we make the eidting as false]

Now here we wanna show based on the edit state..only one text box..
i;e either it will show label or editing text box as per the v-if and v-else-if condn
So, now,, editTodo() has todo.editing value as true(initially)..so..
SO in the v-if condn its value changes to false and it shows us the 1st statement i,e normal statment without 
the edit text..
Once the user double clicks on it.. its value changes to true.. and it shows the edit text box.. 

Next we wanna change its value when user clicks outsdie the text box or presses an enter
..so for that we use the proprty blurr of vuejs.. 

Now,, when we double click and then after clicking again blurr event gets fired.. 
So for getting the element focused,
So, we gonna use  focus directive of vuejs 
after copying the directive we can use it using v-focus