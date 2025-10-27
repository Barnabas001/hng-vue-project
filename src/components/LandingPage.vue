<template>
  <section aria-labelledby="hero-title" class="hero-section">
    <img
      alt="decorative wave"
      src="/src/assets/hero-wave.svg"
      class="hero-wave"
    />
    <div class="hero-content">
      <h1 id="hero-title" class="hero-title">
        Manage tickets. Resolve faster.
      </h1>
      <p class="lead">
        A simple ticket management starter app. Build, track and close tickets
        with ease.
      </p>
      <div class="hero-actions">
        <button @click="$emit('login')">Get Started</button>
        <a href="#docs">View Docs</a>
      </div>
    </div>
  </section>
</template>
