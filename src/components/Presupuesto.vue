<script setup>
import { ref } from "vue";
import Alerta from "./Alerta.vue";

const presupuesto = ref(0);
const error = ref("");

const emit = defineEmits(["definir-presupuesto"]);

const definirPresupuesto = () => {
  if (presupuesto.value <= 0 || presupuesto.value === "") {
    error.value = "presupuesto no valido";

    setTimeout(() => {
      error.value = "";
    }, 3000);

    return;
  }

  emit("definir-presupuesto", presupuesto.value);
};
</script>

<template>
  <form class="presupuesto" @submit.prevent="definirPresupuesto">
    <Alerta v-if="error">{{ error }}</Alerta>

    <p>Organiza tu dinero</p>

    <div class="campo">
      <input
        id="nuevo-presupuesto"
        class="nuevo-presupuesto"
        placeholder="Ingresa tu presupuesto"
        type="number"
        v-model.number="presupuesto"
      />
    </div>

    <input type="submit" value="Enviar" />
  </form>
</template>

<style lang="css" scoped>
.presupuesto {
  width: 100%;
}
.campo {
  display: grid;
  gap: 2rem;
}
.presupuesto p {
  font-size: 2rem;
  color: var(--anaranjado);
  text-align: center;
  margin-top: 0;
  text-transform: uppercase;
  font-weight: 900;
}
.presupuesto input[type="number"] {
  background-color: var(--gris-claro);
  padding: 1rem;
  border-radius: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
  width: 100%;
}
.presupuesto input[type="submit"] {
  background-color: var(--anaranjado);
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 2rem;
  margin-top: 2rem;
  border-radius: 1rem;
  color: var(--blanco);
  font-weight: 900;
  width: 100%;
  transition: background-color 300ms ease;
  text-transform: uppercase;
}

.presupuesto input[type="submit"]:hover {
  background-color: var(--anaranjado-oscuro);
  cursor: pointer;
}
</style>
