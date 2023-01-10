<template>
  <v-app>
    <HeaderView />

    <v-main>
      <v-container>
        <router-view @add-book-list="addBook"
        :books="books"
        />
      </v-container>
    </v-main>
    <FooterView />
  </v-app>
</template>

<script>
import HeaderView from "./global/HeaderView.vue";
import FooterView from "./global/FooterView.vue";

    const STORAGE_KEY = 'books'


export default {
  components: { HeaderView, FooterView },
  name: "App",

   data(){
    return{
      books: [],
    }
  },
  mounted() {
    if (localStorage.getItem(STORAGE_KEY)) {
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY));
      } catch(e) {
        localStorage.removeItem(STORAGE_KEY);
      }
    }
  },
  methods: {
    addBook(e) {

      this.books.push(
        {id: this.books.length,
        title: e.title,
        image: e.image,
        description: e.description,
        readDate: '',
        memo: ''
        }
      );
      this.saveBooks();
       this.goToEditPage(this.books.slice(-1)[0].id)
    },
    removeBook(x) {
      this.books.splice(x, 1);
      this.saveBooks();
    },
    saveBooks() {
      const parsed = JSON.stringify(this.books);
      localStorage.setItem(STORAGE_KEY, parsed);
    },
    goToEditPage(id){
      this.$router.push(`/edit/${id}`)
    }
  }

};
</script>
