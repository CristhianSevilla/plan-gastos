<script setup>
import cerrarModal from "../assets/img/cerrar.svg";

const emit = defineEmits(["ocultar-modal"]);
</script>
<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img
        :src="cerrarModal"
        alt="cerrar modal"
        @click="$emit('ocultar-modal')"
      />
    </div>

    <div class="contenedor">
      <form class="nuevo-gasto">
        <legend>Nuevo Gasto</legend>

        <div class="campo">
          <label for="gasto">Gasto:</label>
          <input
            type="text"
            id="gasto"
            placeholder="Escribe el Nombre del Gasto"
          />
        </div>

        <div class="campo">
          <label for="cantidad">Cantidad Gastada:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Escribe la cantidad, Ej. 300"
          />
        </div>

        <div class="campo">
          <label for="categoria">Categoria:</label>

          <select id="categoria">
            <option value="">Selecciona una categoría</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>

        <input type="submit" value="Agregar Gasto" />
      </form>
    </div>
  </div>
</template>

<style lang="css" scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
}
.cerrar-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}
.cerrar-modal img {
  width: 2.5rem;
  cursor: pointer;
}
.nuevo-gasto {
  margin: 8rem auto 0 auto;
  display: grid;
  gap: 2rem;
  padding: 2.5rem;
}
@media (min-width: 768px) {
  .nuevo-gasto {
    padding: 5rem;
  }
}
.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 3rem;
  font-weight: 700;
  padding-bottom: 1rem;
  border-bottom: 2px solid var(--azul);
  text-transform: uppercase;
}
.campo {
  display: grid;
  gap: 2rem;
}
.nuevo-gasto label {
  color: var(--blanco);
  font-size: 2.5rem;
}
.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2rem;
}
.nuevo-gasto input[type="submit"] {
  background-color: var(--azul);
  color: var(--blanco);
  font-weight: 700;
  margin-top: 4rem;
  transition: background-color 300ms ease;
}
.nuevo-gasto input[type="submit"]:hover {
  background-color: #1848a4;
  cursor: pointer;
}
</style>
