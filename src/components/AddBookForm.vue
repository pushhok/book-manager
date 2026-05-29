<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <h5 class="card-title mb-3">Добавить новую книгу</h5>
      <form @submit.prevent="handleSubmit">
        <div class="mb-3">
          <label class="form-label">Название книги</label>
          <input 
            v-model="formData.title" 
            type="text" 
            class="form-control" 
            placeholder="Введите название" 
            required 
          />
        </div>
        
        <div class="mb-3">
          <label class="form-label">Автор</label>
          <input 
            v-model="formData.author" 
            type="text" 
            class="form-control" 
            placeholder="Введите автора" 
            required 
          />
        </div>
        
        <div class="mb-3">
          <label class="form-label">Жанр</label>
          <select v-model="formData.genre" class="form-select" required>
            <option value="">Выберите жанр</option>
            <option value="Роман">Роман</option>
            <option value="Фантастика">Фантастика</option>
            <option value="Детектив">Детектив</option>
            <option value="Научная">Научная</option>
            <option value="Поэзия">Поэзия</option>
            <option value="Биография">Биография</option>
          </select>
        </div>
        
        <div class="mb-3">
          <label class="form-label">Описание</label>
          <textarea 
            v-model="formData.description" 
            class="form-control" 
            rows="3"
            placeholder="Краткое описание книги (необязательно)"
          ></textarea>
        </div>
        
        <div class="mb-3">
          <label class="form-label">Ссылка на обложку</label>
          <input 
            v-model="formData.imageUrl" 
            type="url" 
            class="form-control" 
            placeholder="https://example.com/cover.jpg"
          />
          <div class="form-text">Введите URL изображения книги</div>
        </div>
        
        <button type="submit" class="btn btn-primary w-100">
          ➕ Добавить в коллекцию
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: '',
  description: '',
  imageUrl: ''
})

const handleSubmit = () => {
  emit('add-book', { ...formData })
  Object.keys(formData).forEach(key => {
    formData[key] = ''
  })
}
</script>