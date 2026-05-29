<template>
  <div class="col-md-4 mb-4">
    <div class="card h-100 shadow-sm" :class="{ 'border-success': book.completed }">
      <img v-if="book.imageUrl" :src="book.imageUrl" class="card-img-top" style="height: 200px; object-fit: cover;" alt="cover">
      <div class="card-body">
        <h5 class="card-title">{{ book.title }}</h5>
        <h6 class="card-subtitle mb-2 text-muted">{{ book.author }}</h6>
        <span class="badge bg-info text-dark mb-2">{{ book.genre }}</span>
        <p class="card-text small" v-if="book.description">{{ book.description }}</p>
        
        <div v-if="book.completed" class="mt-2 text-warning fs-4" style="cursor: pointer;">
          <span v-for="star in 5" :key="star" @click="$emit('rate', star)">
            {{ star <= book.rating ? '★' : '☆' }}
          </span>
        </div>
        
        <div class="d-grid gap-2 mt-3">
          <button @click="$emit('toggle')" class="btn" :class="book.completed ? 'btn-outline-success' : 'btn-outline-primary'">
            {{ book.completed ? 'Прочитано ✓' : 'Отметить прочитанной' }}
          </button>
          <button @click="$emit('delete')" class="btn btn-outline-danger btn-sm">Удалить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>