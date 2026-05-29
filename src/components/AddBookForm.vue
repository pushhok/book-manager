<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <h5 class="card-title mb-3">Добавить новую книгу</h5>
      <form @submit.prevent="handleSubmit">
        <div class="mb-3">
          <input v-model="formData.title" type="text" class="form-control" placeholder="Название" required />
        </div>
        <div class="mb-3">
          <input v-model="formData.author" type="text" class="form-control" placeholder="Автор" required />
        </div>
        <div class="mb-3">
          <select v-model="formData.genre" class="form-select" required>
            <option value="">Выберите жанр</option>
            <option value="Роман">Роман</option>
            <option value="Фантастика">Фантастика</option>
            <option value="Детектив">Детектив</option>
            <option value="Научная">Научная</option>
          </select>
        </div>
        <div class="mb-3">
          <textarea v-model="formData.description" class="form-control" rows="2" placeholder="Описание (необязательно)"></textarea>
        </div>
        <div class="mb-3">
          <input v-model="formData.imageUrl" type="url" class="form-control" placeholder="Ссылка на фото (необязательно)" />
        </div>
        <button type="submit" class="btn btn-primary w-100">Добавить</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
const emit = defineEmits(['add-book'])
const formData = reactive({ title: '', author: '', genre: '', description: '', imageUrl: '' })

const handleSubmit = () => {
  emit('add-book', { ...formData })
  Object.keys(formData).forEach(k => formData[k] = '')
}
</script>