<template>
  <div id="app">
    <h1>Conversor a Dólares</h1>
    <div>Ingrese monto $ <input v-model="montoIngresado" /></div>
    <div>
      Valor del dolar en $ <input v-if="!checked" v-model="valorUSD" /><input v-else v-model="this.cotizacionUSD.valor"/>
      <input type="checkbox" v-model="checked" @change="getCotizacionUSD()"/>
        <div v-if="checked">
         Actualización - {{ cotizacionUSD.fyh }}
        </div>
        <div v-else>
          Actualización - 
        </div>
    </div>

    <p>Valor convertido en USD {{ convertir() }}</p>

    <br />

    <p>1/5 - Como es la forma reducida de v-on:click?: <b>C</b></p>
    <p>2/5 - vue create se utiliza para: <b>B</b></p>
    <p>3/5 - El modificador trim recorta: <b>C</b></p>
    <p>
      4/5 - La propiedad routes en la definición del router de Vue.js tiene como
      función: <b>A, C</b>
    </p>
    <p>5/5 - Las mutaciones contienen: <b>B, C</b></p>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  mounted() {
  },
  data() {
    return {
      valorUSD: 0,
      montoIngresado: 0,
      montoCalculado: 0,
      cotizacionUSD: {},
      checked: false,
    };
  },
  methods: {
    convertir() {
      return this.valorUSD == 0 || this.montoIngresado == 0
        ? 0
        : (this.montoIngresado / this.valorUSD).toFixed(2);
    },
    async getCotizacionUSD() {
      let { data: respuesta } = await this.axios(
        "https://www.dolarsi.com/api/api.php?type=valoresprincipales"
      );

      let cotizacion = Number(
        respuesta
          .find((dato) => dato.casa.nombre == "Dolar Blue")
          .casa.venta.replace(",", ".")
      );

      this.cotizacionUSD = {
        valor: cotizacion.toFixed(2),
        fyh: new Date().toLocaleString(),
      };
      console.log(this.cotizacionUSD);

      setTimeout(() => {
          this.getCotizacionUSD()
        },20000)
    },
  },
};
</script>

<style>
#app {
}
</style>
