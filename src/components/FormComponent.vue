<template>
    <h1>Formulario de login</h1>
    <div id="barra"></div>
    <section>

        <form @submit.prevent="saveUser">

            <div>
                <label for="username">Username:</label>
                <input type="text" v-model="username" required />
            </div>
            <div>
                <label for="email">Email:</label>
                <input type="email" v-model="email" required />
            </div>
            <div>
                <label for="password">Password:</label>
                <input type="password" v-model="password" required />
            </div>
            <div>
                <label for="fecha_nacimiento">Fecha de Nacimiento:</label>
                <input type="date" v-model="fecha_nacimiento" required />
            </div>
            <input type="submit" value="Guardar" />
        </form>
        <aside v-if="usuarioCreado">

            <h2>Datos del usuario</h2>
            <p>Username: {{ usuario.username }}</p>
            <p>Email: {{ usuario.email }}</p>
            <p>Fecha de Nacimiento: {{ usuario.fecha_nacimiento }}</p>
            <p>Edad: {{ usuario.edad }}</p>

        </aside>
    </section>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Usuario {
    username: string;
    email: string;
    password: string;
    fecha_nacimiento: string;
    edad: Number
}

const validarDatos = () => {
    if (!username.value || !email.value || !password.value || !fecha_nacimiento.value) {
        return false;
    }
    return true;
}
const calcularEdad = (fecha_nacimiento: string): number => {
    // Calcular la edad a partir de la fecha de nacimiento
    // La fecha de nacimiento debe estar en formato YYYY-MM-DD
    // Ejemplo: 1990-01-01
    // La fecha de hoy debe estar en formato YYYY-MM-DD
    // Ejemplo: 2023-10-01

    const fechaNacimiento = new Date(fecha_nacimiento);
    const hoy = new Date();
    let edad = hoy.getFullYear() - fechaNacimiento.getFullYear();
    const mes = hoy.getMonth() - fechaNacimiento.getMonth();
    // Si el mes actual es menor que el mes de nacimiento, restar un año
    // Si el mes actual es igual al mes de nacimiento, pero el día actual es menor que el día de nacimiento, restar un año
    // Si el mes actual es mayor que el mes de nacimiento, no hacer nada
    if (mes < 0 || (mes === 0 && hoy.getDate() < fechaNacimiento.getDate())) {
        edad--;
    }
    // Devolver la edad

    return edad;
}
// Limpiar los datos del formulario
const limpiarDatos = () => {
    username.value = "";
    email.value = "";
    password.value = "";
    fecha_nacimiento.value = "";
}



function mostrarAlerta() {
    alert(`Usuario guardado: 
        username ${usuario.username} 
        email:${usuario.email} 
        fecha nacimiento ${usuario.fecha_nacimiento}
        edad ${usuario.edad} años`);
    usuarioCreado.value = true;
    limpiarDatos();
}
// Guardar el usuario
const saveUser = () => {

    usuarioCreado.value = false;
    // Validar datos
    // Si los datos son válidos, guardar el usuario
    // Si no son válidos, mostrar un mensaje de error
    if (validarDatos()) {
        usuario.edad = calcularEdad(fecha_nacimiento.value);
        usuario.username = username.value;
        usuario.email = email.value;
        usuario.password = password.value;
        usuario.fecha_nacimiento = fecha_nacimiento.value;
        // Mostrar el usuario
        mostrarAlerta();
        // Limpiar los datos del formulario
        limpiarDatos();
    } else {
        alert('Datos inválidos');
    }
}
// Crear el usuario
// El usuario se crea al guardar los datos
const usuario: Usuario = {
    username: '',
    email: '',
    password: '',
    fecha_nacimiento: '',
    edad: 0
}

const username = ref("");
const email = ref("");
const password = ref("");
const fecha_nacimiento = ref("");
const usuarioCreado = ref(false);




</script>

<style scoped>
section {
    display: flex;
}

#barra {
    width: 0%;
    height: 5px;
    background-color: #15cc49;
    margin-bottom: 10px;
}

form {
    display: flex;
    flex-direction: column;
    width: 500px;
    justify-content: center;
    align-items: center;
}

div {
    margin-bottom: 10px;
    display: flex;
}

label {
    width: 250px;
    margin-bottom: 5px;
    text-align: start;
}

input {
    padding: 0.25rem 1rem;
    height: 30px;
    width: 250px;
    margin-bottom: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 7px;
    border: none;
}

aside,
form {
    margin-left: 50px;
    padding: 1rem 3em;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);

    border-radius: 7px;

}

input[type="submit"] {
    padding: 0.25rem 1rem;
    height: 40px;
    width: 100%;
    text-transform: uppercase;
    background-color: #15cc49;
    color: white;
    transition: all 0.5s ease;

    &:hover {
        background-color: #0b8f30;

    }
}
</style>