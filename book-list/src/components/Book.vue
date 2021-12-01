<template>
  <div class="main-div-form">
    <form class="main-form">
      <div calss="title">
        <span class="title">Title:</span>
        <br>
        <input type="text" v-model="bookCopy.title">
      </div>
      <br>
      <div class="subjects">
        <span class="title">Subjects:</span>
        <br>
        <label
            class="subject-label"
            v-for="(subject, index) in subjects"
            :key="index"
        >
        <br>
        <span>{{ subject }}</span>
        <input
          type="checkbox"
          :value="subject"
          v-model="bookCopy.subjects"
        >
        </label>
      </div>
      <div class="author">
        <span class="title">Authors: </span>
        <br>
        <div class="author-input-block" v-for="(author, index) in bookCopy.authors" :key="index">
          <span class="span-input">
            Name:
            <br>
            <input
                type="text"
                v-model="author.name"
            >
          </span>
          <span class="span-input">
            Birthday Year:
            <br>
            <input
              class="date-input"
              type="text"
              v-model="author.birth_year"
            >
          </span>
          <span class="span-input">
            Death Year:
            <br>
            <input
                class="date-input"
                type="text"
                v-model="author.death_year"
            >
          </span>
          <button
              class="button-small"
              @click.prevent="deleteElement('authors', index)"
          >Remove Element</button>
        </div>
        <br>
        <button class="button-small" @click.prevent="addAuthor">Add Author</button>
      </div>
      <div class="bookshelve-block">
        <span class="title">Bookshelves:</span>
        <div class="bookshelve-input-block" v-for="(bookshelve, index) in bookCopy.bookshelves" :key="index">
          <input
              class="bookshelve-input"
              type="text"
              v-model="bookCopy.bookshelves[index]"
          >
          <button
              class="button-small"
              @click.prevent="deleteElement('bookshelves', index)"
          >Remove Element</button>
        </div>
        <button class="button-small" @click.prevent="addBookShelve">Add BookShelve</button>
      </div>
      <div class="buttons-List">
        <button class="button" @click.prevent="saveBook">Save</button>
        <button class="button" @click.prevent="setBookCopy">Cancel</button>
      </div>
    </form>
  </div>
</template>

<script>
import { saveBook } from '/api';
export default {
  props: {
    book: {
      type: Object,
      required: true,
    },
    subjects: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      bookCopy: null,
    };
  },

  watch: {
    book: {
      handler: 'setBookCopy',
      immediate: true,
    },
  },

  methods: {
    setBookCopy() {
      this.isBookEditing = false;
      this.bookCopy = JSON.parse(JSON.stringify(this.book));
    },

    async saveBook() {
      const updatedBook = await saveBook(this.bookCopy);
      this.$emit('book-update', updatedBook);
    },

    addAuthor() {
      this.bookCopy.authors.push({
        book: {
          handler: 'setBookCopy',
          immediate: true,
        },
        name: '',
        birth_year: '',
        death_year: '',
      });
    },

    addBookShelve() {
      this.bookCopy.bookshelves.push('');
    },

    deleteElement(category, index) {
      this.bookCopy[category].splice(index, 1);
    },
  },
};
</script>

<style>
  .main-div-form{
    font-size: 18px;
    display: inline-flex;
    padding: 10px;
    border: 2px solid hsla(0, 0%, 30%, .14);
    border-radius: 15px;
  }
  .span-input,
  .subject-label,
  .author-input-block{
    font-family: cursive;
    font-size: 18px;
    display: inline-block;
    padding: 10px;
    margin: 15px 10px 10px 5px;
    border: 2px solid hsla(0, 0%, 30%, .14);
    border-radius: 15px;
  }
  .author-input-block{
    display: block;
  }
  .button {
    transition-duration: 0.4s;
    border-radius: 8px;
  }

  .button-small {
    align-self: flex-start;
    margin-top: 5px;
    padding: 2px 5px;
    border-radius: 8px;
  }

  .button-small:hover,
  .button:hover {
    background-color: hsla(0, 0%, 10%, .14);
    color: white;
  }
</style>