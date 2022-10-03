<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @select="select" />
        </template>
        <template #action>
          <Action @create="create" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements
        :movements="movements" 
        @remove="remove"
      />
    </template>
  </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements/Index.vue";
import Action from "./Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic
},
  data(){
    return {
      amount: null,
      label: null,
      movements: [],
    }
  },
  computed: {
    amounts(){
      const lastDays =  this.movements
      //Obtener ultimos 30 dias
      .filter(m => {
        const today = new Date();//Dia de hoy
        const oldDate = today.setDate(today.getDate() - 30); //establecer una fecha apartir de hoy
        return m.time > oldDate; //Comparamos si es verdadero o falso
      })
      //obtiene el elemento m que es el movements
      .map(m => m.amount); //lista de todos los montos en especifico
      
      //tomar cada una de las fechas y sumarle los movimientos anteriores
      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1); //Por cada movimiento obtener toda la lista de movimientos anteriores a este
        //retornar toda la sumatoria de los ultimos movimientos

        return lastMovements.reduce((suma, movement) => {
          return suma + movement
        }, 0); 
      });
    },
    
    totalAmount(){
      return this.movements.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    }
  },

  mounted(){
    const movements = JSON.parse(localStorage.getItem("movements"));//Recuperar el valor y cargar movements
    
    //validacion y condicion para transformar los valores de tiem a formato fecha
    if(Array.isArray(movements)) {
      this.movements = movements.map(m => {
        return { ...m, time: new Date(m.time) };
      }); 
    }
    
  },

  methods: {
    create(movement){
      this.movements.push(movement);//agregamos el movimiento
      this.save();
    },
    remove(id){
      const index = this.movements.findIndex(m => m.id === id); //comparar si el id es el del movimiento que estamos haciendo
      this.movements.splice(index, 1);
      this.save();
    },
    save(){
      localStorage.setItem("movements", JSON.stringify(this.movements));  
    },
    select(el){
      console.log(el)
      this.amount = el;
    }
  },
};
</script>
