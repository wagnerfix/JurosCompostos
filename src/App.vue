<template>
  <div id="app">
    <v-container>
      <v-row>
        <v-col cols="4" md="4">
          <v-text-field
            type="number"
            v-model="inicial"
            label="Montante Inicial"
            @input="calcular"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="4" md="4">
          <v-text-field
            type="number"
            v-model="juros"
            label="Taxa de Juros Mensal"
            @input="calcular"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="4" md="4">
          <v-text-field
            type="number"
            v-model="periodo"
            label="Período (meses)"
            @input="calcular"
            required
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <div v-for="comp in composto" :key="comp.id" class="country border">
          <div class="parcela">{{comp.id}}</div>
          <span :class="[corFonte]">R$ {{formatNumber(comp.vltotal)}}</span>
          <span :class="[corFonte]">R$ {{formatNumber(comp.montante)}}</span>
          <span :class="[corFonteP]">{{formatNumber(comp.porcentagem)}}%</span>
        </div>
      </v-row>
    </v-container>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        inicial: 1000,
        juros: 0.1,
        periodo: 0,
        composto: [],
        corFonte: '',
        corFonteP: ''
      }
    },
    methods: {
      calcular() {
        let vi = this.inicial
        let taxa = this.juros
        let mes = this.periodo
        this.composto = []
        for (let i = 1; i <= mes; i++) {
          let total = vi * (1 + taxa / 100) ** i
          let aum = total - vi
          let porc = ((total - vi) / vi) * 100
          this.composto.push({
            id: i,
            vltotal: total,
            montante: aum,
            porcentagem: porc
          })
          console.log(this.composto.id)
        }
        this.corFonte = taxa < 0 ? 'negativo' : 'positivo'
        this.corFonteP = taxa < 0 ? 'negativo' : 'corPorc'
      },
      formatNumber(value) {
        let val = (value / 1).toFixed(2).replace('.', ',')
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Arial', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;

    display: flex;
    justify-content: space-between;
    flex-direction: column;
  }
  .flexRow {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    padding: 10px;
  }
  .country {
    min-width: 250px;
    max-width: 220px;

    min-height: 120px;
    margin: 10px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .border {
    border: 1px solid lightgray;
    border-radius: 5px;
    margin-top: 5px;
    padding: 10px;
  }
  .parcela {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    font-weight: 500;
    color: dimgrey;
    margin-bottom: 5px;
    font-weight: 900;
  }
  .positivo {
    color: yellowgreen;
  }
  .corPorc {
    color: deepskyblue;
  }
  .negativo {
    color: crimson;
  }
</style>