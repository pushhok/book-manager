<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <div class="row g-3 align-items-center">
        <div class="col-md-5">
          <div class="input-group">
            <span class="input-group-text">🔍</span>
            <input 
              v-model="searchQuery" 
              type="text" 
              class="form-control" 
              placeholder="Поиск по названию или автору..."
            />
          </div>
        </div>
        
        <div class="col-md-3">
          <select v-model="sortBy" class="form-select">
            <option value="date">По дате добавления</option>
            <option value="title">По названию</option>
            <option value="author">По автору</option>
            <option value="rating">По рейтингу</option>
          </select>
        </div>
        
        <div class="col-md-4">
          <div class="btn-group w-100" role="group">
            <button 
              type="button" 
              class="btn" 
              :class="filter === 'all' ? 'btn-primary' : 'btn-outline-primary'"
              @click="$emit('update:filter', 'all')"
            >
              Все
            </button>
            <button 
              type="button" 
              class="btn" 
              :class="filter === 'unread' ? 'btn-primary' : 'btn-outline-primary'"
              @click="$emit('update:filter', 'unread')"
            >
              Непрочитанные
            </button>
            <button 
              type="button" 
              class="btn" 
              :class="filter === 'read' ? 'btn-primary' : 'btn-outline-primary'"
              @click="$emit('update:filter', 'read')"
            >
              Прочитанные
            </button>
          </div>
        </div>
      </div>
      
      <div class="mt-3 pt-3 border-top">
        <div class="row text-center">
          <div class="col-4">
            <h6 class="mb-0 text-primary">{{ total }}</h6>
            <small class="text-muted">Всего книг</small>
          </div>
          <div class="col-4">
            <h6 class="mb-0 text-success">{{ completed }}</h6>
            <small class="text-muted">Прочитано</small>
          </div>
          <div class="col-4">
            <h6 class="mb-0 text-warning">{{ total - completed }}</h6>
            <small class="text-muted">Осталось</small>
          </div>
        </div>
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