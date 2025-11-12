<template>
    <div class="admin-view container-fluid py-2 animate__animated animate__fadeIn">
        <!-- 🌟 HEADER TRANSPARENTE -->
        <header
            class="admin-header d-flex align-items-center justify-content-between px-4 py-3 shadow-sm animate__animated animate__fadeInDown">
            <div class="d-flex align-items-center gap-3">
                <div class="logo-bg">
                    <img src="/src/assets/LogoContraloriaMobile.png" alt="Logo Contraloría" class="logo-header" />
                </div>
                <h1 class="mb-0 text-white fw-bold fs-4">Panel de Administrador</h1>
            </div>

            <!-- 🧑‍💼 Menú de usuario -->
            <div class="user-menu position-relative">
                <button class="user-chip fw-semibold d-flex align-items-center gap-2" @click="toggleMenu">
                    <i class="bi bi-person-circle fs-5"></i>
                    <span>Administrador</span>
                </button>

                <transition name="fade-slide">
                    <div v-if="menuVisible" class="dropdown-menu-custom animate__animated animate__fadeInDown">
                        <button class="dropdown-item-custom text-danger" @click="cerrarSesion">
                            <i class="bi bi-box-arrow-right me-2"></i> Cerrar sesión
                        </button>
                    </div>
                </transition>
            </div>
        </header>


        <!-- 🌿 CONTENIDO PRINCIPAL -->
        <main class="container mt-5">
            <div class="row g-4">
                <!-- 🧾 Gestión de Participantes -->
                <div class="col-12 col-md-6 col-lg-4">
                    <ParticipantesCard :totalParticipantes="totalParticipantes" :totalActivos="totalActivos"
                        @ver-participantes="verParticipantes" @eliminar-base="eliminarBase"
                        @archivo-cargado="onArchivoCargado" />
                </div>

                <!-- 📋 Modal de lista de participantes -->
                <ModalParticipantes :visible="mostrarModal" :participantes="listaDemo" @cerrar="mostrarModal = false" />


                <!-- 🧍 Registro de Asistencia -->
                <div class="col-12 col-md-6 col-lg-8">
                    <div class="admin-card animate__animated animate__fadeInUp">
                        <h5 class="card-title">
                            <i class="bi bi-person-check-fill me-2 text-info"></i> Registro de Asistencia
                        </h5>

                        <div class="d-flex flex-column flex-md-row align-items-center gap-3 mt-3">
                            <input type="text" class="form-control flex-grow-1"
                                placeholder="Ingrese cédula del participante" />
                            <button class="btn btn-success px-4 fw-bold">Registrar</button>
                        </div>
                    </div>
                </div>

                <!-- 🚫 Gestión de Excluidos -->
                <div class="col-12 col-md-6 col-lg-6">
                    <div class="admin-card animate__animated animate__fadeInUp">
                        <h5 class="card-title">
                            <i class="bi bi-person-dash-fill me-2 text-info"></i> Gestión de Excluidos
                        </h5>

                        <div class="d-flex flex-column flex-md-row align-items-center gap-3 mt-3">
                            <input type="text" class="form-control flex-grow-1" placeholder="Cédula a excluir" />
                            <button class="btn btn-danger px-4 fw-bold">Añadir</button>
                        </div>

                        <ul class="list-group list-group-flush mt-3 small text-white">
                            <li class="list-group-item bg-transparent border-0 text-white-50">No hay excluidos.</li>
                        </ul>
                    </div>
                </div>

                <!-- 🎁 Gestión de Premios -->
                <div class="col-12 col-md-6 col-lg-6">
                    <div class="admin-card animate__animated animate__fadeInUp">
                        <h5 class="card-title">
                            <i class="bi bi-gift-fill me-2 text-info"></i> Gestión de Premios
                        </h5>

                        <div class="d-flex flex-column flex-md-row align-items-center gap-3 mt-3">
                            <input type="number" class="form-control flex-grow-1" placeholder="Monto del premio" />
                            <button class="btn btn-primary px-4 fw-bold">Añadir</button>
                        </div>

                        <ul class="list-group list-group-flush mt-3 small text-white">
                            <li class="list-group-item bg-transparent border-0 text-white-50">No hay premios
                                registrados.</li>
                        </ul>
                    </div>
                </div>
            </div>

            <!-- 🌟 Botón IR AL SORTEO -->
            <div class="text-center mt-5">
                <button class="btn btn-lg btn-success fw-bold px-5 py-3 pulse-wide" @click="$router.push('/sorteo')">
                    Ir al Sorteo
                </button>
            </div>
        </main>
    </div>
</template>

<script>
import ParticipantesCard from '@/components/ParticipantesCard.vue';
import ModalParticipantes from '@/components/ModalParticipantes.vue';

export default {
    name: "AdminView",
    components: { ParticipantesCard, ModalParticipantes },
    data() {
        return {
            menuVisible: false,
            mostrarModal: false,
            totalParticipantes: 0,
            totalActivos: 0,
        };
    },
    methods: {
        toggleMenu() {
            this.menuVisible = !this.menuVisible;
        },
        cerrarSesion() {
            this.menuVisible = false;
            this.$router.push("/login");
        },
        verParticipantes() {
            this.mostrarModal = true;
        },
        eliminarBase() {
            this.totalParticipantes = 0;
            this.totalActivos = 0;
        },
        onArchivoCargado(lista) {
            this.listaDemo = lista.map((item) => ({
                nombre: item.Nombre || item.nombre || "Sin nombre",
                cedula: item.Cédula || item.cedula || "N/A",
                departamento: item.Departamento || item.departamento || "N/A",
                cargo: item.Cargo || item.cargo || "N/A",
                activo: item.Activo?.toString().toLowerCase() === "true",
            }));

            this.totalParticipantes = this.listaDemo.length;
            this.totalActivos = this.listaDemo.filter((p) => p.activo).length;
        }
    },
};
</script>

<style scoped>
.admin-view {
    position: relative;
    min-height: 100vh;
    color: #fff;
    z-index: 1;
}

/* 🪟 Header */
.admin-header {
    position: sticky;
    top: 0;
    background: rgba(255, 255, 255, 0.08);
    backdrop-filter: blur(8px);
    border-radius: 0.75rem;
    z-index: 100;
}

.logo-bg {
    background: rgba(255, 255, 255, 0.85);
    border-radius: 1rem;
    padding: 0.4rem 0.8rem;
}

.logo-header {
    width: 80px;
    height: auto;
}

/* 🧑‍💼 Botón rectangular del usuario */
.user-chip {
    background: linear-gradient(135deg, #0058b0, #003b84);
    color: #fff;
    border: none;
    padding: 0.55rem 1.3rem;
    border-radius: 1.5rem;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 0 10px rgba(0, 91, 196, 0.4);
}

.user-chip:hover {
    background: linear-gradient(135deg, #0066cc, #004a99);
    box-shadow: 0 0 18px rgba(0, 123, 255, 0.7);
    transform: translateY(-1px);
}


/* 🌟 Dropdown elegante */
.dropdown-menu-custom {
    position: absolute;
    right: 0;
    top: 115%;
    background: rgba(0, 60, 130, 0.35);
    backdrop-filter: blur(12px);
    border-radius: 0.75rem;
    padding: 0.4rem;
    min-width: 160px;
    z-index: 200;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
    animation: fadePop 0.25s ease forwards;
}

/* 🎨 Botón dentro del menú */
.dropdown-item-custom {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
    width: 100%;
    color: #ffffff;
    background: transparent;
    border: none;
    padding: 0.6rem 0.8rem;
    border-radius: 0.5rem;
    text-align: center;
    font-weight: 500;
    transition: all 0.25s ease;
}

.dropdown-item-custom:hover {
    background: rgba(255, 255, 255, 0.15);
    color: #ff4b4b;
}

/* ✨ Nueva animación tipo “fade + pop” */
@keyframes fadePop {
    0% {
        opacity: 0;
        transform: scale(0.9);
    }

    100% {
        opacity: 1;
        transform: scale(1);
    }
}


/* Tarjetas */
.admin-card {
    background: rgba(255, 255, 255, 0.08);
    backdrop-filter: blur(8px);
    border-radius: 1rem;
    padding: 1.5rem;
    box-shadow: 0 3px 12px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
}

.admin-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 15px rgba(255, 255, 255, 0.1);
}

/* Títulos */
.card-title {
    font-weight: 700;
    color: #5cc4ff;
}

/* Inputs */
.form-control {
    background-color: rgba(255, 255, 255, 0.9);
    border: none;
    border-radius: 0.5rem;
}

/* ❤️ Botón animado IR AL SORTEO */
.pulse-wide {
    animation: pulse-wide 1.8s infinite;
    width: 320px;
    font-size: 1.3rem;
}

@keyframes pulse-wide {
    0% {
        transform: scale(1);
        box-shadow: 0 0 0 0 rgba(25, 135, 84, 0.7);
    }

    70% {
        transform: scale(1.07);
        box-shadow: 0 0 0 25px rgba(25, 135, 84, 0);
    }

    100% {
        transform: scale(1);
    }
}

/* Transición menú */
.fade-slide-enter-active,
.fade-slide-leave-active {
    transition: all 0.3s ease;
}

.fade-slide-enter-from {
    opacity: 0;
    transform: translateY(-8px);
}

.fade-slide-leave-to {
    opacity: 0;
    transform: translateY(-8px);
}
</style>
