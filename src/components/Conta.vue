<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
        <p>Saldo: <span :style="{ color: saldo < 0 ? 'red' : 'green' }">{{ formatarSaldo(saldo) }}</span></p>
        <input v-model="valor" type="number">
        <button @click="depositar">Depositar</button>
        <button @click="tentarSacar">Sacar</button>
        <p v-if="erro" style="color: red;">{{ mensagemErro }}</p>
    </div>
</template>

<script>
export default {
    props: ['saldo'],
    data() {
        return {
            valor: 0,
            erro: false,
            mensagemErro: ''
        }
    },
    methods: {
        depositar() {
            if (this.valor >= 0) {
                this.$emit('depositar', parseFloat(this.valor));
                // $emit é uma maneira de permitir a comunicação entre componentes Vue, 
                // permitindo que eles disparem eventos personalizados e ouçam esses eventos 
                // em outros lugares em sua aplicação.
                this.valor = 0;
                this.erro = false;
            } else {
                this.erro = true;
                this.mensagemErro = 'Valor inválido';
            }
        },

        tentarSacar() {
            if (this.valor >= 0 && this.valor <= this.saldo) {
                this.$emit('sacar', parseFloat(this.valor));
                this.valor = 0;
                this.erro = false;
            } else {
                this.erro = true;
                if (this.saldo < 0) {
                    this.mensagemErro = `Saldo insuficiente. Saldo: -R$ ${Math.abs(this.saldo).toFixed(2)}`;
                } else {
                    this.mensagemErro = `Saldo insuficiente. Saldo: R$ ${this.saldo.toFixed(2)}`;
                }
            }
        },

        formatarSaldo(saldo) {
            if (saldo < 0) {
                return `-R$ ${Math.abs(saldo).toFixed(2)}`;
            } else {
                return `R$ ${saldo.toFixed(2)}`;
            }
        }
    }
}
</script>

<style scoped>
/* Seus estilos, se necessário */
</style>
