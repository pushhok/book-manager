<template>
  <div class="col-md-4 mb-4">
    <div class="card h-100 shadow-sm" :class="{ 'border-success': book.completed }">
      <img 
        v-if="book.imageUrl" 
        :src="book.imageUrl" 
        class="card-img-top" 
        style="height: 250px; object-fit: cover;"
        alt="Обложка книги"
        @error="$event.target.style.display='none'"
      >
      <div class="card-body">
        <div class="d-flex justify-content-between align-items-start mb-2">
          <h5 class="card-title mb-0">{{ book.title }}</h5>
          <button 
            @click="$emit('delete')" 
            class="btn-close"
            title="Удалить"
          ></button>
        </div>
        
        <h6 class="card-subtitle mb-2 text-muted"> {{ book.author }}</h6>
        
        <span class="badge bg-info text-dark mb-2">{{ book.genre }}</span>
        
        <p v-if="book.description" class="card-text small text-muted">
          {{ book.description }}
        </p>
        
        <div v-if="book.completed" class="mt-3">
          <label class="form-label small">Ваша оценка:</label>
          <div class="text-warning" style="font-size: 1.5em; cursor: pointer;">
            <span 
              v-for="star in 5" 
              :key="star" 
              @click="$emit('rate', star)"
              class="me-1"
            >
              {{ star <= book.rating ? '★' : '☆' }}
            </span>
          </div>
          <small class="text-muted">{{ book.rating }}/5</small>
        </div>
        
        <div class="mt-3">
          <button 
            @click="$emit('toggle')" 
            class="btn w-100" 
            :class="book.completed ? 'btn-outline-success' : 'btn-outline-primary'"
          >
            {{ book.completed ? 'Прочитано' : 'Отметить прочитанной' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>