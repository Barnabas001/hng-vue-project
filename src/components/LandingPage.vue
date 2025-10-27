<template>
  <section aria-labelledby="hero-title" class="hero-section">
    <img
      alt="decorative wave"
      src="/src/assets/hero-wave.svg"
      class="hero-wave"
    />
    <div class="hero-content">
      <h1 id="hero-title" class="hero-title">
        My Ticket Management Platform
      </h1>
      <p class="lead">
        Easy to use ticket management starter platform. Supper User friendly.
      </p>
      <div class="hero-actions">
        <button @click="$emit('login')">Get Started</button>
        <a href="#docs">View Docs</a>
      </div>
    </div>
  </section>
</template>
