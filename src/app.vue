<template>
  <div class="main-container">
    <Header :auth="auth" @navigate="view = $event" @logout="handleLogout" />

    <main class="sub-main">
      <LandingPage v-if="view === 'landing'" @login="view = 'login'" />
      <LoginForm
        v-if="view === 'login'"
        @success="onLoginSuccess"
        @back="view = 'landing'"
      />
      <Dashboard
        v-if="view === 'dashboard' && auth"
        :tickets="tickets"
        @seed="seedDemoTickets"
        @openTickets="view = 'tickets'"
      />
      <TicketsPage
        v-if="view === 'tickets' && auth"
        :tickets="tickets"
        @update="updateTickets"
        @back="view = 'dashboard'"
      />
      <div v-if="!auth && view !== 'landing' && view !== 'login'" role="status">
        Unauthorized — redirecting to login...
      </div>
    </main>

    <footer class="footer">
      <small>Ticket Management — Vue Version</small>
    </footer>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from "vue";
import Header from "./components/Header.vue";
import LandingPage from "./components/LandingPage.vue";
import LoginForm from "./components/LoginForm.vue";
import Dashboard from "./components/Dashboard.vue";
import TicketsPage from "./components/TicketsPage.vue";

const TICKETS_KEY = "tm_tickets";
const TOKEN_KEY = "tm_token";

const view = ref("landing");
const auth = ref(Boolean(localStorage.getItem(TOKEN_KEY)));
const tickets = ref([]);

// Load tickets
onMounted(() => {
  const raw = localStorage.getItem(TICKETS_KEY);
  if (raw) {
    try {
      tickets.value = JSON.parse(raw);
    } catch {
      tickets.value = [];
    }
  }
  if (auth.value) view.value = "dashboard";
});

// Persist tickets
watch(
  tickets,
  (val) => {
    localStorage.setItem(TICKETS_KEY, JSON.stringify(val));
  },
  { deep: true }
);

function handleLogout() {
  localStorage.removeItem(TOKEN_KEY);
  auth.value = false;
  view.value = "landing";
}

function onLoginSuccess() {
  localStorage.setItem(TOKEN_KEY, "sample_token");
  auth.value = true;
  view.value = "dashboard";
}

function seedDemoTickets() {
  const demo = [
    {
      id: "t1",
      title: "Cannot login",
      description: "User reports login failure.",
      status: "open",
      createdAt: Date.now(),
    },
    {
      id: "t2",
      title: "UI bug on dashboard",
      description: "Chart not rendering.",
      status: "resolved",
      createdAt: Date.now() - 3600000,
    },
    {
      id: "t3",
      title: "Feature request: export",
      description: "Add CSV export.",
      status: "open",
      createdAt: Date.now() - 86400000,
    },
  ];
  tickets.value = demo;
}

function updateTickets(newList) {
  tickets.value = newList;
}
</script>
