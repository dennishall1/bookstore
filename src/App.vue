<template>
  <div id="app" class="container">
    <hello></hello>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add book</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">URL</label>
            <input type="text" id="bookURL" class="form-control" v-model="newBook.url" placeholder="http://...">
          </div>
          <input type="submit" class="btn btn-primary" name="Add book">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Books List</h3>
        </div>
        <div class="panel-body">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>title</th>
                <th>author</th>
                <th>date</th>
                <th>action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="book in books">
                <td><a v-bind:href="book.url">{{book.title}}</a></td>
                <td>{{book.author}}</td>
                <td>{{book.date}}</td>
                <td><span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span></td>
              </tr>
            </tbody>
          </table>
        </div>
    </div>
  </div>
</template>

<script>

var date = new Date().toLocaleString();

//Component example
import Hello from './components/Hello'
//Firebase
import Firebase from 'firebase'
//Toastr
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyAv-ooisrqviMnFwIahITGJkZrOw_GqWYg",
  authDomain: "vuejs-4a04f.firebaseapp.com",
  databaseURL: "https://vuejs-4a04f.firebaseio.com",
  projectId: "vuejs-4a04f",
  storageBucket: "vuejs-4a04f.appspot.com",
  messagingSenderId: "71616474618"
}

let app = Firebase.initializeApp(config);
let db = app.database();
let booksRef = db.ref('books');

export default {
  //app name
  name: 'app',
  //import componentes
  components: {
    Hello
  },
  //add firebase
  firebase: {
    books: booksRef,
  },
  //template for send data to firebase
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: '',
        date: date,
      }
    }
  },
  //methods for send data to firebase
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      //restart form
      this.newBook.title = '',
      this.newBook.author = '',
      this.newBook.url = ''
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove();
      //show alert with toastr
      toastr.success("Book removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
