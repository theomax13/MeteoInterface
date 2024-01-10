<!-- NavBarComponent.vue -->
<template>
  <div id="app">
    <nav
      class="navbar navbar-expand-lg"
      style="background-color: rgb(64, 133, 88)"
    >
      <div class="container">
        <a class="navbar-brand text-white" href="#">Capteurs temperatures</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarTogglerDemo02"
          aria-controls="navbarTogglerDemo02"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item">
              <button
                class="nav-link btn text-white"
                @click="switchToHistorique"
              >
                Historique
              </button>
            </li>
            <li class="nav-item">
              <button class="nav-link btn text-white" @click="switchToLive">
                Live
              </button>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  methods: {
    switchToHistorique() {
      this.$emit("switchDisplay", true);
    },
    switchToLive() {
      this.$emit("switchDisplay", false);
    },
  },
};
</script>
