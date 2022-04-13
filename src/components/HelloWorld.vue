<template>
  <div class="to-do-list">
    <h1>To Do List</h1>
    <input type="text" v-model="newTitle">
    <input type="text" v-model="newItem" @keyup.enter="addItem">
    <button @click="addToList()">ADD</button>
    <ul>
      <li v-for="item in listToDo" :key="item.id">
        <div style="display:block">
          <input type="checkbox" v-model="item.completed">
        <div>
        <p>Title: {{item.title}}</p>
        <span>Content: {{item.content}}</span>
        </div>
        <button @click="removeItem(item)">X</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return{
      listToDo:[],
      newItem:'',
      newTitle:'',
      url:'http://192.168.1.38/note_cy/long/listNote.php',
      method:'GET',
      
    }
  },
  methods:{
    send(){
      fetch(this.url,{
        method:this.method,
        headers:{
          'Accept':'application/json'
        },
        
      }).then(res=>res.json())
      .then(data=>{
        console.log(data);
        this.listToDo=data;
      }).catch(err=>{
        console.log(err);
      })
    },
     addToList() {
      const formData = new FormData();
      formData.append("title", this.newTitle);
      formData.append("content", this.newItem);
      fetch('http://192.168.1.38/note_cy/long/addNote.php', {
            method: 'POST',
            body: formData,
          }
      )
          .then(() => {
                alert('ok');
                this.send();
              }
          )
          .catch(error => console.log(error))
    },
    removeItem(item){
      this.listToDo.splice(this.listToDo.indexOf(item),1)
    },
    },
    mounted() {
      this.send();
    }
  }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
