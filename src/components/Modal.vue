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
      <form>
        <legend>Agregar Gasto</legend>

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
            placeholder="Escribe la cantidad, ej. 300"
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
  background-color: rgb(0 0 0 / 0.8);
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
  width: 3rem;
  cursor: pointer;
}
</style>
