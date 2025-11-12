<template>
  <div v-if="visible" class="modal fade show d-block" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered">
      <div class="modal-content rounded-4 border-0 p-3 shadow-lg">
        <div class="modal-header border-0">
          <h5 class="modal-title fw-bold text-primary">
            Lista de Participantes
          </h5>
          <button class="btn-close" @click="$emit('cerrar')"></button>
        </div>

        <div class="modal-body">
          <div class="table-responsive" style="max-height: 60vh; overflow-y: auto;">
            <table class="table table-sm table-hover align-middle">
              <thead class="sticky-top encabezado-tabla">
                <tr>
                  <th>Nombre</th>
                  <th>Cédula</th>
                  <th>Departamento</th>
                  <th>Cargo</th>
                  <th>Activo</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(p, index) in participantes"
                  :key="index"
                  :class="{ 'table-success': p.activo }"
                >
                  <td>{{ p.nombre }}</td>
                  <td>{{ p.cedula }}</td>
                  <td>{{ p.departamento }}</td>
                  <td>{{ p.cargo }}</td>
                  <td>
                    <span
                      class="badge"
                      :class="p.activo ? 'bg-success' : 'bg-secondary'"
                    >
                      {{ p.activo ? 'Sí' : 'No' }}
                    </span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <div class="modal-footer border-0 justify-content-center">
          <button class="btn btn-primary px-4 fw-semibold" @click="$emit('cerrar')">
            Cerrar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalParticipantes",
  props: {
    visible: Boolean,
    participantes: Array,
  },
};
</script>

<style scoped>
.modal-content {
  background: linear-gradient(180deg, rgba(245, 248, 255, 0.98), rgba(235, 240, 250, 0.97)); /* Fondo claro azulado */
  color: #1e2a38; /* Texto gris oscuro elegante */
  border-radius: 1rem;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(0, 56, 112, 0.3);
  backdrop-filter: blur(6px);
}

/* Encabezado */
.modal-header {
  border-bottom: 2px solid rgba(0, 56, 112, 0.15);
}

.modal-header h5 {
  font-weight: 700;
  color: #003870;
}

/* Tabla */
.encabezado-tabla {
  background: #003870;
  color: #fff;
  top: 0;
  font-weight: 600;
}

.table {
  color: #1a1a1a;
  background: transparent;
}

.table-hover tbody tr:hover {
  background-color: rgba(0, 56, 112, 0.08);
  transition: background-color 0.2s ease;
}

/* Filas activas */
.table-success {
  background-color: rgba(4, 120, 87, 0.15) !important;
}

/* Badges */
.badge {
  font-size: 0.85rem;
  padding: 0.4em 0.6em;
  border-radius: 0.4rem;
}

.bg-success {
  background-color: #16a34a !important;
}

.bg-secondary {
  background-color: #9ca3af !important;
}

/* Footer */
.modal-footer {
  border-top: 1px solid rgba(0, 56, 112, 0.15);
}

.btn-primary {
  background-color: #003870;
  border: none;
  transition: all 0.3s ease;
}

.btn-primary:hover {
  background-color: #0058b0;
  transform: scale(1.03);
}

/* Animación sutil de entrada */
.modal-content {
  animation: fadeInModal 0.3s ease forwards;
}

@keyframes fadeInModal {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style>

