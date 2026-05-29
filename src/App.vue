<template>
  <div class="container mt-5">
    <header class="text-center mb-5">
      <h1 class="display-4 text-primary">Менеджер книг</h1>
      <p class="lead text-muted">Практическое занятие 15 (Vue 3 + Bootstrap)</p>
    </header>

    <main>
      <AddBookForm @add-book="addBook"/>
      
      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        v-model:sortBy="currentSort"
        :books="books"
      />

      <div v-if="filteredBooks.length === 0" class="alert alert-info text-center mt-3">
        <h5>Книги не найдены</h5>
        <p>Добавьте первую книгу или измените параметры поиска</p>
      </div>

      <div v-else class="row">
        <BookCard
          v-for="book in filteredBooks"
          :key="book.id"
          :book="book"
          @toggle="toggleBook(book.id)"
          @delete="deleteBook(book.id)"
          @rate="rateBook(book.id, $event)"
        />
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  books.value = JSON.parse(savedBooks)
}

const currentFilter = ref('all')
const searchQuery = ref('')
const currentSort = ref('date')

watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0
  }
  books.value.push(newBook)
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) {
      book.rating = 0
    }
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) {
    book.rating = rating
  }
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

const filteredBooks = computed(() => {
  let result = books.value.filter(book => {
    if (currentFilter.value === 'unread') return !book.completed
    if (currentFilter.value === 'read') return book.completed
    return true
  })

  result = result.filter(book => {
    if (!searchQuery.value) return true
    const query = searchQuery.value.toLowerCase()
    return book.title.toLowerCase().includes(query) || 
           book.author.toLowerCase().includes(query)
  })

  result.sort((a, b) => {
    if (currentSort.value === 'title') return a.title.localeCompare(b.title)
    if (currentSort.value === 'author') return a.author.localeCompare(b.author)
    if (currentSort.value === 'rating') return b.rating - a.rating
    return b.id - a.id
  })

  return result
})
</script>

<style scoped>
header {
  padding: 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 10px;
  color: white;
}
</style>