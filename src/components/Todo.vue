<template>
  <div class="main" >
   <link href="https://fonts.googleapis.com/css2?family=Courgette&family=Playfair+Display+SC&display=swap" rel="stylesheet">
     <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Asul&family=Philosopher& family=Kodchasan:wght@200&family=Great+Vibes&family=ABeeZee:ital@1&family=Baloo+Tammudu+2&family=Mulish:wght@300&family=Tenali+Ramakrishna&display=swap" rel="stylesheet" />
   <link  rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.14.0/css/all.css" />
   <link href="https://fonts.googleapis.com/css2? amily=Baloo+Thambi+2:wght@500&family=Montaga&family=Tajawal&display=swap" rel="stylesheet">
        <h1>#Todos</h1>
        <button class="add-btn" @click="addTodo">Add</button>
        <button :class="{active: filter == 'all'}" @click="filter = 'all'"> All</button> 
        <button :class="{active: filter == 'active'}" @click="filter='active'">Active</button>
        <button :class="{active: filter == 'completed'}" @click="filter ='completed'">Completed</button>
    
      <input type="text" name="fname" class="todo-input" placeholder="What needs to be done?" v-model="newTodo" /> 
       <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
       <div v-for="todo in todosFiltered" :key="todo.id" class="todo-item"> 
       <div class="todo-item-left">
         
         <input class="check" type="checkbox" v-model="todo.completed">
         <div  v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed}">{{todo.title}} </div>
         <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"   v-focus>
        </div>
        <div class="remove-item" >
         <i class="fas fa-trash-alt" @click="removeTodo(index)"></i>
        </div>
        
    </div>
      </transition-group>
    <div class="extra-container"><br><br>
      <div class="remain">{{ remaining }} items left </div> 
             <div>
            <button id="btn" v-if="ClearCompletedButton" @click="ClearCompleted">Clear Completed</button>
        </div>
       
       </div>
          
  </div>
  
</template>

<script>
export default {
  name: 'todo-list',
    data () {
    return {
      newTodo: '',
      idForTodo: '',
      beforeEditCache:'',
      filter:'all',
      todos:[],
        completed: false,
          editing: false,
    }
  },
  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length 
    },
    anyRemaining(){
      return this.remaining !=0
    },
    todosFiltered(){
      if(this.filter == 'all'){
        return this.todos
      } else if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
      }else if( this.filter =='completed'){
        return this.todos.filter(todo => todo.completed)
      }
        return this.todos
    },
    ClearCompletedButton() {
        return this.todos.filter(todo => todo.completed).length > 0
      }
  },
  directives: {
    focus:{
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods:{
      addTodo(){
        if(this.newTodo.trim().length == 0){
          return
        }
        
         this.todos.push({
             id: this.idForTodo,
             title: this.newTodo,
             completed: false,
         }) 
          
          this.newTodo = ''
          this.idForTodo++
      },
      editTodo(todo){
        this.beforeEditCache = todo.title
        todo.editing = true
      },
      doneEdit(todo){
        if(todo.title.trim() == ''){
          todo.title = this.beforeEditCache
        }
        todo.editing = false
      },
      cancelEdit(todo){
        
        todo.title = this.beforeEditCache
        todo.editing = false
      },
      removeTodo(index){
        
         this.todos.splice(index, 1);
         
       
      },
      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    ClearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
.main{
  background-color:white;
  position: relative;
  width:450px;
  margin: auto;
  }
  .add-btn{
    width:75px;
    height:60px;
    background:rgb(0, 60, 255);
    color:#fff;
   margin-left:30rem;
   margin-top:6rem;
   position: absolute;
   border: none;
   border-radius:5px;
   font-size:16px;
  }
  .todo-input{
     height:60px;
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 30px 0 rgba(0, 0, 0, 0.1);
    width:400px;
    margin-top:3rem;
    background-color:white;
    border:0.5px solid rgb(177, 170, 170, 0.2);
    text-align:left;
    font: 25px black;
    padding-left: 80px;
    box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.2), 0 4x 4px 0 rgba(0, 0, 0, 0.1);
}
    input[type="text"]:focus{
        outline: 0;
    
    
}
::-webkit-input-placeholder{
  opacity:0.5;
  color:rgb(167, 163, 163);
  font-style:italic calibry;
  font-weight:5px;
  margin-left:-3rem;
}
.todo-item{
    display: flex;
    align-items:center;
    justify-content:space-between;
    animation-duration: 0.10s; 
    border-bottom: 0.2px solid rgb(231, 219, 219, 0.5);
    padding: 25 px;
    font-size:26px;
    }
       
    .todo-item-left{
      display: flex;
      align-items: center;
      margin: 6px;
    }
    .todo-item-edit{
      padding: 10px;
      color: #2c3e50;
      width: 200px;
      border: 1px solid black;
      font-family: 'avenir',Helvatica, Arial, sans-serif;
      outline:none;
      align-items:center;
      font-size:25px;
      
    }
    

     .todo-item-edit:focus{
        outline: none;
      }
    .completed{
      text-decoration: line-through;
      color: grey;
      
    }
    .extra-container {
      display: flex;
      align-items: center;
      padding: 5px;
      font-size: 16px;
      border-top:1px solid lightgrey;
      padding-top: 14px;
      margin-bottom: 14px;
      opacity:0.5;
      width:450px;
      color:black;
     font-family: 'Courgette', cursive;
    }
    button{
      font-size: 20px;
      background-color: white;
      margin-left: 2rem;
      border:none;
      margin-top:1rem;
      align-items: center;
      font-family: 'Playfair Display SC', serif;
      font-weight:700;
       
    }
    button:hover{
       
         border-bottom:2px solid rgb(0, 81, 255);
         }
         button:focus {
           outline: none;

         }
    .active {
  border-bottom:2px solid rgb(0, 81, 255);

    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to{
      opacity: 0;
    }
    .check{
    opacity: 1;
     
    }
    #btn{
       margin-left:160px;
      font-family: 'Philosopher', sans-serif;
      font-size:14px;
      color:black;
   
    }
    #btn:hover{
    text-decoration:underline;
    cursor:pointer;
     border:none;

    }
    input[type="checkbox"]{
    border-style:1px solid gray;
    background: white;
    width:20px;
    height:20px;
     
    }
   input[type="checkbox"]:focus{
      background:blue;
    }
    .remain{
      margin-right:80px;
      margin-left: 10px;
      color:black;
      font-size:16px;
    }
   .remove-item{
     width:35px;
     height:35px;
   
     float:right;
     position:relative;
   }
   .remove-item>i{
     position:absolute;
     color:black;
     font-size:18px; 
     margin-left:-5px;
     margin-top:5px;
   }
     .remove-item>i:hover{
       color:red;
     }
</style>
