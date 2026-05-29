<template>
  <div class="container mt-5">
    <header class="text-center mb-5 p-4 bg-primary text-white rounded shadow">
      <h1 class="display-4">Менеджер книг</h1>
      <p class="lead">Практическое занятие 15: Vue + Bootstrap</p>
    </header>

    <div class="row">
      <div class="col-md-8 offset-md-2">
        <!-- Форма добавления -->
        <AddBookForm @add-book="addBook"/>
        
        <!-- Фильтры -->
        <BookFilters
          v-model:searchQuery="searchQuery"
          v-model:filter="currentFilter"
          v-model:sortBy="currentSort"
          :books="books"
        />

        <!-- Список книг -->
        <div v-if="filteredBooks.length === 0" class="alert alert-info text-center">
          Книги не найдены. Добавьте что-нибудь новенькое!
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
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const savedBooks = localStorage.getItem('books')
if (savedBooks) books.value = JSON.parse(savedBooks)

const currentFilter = ref('all')
const searchQuery = ref('')
const currentSort = ref('date')

watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

const addBook = (bookData) => {
  books.value.push({ id: Date.now(), ...bookData, completed: false, rating: 0 })
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) book.rating = 0
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) book.rating = rating
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) books.value = books.value.filter(b => b.id !== id)
}

const filteredBooks = computed(() => {
  let result = books.value.filter(book => {
    if (currentFilter.value === 'unread') return !book.completed
    if (currentFilter.value === 'read') return book.completed
    return true
  })

  result = result.filter(book => {
    if (!searchQuery.value) return true
    const q = searchQuery.value.toLowerCase()
    return book.title.toLowerCase().includes(q) || book.author.toLowerCase().includes(q)
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