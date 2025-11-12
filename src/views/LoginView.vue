<template>
    <div class="login-page d-flex align-items-center justify-content-center min-vh-100">
        <div class="card login-card shadow-lg text-center animate__animated animate__fadeInDown">
            <div class="card-body px-5 py-4">
                <!-- Logo -->
                <img src="/src/assets/LogoContraloriaMobile.png" alt="Logo Contraloría" class="logo mb-2" />

                <h4 class="fw-bold mb-4 animate__animated animate__fadeInDown">
                    <span class="titulo-login">Sorteo Navideño</span>
                </h4>

                <!-- Formulario -->
                <form @submit.prevent="login">
                    <div class="mb-3 text-start">
                        <label class="form-label">Usuario</label>
                        <input v-model="usuario" type="text" class="form-control" placeholder="Ingrese su usuario"
                            required />
                    </div>
                    <div class="mb-4 text-start">
                        <label class="form-label">Contraseña</label>
                        <input v-model="clave" type="password" class="form-control" placeholder="Ingrese su contraseña"
                            required />
                    </div>

                    <button type="submit" class="btn btn-primary w-100 py-2 fw-bold">
                        Iniciar sesión
                    </button>
                </form>

                <p class="mt-4 text-muted small">
                    © 2025 Contraloría General de la República Dominicana
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "LoginView",
    data() {
        return {
            usuario: "",
            clave: "",
        };
    },
    methods: {
        login() {
            if (this.usuario === "admin" && this.clave === "1234") {
                this.$router.push("/admin");
            } else {
                alert("Credenciales incorrectas. Intente de nuevo.");
            }
        },
    },
};
</script>

<style scoped>
.login-page {
    width: 100%;
    background: transparent;
    /* usa el fondo del App.vue */
}

/* Tarjeta */
.login-card {
    max-width: 420px;
    border: none;
    border-radius: 1rem;
    background: rgba(255, 255, 255, 0.92);
    color: #222;
    backdrop-filter: blur(6px);
}

/* titulo */
.titulo-login {
    color: #004c9a;
    /* azul intermedio elegante */
    font-weight: 700;
    letter-spacing: 0.5px;
    text-shadow: 0 1px 4px rgba(255, 255, 255, 0.5);
}

/* Logo */
.logo {
    width: 180px;
    height: auto;
}

/* Botón */
.btn-primary {
    background-color: #003870 !important;
    border: none;
    transition: all 0.3s ease;
}

.btn-primary:hover {
    background-color: #0055b3 !important;
    transform: scale(1.03);
}
</style>
