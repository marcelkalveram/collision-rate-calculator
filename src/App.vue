<script setup lang="ts">
import { computed } from 'vue'
import { useForm } from '@tanstack/vue-form'
import FormField from './FormField.vue'

const form = useForm({
  defaultValues: {
    updateRatePerSecond: 20,
    numberOfInstances: 5,
    cacheSize: 50000,
    replicationLatency: 100,
  },
})

const HOUR_IN_SECONDS = 3600
const SECOND_IN_MILLISECONDS = 1000
const MILLISECONDS_TO_HOUR = HOUR_IN_SECONDS / SECOND_IN_MILLISECONDS

// store values
const updateRatePerSecond = form.useStore((state) => state.values.updateRatePerSecond)
const numberOfInstances = form.useStore((state) => state.values.numberOfInstances)
const cacheSize = form.useStore((state) => state.values.cacheSize)
const replicationLatency = form.useStore((state) => state.values.replicationLatency)

// computed values
const collisionRate = computed(
  () =>
    MILLISECONDS_TO_HOUR *
    ((numberOfInstances.value * updateRatePerSecond.value ** 2) / cacheSize.value) *
    replicationLatency.value,
)
</script>

<template>
  <main class="wrapper">
    <h1>Cache Collision Rate Calculator</h1>
    <p>
      This tool calculates the rate of data collisions in a replicated caching system between
      service instances in a space-based architecture. Adjust the parameters below to see how
      changes affect the collision rate.
    </p>

    <form class="form">
      <form.Field
        name="updateRatePerSecond"
        :validators="{
          onChange: ({ value }) =>
            isNaN(value) || value <= 0 ? 'Value must be a number greater than 0' : null,
        }"
      >
        <template v-slot="{ field }">
          <FormField
            label="Update rate per second"
            description="The rate at which the collision calculations are updated, in milliseconds."
            :name="field.name"
            :state="field.state"
            @handleChange="field.handleChange"
          />
        </template>
      </form.Field>

      <form.Field
        name="numberOfInstances"
        :validators="{
          onChange: ({ value }) =>
            isNaN(value) || value <= 0 ? 'Value must be a number greater than 0' : null,
        }"
      >
        <template v-slot="{ field }">
          <FormField
            label="Number of instances"
            description="The amount of services containing a replicated cache"
            :name="field.name"
            :state="field.state"
            @handleChange="field.handleChange"
          />
        </template>
      </form.Field>

      <form.Field
        name="cacheSize"
        :validators="{
          onChange: ({ value }) =>
            isNaN(value) || value <= 0 ? 'Value must be a number greater than 0' : null,
        }"
      >
        <template v-slot="{ field }">
          <FormField
            label="Cache size"
            description="The size of cache in number of rows"
            :name="field.name"
            :state="field.state"
            @handleChange="field.handleChange"
          />
        </template>
      </form.Field>

      <form.Field
        name="replicationLatency"
        :validators="{
          onChange: ({ value }) =>
            isNaN(value) || value <= 0 ? 'Value must be a number greater than 0' : null,
        }"
      >
        <template v-slot="{ field }">
          <FormField
            label="Replication latency"
            description="The latency for replication in milliseconds"
            :name="field.name"
            :state="field.state"
            @handleChange="field.handleChange"
          />
        </template>
      </form.Field>
    </form>

    <b>Updates per hour</b>
    <form.Subscribe>
      <template v-slot="{ values }">
        <p>{{ values.updateRatePerSecond * HOUR_IN_SECONDS }} / hour</p>
      </template>
    </form.Subscribe>

    <h2>Collision rate</h2>
    <p>
      The collision rate is the rate at which two or more service instances attempt to update the
      same row in the cache at the same time.
    </p>
    <p>
      <b>{{ collisionRate }} / hour</b>
    </p>
  </main>
</template>

<style scoped>
.wrapper {
  padding: 1rem;
  max-width: 640px;
  margin: 0 auto;
  background-color: aliceblue;
  font-family: 'Courier New', Courier, monospace;
}

.form {
  display: grid;
  gap: 1rem;
  margin-bottom: 1rem;
}
</style>
