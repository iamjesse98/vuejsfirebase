<template>
  <div id="app">
    <md-toolbar style="box-shadow: 0 1px 4px rgba(0, 0, 0, .7); position: fixed; width: 100%; z-index: 10;">
    <h1 class="md-title">Vue and firebase sample...</h1>
  </md-toolbar>
  <br/><br><br>
  <div style="padding: 10px">
  <md-card>
    <md-card-header style="background: #eee; cursor: pointer; user-select: none;" v-on:click="showAdd = !showAdd">
      <div v-on:click="showAdd = !showAdd"><div class="md-title">Add a Book</div>
      <div class="md-subhead">Adds a Book to the Database</div></div>
     </md-card-header>
    <md-card-content v-show="showAdd">
        <form id="form" v-on:submit.prevent="addBook">
          <md-layout md-gutter>
          <md-layout md-flex-xsmall="100" md-flex-small="33" md-flex-medium="33">
           <md-input-container>
            <label>Title</label>
            <md-input id="bookTitle" v-model="newBook.title"></md-input>
          </md-input-container>
          </md-layout>

          <md-layout md-flex-xsmall="100" md-flex-small="33" md-flex-medium="33">
          <md-input-container>
            <label>Author</label>
            <md-input id="bookTitle" v-model="newBook.author"></md-input>
          </md-input-container>
          </md-layout>

          <md-layout md-flex-xsmall="100" md-flex-small="33" md-flex-medium="33">
          <md-input-container>
            <label>URL</label>
            <md-input id="bookTitle" v-model="newBook.url"></md-input>
          </md-input-container>
          </md-layout>

          </md-layout>

          <md-layout md-gutter>

          <md-layout md-flex-xsmall="100" md-flex-small="50" md-flex-medium="50">
            <md-button class="md-raised md-primary" type="submit">Add Book</md-button>
          </md-layout>

          <md-layout md-flex-xsmall="100" md-flex-small="50" md-flex-medium="50">
            <md-button class="md-raised md-warn" type="reset">Reset</md-button>
          </md-layout>

          </md-layout>

        </form>
      </md-card-content>
  </md-card>

  <br>

  <md-card>
     <md-card-header style="background: #eee; cursor: pointer; user-select: none;">
      <div v-on:click="showBooks = !showBooks"><div class="md-title">Books</div>
      <div class="md-subhead">Books in Database</div></div>
     </md-card-header>
     <md-table v-show="showBooks">
       <md-table-row>
        <md-table-head>Title</md-table-head>
        <md-table-head>Author</md-table-head>
        <md-table-head>Delete</md-table-head>
       </md-table-row>

       <md-table-body>
         <md-table-row v-for="book in books" :key="book">
            <md-table-cell><a v-bind:href="book.url">{{ book.title }} <md-tooltip md-direction="top">{{ book.url }}</md-tooltip></a></md-table-cell>
             <md-table-cell>{{ book.author }}</md-table-cell>
             <md-table-cell><div style="cursor: pointer; user-select: none; width: 100%; text-align: left" v-on:click.prevent="removeBook(book)"><md-icon>delete_sweep</md-icon></div></md-table-cell>
         </md-table-row>
       </md-table-body>
     </md-table>

  </md-card>
  </div>

    <md-snackbar md-position="bottom center" ref="snackbar" md-duration="3000">Successfully Removed</md-snackbar>

    <md-snackbar md-position="bottom center" ref="errsnackbar" md-duration="3000">Fill the form</md-snackbar>

  </div>
</template>

<script>
import Firebase from 'firebase'

let config = {
    apiKey: "AIzaSyBipm7koSPyMxlx4ILTIe1VnMmFWOoKujc",
    authDomain: "vuejs-firabase.firebaseapp.com",
    databaseURL: "https://vuejs-firabase.firebaseio.com",
    projectId: "vuejs-firabase",
    storageBucket: "vuejs-firabase.appspot.com",
    messagingSenderId: "1027218285386"
  }
 let app = Firebase.initializeApp(config)

 let db = app.database()

 let booksRef = db.ref('books')


export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      },
      showAdd: true,
      showBooks: true
    }
  },
  methods: {
    addBook: function() {
      if (this.newBook.title != '' && this.newBook.author != '' && this.newBook.url != '') booksRef.push(this.newBook)
      else this.$refs.errsnackbar.open()
      this.newBook.title = this.newBook.author = this.newBook.url = ''
      this.booksRef.title = this.booksRef.author = this.booksRef.url = ''
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove()
      this.$refs.snackbar.open()
    }
  }
}
</script>

<style>
.my-content {
  padding: 10px;
}
.md-button {
  width: 100%;
}
</style>
