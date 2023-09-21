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
            if (this.valor >= 0) {  // Se o valor inserido for maior ou igual a zero
                this.$emit('depositar', parseFloat(this.valor));
                // $emit é uma maneira de permitir a comunicação entre componentes Vue, 
                // permitindo que eles disparem eventos
                this.valor = 0; // Definimos o valor como 0
                this.erro = false; //  a variável é `false` para garantir que qualquer mensagem de erro anterior seja removida
            } else {
                this.erro = true;
                this.mensagemErro = 'Valor inválido';
            }
        },

        tentarSacar() {
            if (this.valor >= 0 && this.valor <= this.saldo) {  // o valor inserido for maior ou igual a zero e menor ou igual ao saldo disponível
                this.$emit('sacar', parseFloat(this.valor));  // Isso permite que o componente pai saiba que uma ação de saque ocorreu e obtenha o valor do saque.
                this.valor = 0;
                this.erro = false;
            } else {
                this.erro = true;
                if (this.saldo < 0) {
                    this.mensagemErro = `Saldo insuficiente. Saldo: -R$ ${Math.abs(this.saldo).toFixed(2)}`; // se for menor que 0, aqui vai dizer que o saldo é insuficiente e mostrar quanto de saldo tem na conta
                } else {
                    this.mensagemErro = `Saldo insuficiente. Saldo: R$ ${this.saldo.toFixed(2)}`; // se o valor for positivo, vai mostrar o saldo tambem
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

</style>
