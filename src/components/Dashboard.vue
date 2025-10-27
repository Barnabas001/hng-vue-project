<template>
  <section aria-labelledby="dashboard-heading" class="dashboard">
    <h2 id="dashboard-heading">Dashboard</h2>
    <p>Summary of tickets and quick actions.</p>

    <div class="stats-row">
      <div class="stat-card">
        <h3>{{ total }}</h3>
        <p>Total Tickets</p>
      </div>
      <div class="stat-card">
        <h3>{{ open }}</h3>
        <p>Open</p>
      </div>
      <div class="stat-card">
        <h3>{{ resolved }}</h3>
        <p>Resolved</p>
      </div>
    </div>

    <div style="margin-top: 20px; display: flex; gap: 12px; flex-wrap: wrap">
      <button class="button" @click="$emit('openTickets')">
        Manage Tickets
      </button>
      <button class="button" @click="$emit('seed')">Seed Demo Tickets</button>
    </div>
  </section>
</template>

<script setup>
import { computed } from "vue";
const props = defineProps({ tickets: Array });
const total = computed(() => props.tickets.length);
const open = computed(
  () => props.tickets.filter((t) => t.status === "open").length
);
const resolved = computed(
  () =>
    props.tickets.filter(
      (t) => t.status === "resolved" || t.status === "closed"
    ).length
);
</script>
