<template>
  <div id="app" class="container">
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add New Books</h3>
      </div>
      <div class="panel-body">
         <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Book List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import Firebase from 'firebase'

  // Initialize Firebase
  let config = {
    apiKey: "AIzaSyDx0-QjZTQteiDuR_h2zN9PLwaobLGpHTg",
    authDomain: "vuejs-demo-b49b8.firebaseapp.com",
    databaseURL: "https://vuejs-demo-b49b8.firebaseio.com",
    projectId: "vuejs-demo-b49b8",
    storageBucket: "vuejs-demo-b49b8.appspot.com",
    messagingSenderId: "235974379197"
  };

  let app = Firebase.initializeApp(config)
  let db = app.database()
  let booksRef = db.ref('books')

  export default {
    name: 'App',
    firebase: {
      books: booksRef
    },

    data() {
      return {
        newBook: {
          title: '',
          author: '',
          url: 'http://'
        }
      }
    },

    methods: {
      addBook: function () {
        booksRef.push(this.newBook);
        this.newBook.title = '';
        this.newBook.author = '';
      },
      removeBook: function (book) {
        booksRef.child(book['.key']).remove()
      }
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
