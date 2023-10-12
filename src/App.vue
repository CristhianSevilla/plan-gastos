<script setup>
import { ref } from "vue";
import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";
import iconoNuevoGasto from "./assets/img/nuevo-gasto.svg";

const presupuesto = ref(0);
const disponible = ref(0);

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad;
  disponible.value = cantidad;
};
</script>

<template>
  <div>
    <header>
      <div class="contenedor-header">
        <h1>Planificador de Gastos</h1>

        <div class="sombra">
          <Presupuesto
            v-if="presupuesto === 0"
            @definir-presupuesto="definirPresupuesto"
          />
          <ControlPresupuesto
            v-else
            :presupuesto="presupuesto"
            :disponible="disponible"
          />
        </div>
      </div>
    </header>
    <main v-if="presupuesto > 0" class="imagen-presupuesto crear-gasto">
      <img :src="iconoNuevoGasto" alt="Icono Nuevo Gasto" />
    </main>
  </div>
</template>

<style>
:root {
  --azul: #3167be;
  --blanco: #fff;
  --gris-claro: #e6e6e6;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000;
}
html {
  font-size: 62.5%;
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-image: url("/src/assets/img/2.svg");
  background-size: cover;
  background-position: bottom right;
  background-repeat: no-repeat;
}
h1 {
  font-size: 4rem;
}
h2 {
  font-size: 3rem;
}
header h1 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  padding: 3rem 0;
  margin: 0;
  color: var(--blanco);
  text-align: center;
  background-color: var(--azul);
  margin-bottom: 5rem;
  text-transform: capitalize;
  font-size: 3rem;
}
.contenedor {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}
.contenedor-header {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  width: 100%;
  height: 100vh;
}
@media (min-width: 768px) {
}
.sombra {
  position: absolute;
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 5rem;
  width: 90%;
  max-width: 60rem;
  margin-top: -20rem;
}
.crear-gasto {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
  transition: transform 0.3s ease;
}
.crear-gasto img {
  width: 5rem;
}
.crear-gasto:hover {
  cursor: pointer;
  transform: scale(1.2);
}
</style>
