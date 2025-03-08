<script setup lang="ts">
defineProps<{
  name: string
  label: string
  description: string
  state: {
    value: number
    meta: {
      errors: (string | null | undefined)[]
    }
  }
}>()

defineEmits<{
  (event: 'handleChange', value: string): void
}>()
</script>

<template>
  <div class="form-field">
    <label class="label" for="updateRate">{{ label }}</label>
    <small>{{ description }}</small>
    <input
      :name="name"
      :id="name"
      :value="state?.value"
      @input="
        (event: Event) => {
          $emit('handleChange', (event.target as HTMLInputElement).value)
        }
      "
      type="text"
      placeholder="Update rate in ms"
    />
    <p class="errors" v-if="state.meta.errors.length">
      <span class="error" :key="index" v-for="(error, index) in state.meta.errors">
        {{ error }}
      </span>
    </p>
  </div>
</template>

<style scoped>
.form-field {
  display: grid;
  gap: 0.5rem;
}

.label {
  font-weight: bold;
}

.errors {
  margin: 0;
  color: red;
}

.error {
  font-size: 0.8rem;
  font-weight: bold;
}
</style>
