<template>
  <header class="header">
    <div class="logo">Ticketly</div>
    <nav aria-label="Main Navigation">
      <button class="link" @click="$emit('navigate', 'landing')">Home</button>
      <button
        class="link"
        @click="$emit('navigate', auth ? 'dashboard' : 'login')"
      >
        Dashboard
      </button>
      <button class="link" @click="$emit('navigate', 'tickets')">
        Tickets
      </button>
      <button v-if="auth" class="link" @click="$emit('logout')">Logout</button>
      <button v-else class="link" @click="$emit('navigate', 'login')">
        Login
      </button>
    </nav>
  </header>
</template>

<script setup>
defineProps({ auth: Boolean });
</script>
