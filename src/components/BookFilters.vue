<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <div class="row g-2 align-items-center">
        <div class="col-md-6">
          <div class="input-group">
            <span class="input-group-text">🔍</span>
            <input v-model="searchQuery" type="text" class="form-control" placeholder="Поиск..." />
          </div>
        </div>
        <div class="col-md-3">
          <select v-model="sortBy" class="form-select">
            <option value="date">По дате</option>
            <option value="title">По названию</option>
            <option value="author">По автору</option>
            <option value="rating">По рейтингу</option>
          </select>
        </div>
        <div class="col-md-3 btn-group">
          <button class="btn" :class="filter === 'all' ? 'btn-primary' : 'btn-outline-primary'" @click="$emit('update:filter', 'all')">Все</button>
          <button class="btn" :class="filter === 'unread' ? 'btn-primary' : 'btn-outline-primary'" @click="$emit('update:filter', 'unread')">Нет</button>
          <button class="btn" :class="filter === 'read' ? 'btn-primary' : 'btn-outline-primary'" @click="$emit('update:filter', 'read')">Да</button>
        </div>
      </div>
      <div class="mt-2 text-muted small">
        Всего: {{ total }} | Прочитано: {{ completed }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
const props = defineProps(['filter', 'books'])
defineEmits(['update:filter'])
const searchQuery = defineModel('searchQuery')
const sortBy = defineModel('sortBy')
const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>