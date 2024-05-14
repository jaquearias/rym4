<template>
  <div>
    <!-- Botón para mostrar/ocultar la barra lateral en pantallas pequeñas -->
    <b-button @click="toggleSidebar" variant="primary" class="d-block d-md-none">
      {{ sidebarOpen ? 'Cerrar Sidebar' : 'Abrir Sidebar' }}
    </b-button>

    <!-- Contenedor de la barra lateral -->
    <b-sidebar v-model="sidebarOpen" id="sidebar" title="Sidebar" shadow>
      <!-- Contenido de la barra lateral -->
      <ul>
        <li>Enlace 1</li>
        <li>Enlace 2</li>
        <li>Enlace 3</li>
        <!-- Agrega más enlaces según sea necesario -->
      </ul>
    </b-sidebar>

    <!-- Contenido principal -->
    <div class="main-content">
      <!-- Contenido principal de la página -->
      <p>Contenido principal de la página.</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sidebarOpen: false
    };
  },
  methods: {
    toggleSidebar() {
      // Alternar el estado de la barra lateral (abierta/cerrada)
      this.sidebarOpen = !this.sidebarOpen;
    }
  }
};
</script>

<style scoped>
.main-content {
  margin-left: 250px; /* Ajusta el margen izquierdo para dejar espacio para la barra lateral */
  transition: margin-left 0.3s; /* Agrega una transición para suavizar el cambio de margen */
}

@media (max-width: 767px) {
  .main-content {
    margin-left: 0; /* Elimina el margen izquierdo en pantallas pequeñas */
  }
}
</style>
