<script lang="ts" setup>
import { ref } from 'vue';
import NotificationAlert from './components/NotificationAlert.vue';

const showSuccessNotification = ref(true);
const showErrorNotification = ref(true);

const handleRetry = () => {
  console.log('Retrying operation...');
};

const handleCloseSuccess = () => {
  showSuccessNotification.value = false;
};

const handleCloseError = () => {
  showErrorNotification.value = false;
};
</script>

<template>
  <div class="container">
    <h2>Notification Examples</h2>
    
    <NotificationAlert
      v-if="showSuccessNotification"
      type="success"
      variant="primary"
      title="Data Saved"
      message="Your changes have been saved successfully"
      :duration="5000"
      @close="handleCloseSuccess"
    />
    
    <NotificationAlert
      v-if="showErrorNotification"
      type="error"
      variant="danger"
      title="Upload Failed"
      errorCode="UPLOAD_001"
      :retryable="true"
      @retry="handleRetry"
      @close="handleCloseError"
    />
    
    <div class="buttons">
      <button @click="showSuccessNotification = true">
        Show Success
      </button>
      <button @click="showErrorNotification = true">
        Show Error
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  padding: 2rem;
  max-width: 600px;
  margin: 0 auto;
}

h2 {
  margin-bottom: 2rem;
}

.buttons {
  margin-top: 2rem;
  display: flex;
  gap: 1rem;
}

button {
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  border: 1px solid #e2e8f0;
  background: white;
  cursor: pointer;
}

button:hover {
  background: #f8fafc;
}
</style>