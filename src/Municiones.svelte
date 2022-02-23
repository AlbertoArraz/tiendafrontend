<script>
  import { onMount, getContext } from "svelte";
  import { jsonData }            from "./store.js";

  import Buscar                  from "./Buscar.svelte";
  import Municion                 from "./Municion.svelte";
  import Boton                   from "./Boton.svelte";

  const URL = getContext("URL");

  let busqueda = "";
  let municion = {};

  onMount(async () => {
    const response = await fetch(URL.municiones);
    const data = await response.json();
    $jsonData = data;
  });

  $: regex = new RegExp(busqueda, "i");
  $: datos = busqueda 
    ? $jsonData.filter(item => regex.test(item.nombre))
    : $jsonData;

</script>

<style>
  .container {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: left;
    flex-wrap: wrap;
  }
</style>

<h1>MUNICIONES</h1>
<Buscar bind:busqueda />

<div class="container">
  <Municion bind:municion>
    <div style="text-align: right">
      <Boton documento={municion} tipo="insertar" coleccion="municiones" />
    </div>
  </Municion>
</div>

<div class="container">
  {#each datos as municion}
    <Municion {municion}>
      <div style="text-align: right">
        <Boton documento={municion} tipo="modificar" coleccion="municiones" />
        <Boton documento={municion} tipo="eliminar" coleccion="municiones" />
      </div>
    </Municion>
  {/each}
</div>