<script lang="ts" setup>
type BaseProps = {
  title: string;
};

type NotificationSuccess = {
  type: 'success';
  variant: 'primary' | 'secondary';
  retryable?: never;
  message: string;
  duration: number;     // Auto-hide duration
  errorCode?: never;
}

type NotificationError = {
  type: 'error';
  variant: 'danger' | 'warning';
  errorCode: string;
  retryable: boolean;   // Show retry button
  duration?: never;     // Errors don't auto-hide
  message?: never;
}

type NotificationProps = NotificationError | NotificationSuccess;

type Props = BaseProps & NotificationProps;

const props = defineProps<Props>();
const emit = defineEmits<{
  (e: 'retry'): void;
  (e: 'close'): void;
}>();

// Auto-hide for success notifications
if (props.type === 'success' && props.duration) {
  setTimeout(() => {
    emit('close');
  }, props.duration);
}
</script>

<template>
  <div 
    class="notification" 
    :class="[type, variant]"
  >
    <div class="notification-content">
      <h3>{{ title }}</h3>
      
      <template v-if="type === 'success'">
        <p>{{ message }}</p>
      </template>
      
      <template v-if="type === 'error'">
        <p class="error-code">Error Code: {{ errorCode }}</p>
        <button 
          v-if="retryable"
          class="retry-button"
          @click="emit('retry')"
        >
          Retry
        </button>
      </template>
    </div>
    
    <button 
      class="close-button"
      @click="emit('close')"
    >
      ×
    </button>
  </div>
</template>

<style scoped>
.notification {
  padding: 1rem;
  border-radius: 0.5rem;
  margin-bottom: 1rem;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
}

.notification-content {
  flex: 1;
}

.success.primary {
  background-color: #ecfdf5;
  border: 1px solid #10b981;
}

.success.secondary {
  background-color: #f8fafc;
  border: 1px solid #94a3b8;
}

.error.danger {
  background-color: #fef2f2;
  border: 1px solid #ef4444;
}

.error.warning {
  background-color: #fffbeb;
  border: 1px solid #f59e0b;
}

h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.1rem;
  font-weight: 600;
}

p {
  margin: 0;
}

.error-code {
  font-family: monospace;
  background: rgba(0, 0, 0, 0.05);
  padding: 0.25rem 0.5rem;
  border-radius: 0.25rem;
  display: inline-block;
}

.retry-button {
  margin-top: 0.75rem;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  border: none;
  background: #3b82f6;
  color: white;
  cursor: pointer;
}

.retry-button:hover {
  background: #2563eb;
}

.close-button {
  background: transparent;
  border: none;
  font-size: 1.5rem;
  line-height: 1;
  padding: 0;
  cursor: pointer;
  opacity: 0.5;
}

.close-button:hover {
  opacity: 1;
}
</style>