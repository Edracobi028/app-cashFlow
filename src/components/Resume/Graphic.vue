<template>
    <div>
        <svg viewBox="0 0 300 200">
            <line
            stroke="#c4c4c4"
            stroke-width="2" 
            x1="0"
            :y1="zero"
            x2="300"
            :y2="zero"
            />
            <polyline 
            fill="none"
            stroke="#0689b0"
            stroke-width="2"
            :points="points"
            />
            <line
            stroke="#04b500"
            stroke-width="2"
            x1="200"
            y1="0"
            x2="200"
            y2="200"
            />
        </svg>   
        <p>Últimos 30 días</p>
        <div>{{ zero}}</div>
    </div>
</template>

<script setup>
     //Importamos props, toRefs, computed para recibir los montos y transformar en pixeles
     import { defineProps, toRefs, computed } from 'vue';

     //Definimos props
     const props = defineProps({
        //lista de montos reactiva
        amounts: {
            type: Array,
            default: () => [],
        }
     });

     //Definimos toRefs de los props y extraer la variable ammount
     const { amounts } =  toRefs(props);

     const amountToPixels = (amount) => {
        const min = Math.min(...amounts.value); //valor minimo del arreglo
        const max = Math.max(...amounts.value); //valor maximo del arreglo

        const amountAbs = amount + Math.abs(min);     //del minimo a nuestro valor
        const minmax = Math.abs(max) + Math.abs(min); //distancia entre el minimo y el maximo

        return 200 - ((amountAbs * 100) / minmax ) * 2;  //Regla de 3
     }

     const zero = computed (() => {
        return amountToPixels(0);
     });

     //Definimos computed
     const points = computed(() =>{
        
        const total = amounts.value.length; //total de elementos

        //lista de ese numero de elementos
        return amounts.value.reduce((points, amount, i) => {
            const x = (300 / total) * (i + 1);
            const y = amountToPixels(amount);
            console.log(y);
            return `${points} ${x}, ${y}`;
        }, "0, 100");
     });

</script>

<style>
    svg {
        width: 100%;
    }
    p {
        text-align: center;
    }
</style>