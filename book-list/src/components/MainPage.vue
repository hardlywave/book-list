<template>
  <div calss="main">
    <div class="main-block">
      <div class="subject-list">
        <h3>You Need to Choose Subject</h3>
        <label class="main-label"
          v-for="subject in subjects"
          :key="subject"
          @change="BooksBySubject"
        >
          <span>{{subject}}</span>
          <input class="main-input" type="radio" :value="subject" v-model="chosenSubject">
        </label>
      </div>
      <div class="book-list">
        <hr v-if=chosenSubject >
        <h3 v-if=chosenSubject>You Need to Choose Book</h3>
        <label class="main-label"
          v-for="book in books"
          :key="book.id"
        >
          <span>{{book.title}}</span>
          <input class="main-input" type="radio" :value="book" v-model="chosenBook">
        </label>
      </div>
      <div class="book">
        <hr v-if=chosenBook>
        <Book
          v-if=chosenBook
          :book=chosenBook
          :subjects=subjects
          @book-update="updateBookList($event)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Book from '@/components/Book.vue';
import  { Subjects, BooksBySubject } from '/api';
export default {
  components: {
    Book,
  },
  data() {
    return {
      subjects: [],
      chosenSubject: null,
      books: [],
      chosenBook: null,
    };
  },
  async created() {
    this.subjects = await Subjects();
  },
  methods: {
    async BooksBySubject() {
      this.chosenBook = null;
      this.books = await BooksBySubject(this.chosenSubject);
    },
    updateBookList(updatedBook) {
      const index = this.books.findIndex(book => book.id === updatedBook.id);
      this.books[index] = updatedBook;
    },
  },
};
</script>

<style>
.main-label {
  display: flex;
  align-items: center;

  border-radius: 100px;
  border: 2px solid hsla(0, 0%, 30%, .14);
  padding: 14px 16px;
  margin: 10px 0;

  cursor: pointer;
  transition: .3s;
}

.main-label:hover,
.main-label:focus-within {
  background: hsla(0, 0%, 30%, .14);
}

.main-input {
  position: absolute;
  left: 0;
  top: 0;
  width: 1px;
  height: 1px;
  opacity: 0;
  z-index: -1;
}
</style>