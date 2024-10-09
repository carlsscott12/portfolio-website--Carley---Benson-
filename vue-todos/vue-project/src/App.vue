<template>
  <div id="app">
    <h1>📚 Book Categorizer</h1>

    <form @submit.prevent="addBook" class="book-form">
      <div class="form-group">
        <input
          v-model="newBook.title"
          class="input-field"
          placeholder="Book Title"
          required
        />
      </div>

      <div class="form-group">
        <!-- Toggle between predefined and custom category -->
        <label>
          <input type="checkbox" v-model="customCategory" /> Use custom category
        </label>

        <!-- Predefined categories dropdown -->
        <select v-if="!customCategory" v-model="newBook.category" class="select-field" required>
          <option value="" disabled>Select Category</option>
          <option v-for="category in predefinedCategories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>

        <!-- Custom category input -->
        <input
          v-if="customCategory"
          v-model="newBook.category"
          class="input-field"
          placeholder="Custom Category"
          required
        />
      </div>

      <button type="submit" class="btn-primary">Add Book</button>
    </form>

    <div class="book-list">
      <h2>All Books</h2>
      <div v-if="books.length" v-for="(book, index) in books" :key="index" class="book">
        <p><strong>Title:</strong> {{ book.title }}</p>
        <p><strong>Category:</strong> {{ book.category }}</p>
      </div>
      <div v-else>
        <p class="no-books">No books added yet.</p>
      </div>
    </div>

    <div class="filter-category">
      <h2>Filter by Category</h2>
      <select v-model="selectedCategory" class="select-field">
        <option value="">All</option>
        <option v-for="category in categories" :key="category" :value="category">
          {{ category }}
        </option>
      </select>

      <div class="filtered-books">
        <h3>Books in {{ selectedCategory || 'All Categories' }}</h3>
        <div v-if="filteredBooks.length" v-for="book in filteredBooks" :key="book.title" class="filtered-book">
          <p><strong>Title:</strong> {{ book.title }}</p>
          <p><strong>Category:</strong> {{ book.category }}</p>
        </div>
        <div v-else>
          <p class="no-books">No books found for this category.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newBook: {
        title: '',
        category: '',
      },
      books: [],
      selectedCategory: '',
      predefinedCategories: ['Fiction', 'Non-Fiction', 'Science', 'History', 'Fantasy', 'Mystery', 'Biography'],
      customCategory: false, // To toggle between predefined and custom category input
    };
  },
  computed: {
    categories() {
      return [...new Set(this.books.map(book => book.category))];
    },
    filteredBooks() {
      if (!this.selectedCategory) {
        return this.books;
      }
      return this.books.filter(book => book.category === this.selectedCategory);
    },
  },
  methods: {
    addBook() {
      if (this.newBook.title && this.newBook.category) {
        this.books.push({ ...this.newBook });
        this.newBook.title = '';
        this.newBook.category = '';
        this.customCategory = false; // Reset to predefined category after submission
      }
    },
  },
};
</script>

<style scoped>
/* Same style from the previous version, you can copy the style from the previous message */
</style>
>
