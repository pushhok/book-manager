<template>
  <div class="book-card" :class="{ completed: book.completed }">
    <img v-if="book.imageUrl" :src="book.imageUrl" class="book-cover" alt="cover" />
    <div class="book-info">
      <h3>{{ book.title }}</h3>
      <p class="author">{{ book.author }} • {{ book.genre }}</p>
      <p v-if="book.description" class="description">{{ book.description }}</p>
    </div>
    <div class="book-actions">
      <div v-if="book.completed" class="rating">
        <span v-for="star in 5" :key="star" @click="$emit('rate', star)">
          {{ star <= book.rating ? '★' : '☆' }}
        </span>
      </div>
      <button @click="$emit('toggle')" :class="['btn', book.completed ? 'btn-secondary' : 'btn-primary']">
        {{ book.completed ? '✓ Прочитано' : 'Читать' }}
      </button>
      <button @click="$emit('delete')" class="btn btn-danger">✕</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>

<style scoped>
.book-card {
  background: white;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  display: flex;
  flex-direction: column;
  gap: 12px;
  transition: all 0.3s;
}
.book-card.completed { background: #f0f7f0; opacity: 0.9; }
.book-cover {
  width: 100%;
  max-height: 160px;
  object-fit: cover;
  border-radius: 6px;
}
.book-info { flex: 1; }
.book-info h3 { margin-bottom: 4px; color: #333; }
.author { color: #666; font-size: 0.9em; margin-bottom: 4px; }
.genre { background: #e0e0e0; padding: 2px 8px; border-radius: 4px; font-size: 0.8em; }
.description { color: #555; font-size: 0.9em; margin-top: 8px; line-height: 1.4; }
.book-actions { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
.rating { display: flex; gap: 2px; }
.rating span { font-size: 20px; cursor: pointer; color: gold; }
.rating span:hover { transform: scale(1.2); }
.btn { padding: 8px 12px; border: none; border-radius: 4px; cursor: pointer; font-size: 0.9em; transition: background 0.3s; }
.btn-primary { background: #4CAF50; color: white; }
.btn-primary:hover { background: #45a049; }
.btn-secondary { background: #2196F3; color: white; }
.btn-secondary:hover { background: #1e87db; }
.btn-danger { background: #f44336; color: white; }
.btn-danger:hover { background: #da190b; }
</style>