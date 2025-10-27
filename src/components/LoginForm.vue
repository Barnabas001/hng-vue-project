<template>
  <section aria-labelledby="login-heading" class="form-wrap">
    <h2 id="login-heading">Login</h2>
    <form
      @submit.prevent="handleSubmit"
      aria-describedby="login-help"
      class="form"
    >
      <label class="label">
        Username
        <input
          v-model="username"
          aria-label="username"
          class="input"
          autocomplete="username"
        />
      </label>
      <label class="label">
        Password
        <input
          type="password"
          v-model="password"
          aria-label="password"
          class="input"
          autocomplete="current-password"
        />
      </label>

      <div class="form-action">
        <button type="button" class="button" @click="$emit('back')">
          Back
        </button>
        <button type="submit" :disabled="loading">
          {{ loading ? "Signing in..." : "Sign in" }}
        </button>
      </div>

      <p id="login-help" class="help-text">
        Demo creds: <strong>admin</strong> / <strong>1234</strong>
      </p>
      <p v-if="error" style="color: #b00020">{{ error }}</p>
    </form>
  </section>
</template>

<script setup>
import { ref, watch } from "vue";
const username = ref("");
const password = ref("");
const error = ref("");
const loading = ref(false);

watch([username, password], () => {
  error.value = "";
});

function handleSubmit() {
  if (!username.value.trim() || !password.value) {
    error.value = "Please enter both username and password.";
    return;
  }
  loading.value = true;
  setTimeout(() => {
    loading.value = false;
    if (username.value === "admin" && password.value === "1234") {
      emit("success");
    } else {
      error.value = "Invalid credentials. Try admin / 1234.";
    }
  }, 600);
}
const emit = defineEmits(["success", "back"]);
</script>
