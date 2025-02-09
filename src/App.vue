<script setup lang="ts">
import { computed, ref } from 'vue'

const updateRatePerSecond = ref(20)
const numberOfInstances = ref(5)
const cacheSize = ref(50000)
const replicationLatencyPerMillisecond = ref(100)

const MS_TO_HOUR = 3600 / 1000

const updatesPerHour = computed(() => updateRatePerSecond.value * 3600)
const collisionRate = computed(
  () =>
    MS_TO_HOUR *
    ((numberOfInstances.value * updateRatePerSecond.value ** 2) / cacheSize.value) *
    replicationLatencyPerMillisecond.value,
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
      <div class="form-field">
        <label class="label" for="updateRate">Update rate</label>
        <small>The rate at which the collision calculations are updated, in milliseconds.</small>
        <input
          id="updateRate"
          type="text"
          v-model="updateRatePerSecond"
          placeholder="Update rate in ms"
        />
      </div>

      <div class="form-field">
        <label class="label" for="updateRate">Number of instances</label>
        <small>The amount of services containing a replicated cache</small>
        <input
          id="updateRate"
          type="text"
          v-model="numberOfInstances"
          placeholder="Number of instances"
        />
      </div>

      <div class="form-field">
        <label class="label" for="cacheSize">Cache size</label>
        <small>The size of cache in number of rows</small>
        <input id="cacheSize" type="text" v-model="cacheSize" placeholder="Cache size" />
      </div>

      <div class="form-field">
        <label class="label" for="replicationLatency">Replication latency</label>
        <small>The latency for replication in milliseconds</small>
        <input
          id="replicationLatency"
          type="text"
          v-model="replicationLatencyPerMillisecond"
          placeholder="Replication latency"
        />
      </div>
    </form>

    <b>Updates per hour</b>
    <p>{{ updatesPerHour }} / hour</p>

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

.form-field {
  display: grid;
  gap: 0.5rem;
}

.label {
  font-weight: bold;
}
</style>
