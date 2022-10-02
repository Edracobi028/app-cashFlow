<template>
    <div>
        <svg 
            @touchstart="tap" 
            @touchmove="tap"
            @touchend="untap"
            viewBox="0 0 300 200">

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
            v-show="showPointer"
            stroke="#04b500"
            stroke-width="2"
            :x1="pointer"
            y1="0"
            :x2="pointer"
            y2="200"
            />
        </svg>   
        <p>Últimos 30 días</p>
        <div>{{ zero}}</div>
    </div>
</template>

<script setup>
     //Importamos props, toRefs, computed para recibir los montos y transformar en pixeles
     import { ref, defineProps, toRefs, computed } from 'vue';

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
            return `${points} ${x}, ${y}`;
        }, "0, 100");
     });

    const showPointer = ref(false); //Crea rvariable dinamica e iniciarla en falso

    const pointer = ref(0); //Crear Variable que tenga esa coordenada e inicializarla en cero

     //Funcion que recibe un evento
     const tap = ({ target, touches }) => {
        showPointer.value = true; //Aparecer el cursor
        const elementWidth = target.getBoundingClientRect().width;  //obtener el tamaño del svg segun el componente donde esta 
        const elementX = target.getBoundingClientRect().x; //Obtener cual es la coordenada x donde inicia
        const touchX = touches[0].clientX; //la coordenada donde hice touch
        pointer.value = ((touchX - elementX) * 300) / elementWidth ;//Transformarlo auna escala
    }

     const untap = () => {
        showPointer.value = false; //Desaparecer el cursor
     }

</script>

<style>
    svg {
        width: 100%;
    }
    p {
        text-align: center;
    }
</style>