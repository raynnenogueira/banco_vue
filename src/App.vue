<template>
  <div>
    <h1>Banco</h1>
    <conta :saldo="saldo" @depositar="depositar" @sacar="sacar"></conta>
  </div>
</template>

<script>
import Conta from "./components/Conta.vue";

export default {
  components: {
    Conta,
  },
  data() {
    return {
      saldo: 0,
    };
  },
  methods: {
    depositar(valor) {
      this.saldo += valor;
    },
    sacar(valor) {
      this.saldo -= valor;
    },
  },
};
</script>

<style scoped></style>
