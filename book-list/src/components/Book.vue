<template>
  <div class="main-div-form">
    <form class="main-form">
      <div class="div_main-title">
        <span class="span_input">
          Title:
            <input
                v-model="bookCopy.title"
                class="title_input"
                type="text"
            >
        </span>
      </div>
      <div class="subjects">
        <span class="title">Subjects:
          <label
              class="subject-label"
              v-for="(subject, index) in subjects"
              :key="index"
          >
          <span>{{ subject }}</span>
          <input
              type="checkbox"
              :value="subject"
              v-model="bookCopy.subjects"
          >
          </label>
        </span>
      </div>
      <div class="author">
        <span class="title">Authors:</span>
        <div
            v-for="(author, index) in bookCopy.authors"
            :key="index"
            class="author-input-block"
        >
          <label class="label-input">
            Name:
            <input
                v-model="author.name"
                class="data-input"
                type="text"
            >
          </label>
          <label class="label-input">
            Birthday Year:
            <input
                v-model="author.birth_year"
                class="date-input"
                type="text"
            >
          </label>
          <label class="label-input">
            Death Year:
            <input
                v-model="author.death_year"
                class="date-input"
                type="text"
            >
          </label>
          <button
              class="button"
              @click.prevent="deleteElement('authors', index)"
          >Remove Element</button>
        </div>
        <button
            class="button add-author"
            @click.prevent="addAuthor"
        >Add Author</button>
      </div>
      <div class="bookshelve-block">
        <span class="title">Bookshelves:</span>
        <div class="div_bookshelve-block">
          <div
              v-for="(bookshelve, index) in bookCopy.bookshelves"
              :key="index"
              class="bookshelve-input-block"
          >
            <input
                v-model="bookCopy.bookshelves[index]"
                class="bookshelve-input"
                type="text"
            >
            <button
                class="button"
                style="margin-left: 5px"
                @click.prevent="deleteElement('bookshelves', index)"
            >Remove Element</button>
          </div>
          <button class="button add-shelve" @click.prevent="addBookShelve">Add BookShelve</button>
        </div>
      </div>
      <div class="buttons-list">
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
  * {
    font-family: cursive;
  }
  .div_main-title span input{
    display: block;
    margin: 15px 10px 10px 15px;
  }
  .subjects span{
    display: inline-block;
  }
  .subjects span label{
    display: block;
    margin: 15px 5px 10px 15px;
  }
  .subjects span label input{
    margin-top: 0px;
  }
  input{
    border: 1px solid ;
    border-radius: 10px;
    font-size: 1.0625rem;
    line-height: 147.6%;
    padding: 0.2rem;
  }
  .main-div-form {
    font-size: 18px;
    display: inline-flex;
    position: relative;
    padding: 10px;
    border: 2px solid hsla(0, 0%, 30%, .14);
    border-radius: 15px;
  }
  .label-input{
    font-size: 14px;
    display: inline-block;
    margin-right: 10px;
  }
  .label-input input{
    display: block;
    border: 1px solid ;
    border-radius: 10px;
    font-size: 1.0625rem;
    line-height: 147.6%;
    padding: 0.2rem;
  }
  .author-input-block{
    font-size: 18px;
    display: inline-block;
    padding: 10px;
    margin: 15px 10px 10px 15px;
    border: 2px solid hsla(0, 0%, 30%, .14);
    border-radius: 15px;
  }
  .subject-label{
    font-size: 15px;
    display: inline-block;
    padding: 6px;
    margin: 15px 10px 10px 5px;
    border: 2px solid hsla(0, 0%, 30%, .14);
    border-radius: 15px;
  }
  .author {
    margin-bottom: 15px;
  }
  .add-author{
    margin-left: 15px;
  }
  .author-input-block{
    display: block;
  }
  .div_bookshelve-block{
    margin: 15px 10px 10px 15px;
  }
  .add-shelve{
   margin-top: 10px;
  }
  .bookshelve-block input {
    margin-top: 5px;
  }
  .buttons-list{
    margin: 0px 10px 10px 15px;
  }
  .buttons-list button{
    margin-right: 5px;
    alignment: center;
    width:62px;
    height: 30px;
    padding: 0px;
  }
  .button {
    background-color: #FAFBFC;
    border: 1px solid rgba(27, 31, 35, 0.15);
    border-radius: 6px;
    box-sizing: border-box;
    color: #24292E;
    cursor: pointer;
    display: inline-block;
    font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
    font-size: 13px;
    font-weight: 500;
    line-height: 20px;
    list-style: none;
    height: 32px;
    width: 130px;
    padding: 5px 12px;
  }
  .button:hover {
    background-color: hsla(0, 0%, 5%, .14);
    color: white;
  }
</style>