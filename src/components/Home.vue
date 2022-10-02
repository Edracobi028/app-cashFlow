<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="1000000"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts"/>
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
      movements: [{
        id: 0,
        title: "Movimiento 1",
        description: "Esto es un gasto",
        amount: 100,
        time: new Date("10-02-2022"),
      },{
        id: 1,
        title: "Movimiento 2",
        description: "Esto es un gasto",
        amount: 200,
        time: new Date("10-02-2022"),
      },{
        id: 2,
        title: "Movimiento 3",
        description: "Esto es un gasto",
        amount: 500,
        time: new Date("10-02-2022"),
      },{
        id: 3,
        title: "Movimiento 4",
        description: "Esto es un gasto",
        amount: 200,
        time: new Date("10-02-2022"),
      },{
        id: 4,
        title: "Movimiento 5",
        description: "Esto es un gasto",
        amount: -400,
        time: new Date("10-02-2022"),
      },{
        id: 5,
        title: "Movimiento 6",
        description: "Esto es un gasto",
        amount: -600,
        time: new Date("10-02-2022"),
      },
      {
        id: 6,
        title: "Movimiento 7",
        description: "Esto es un gasto",
        amount: -300,
        time: new Date("10-02-2022"),
      }, 
      {
        id: 7,
        title: "Movimiento 8",
        description: "Esto es un gasto",
        amount: 100,
        time: new Date("10-02-2022"),
      }, 
      {
        id: 8,
        title: "Movimiento 9",
        description: "Esto es un gasto",
        amount: 300,
        time: new Date("09-02-2022"),
      }, 
      {
        id: 9,
        title: "Movimiento 10",
        description: "Esto es un gasto",
        amount: 500,
        time: new Date("09-02-2022"),
      }],
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
        const lastMovements = lastDays.slice(0, i); //Por cada movimiento obtener toda la lista de movimientos anteriores a este
        //retornar toda la sumatoria de los ultimos movimientos

        return lastMovements.reduce((suma, movement) => {
          return suma + movement
        }, 0); 
      });
    }
  },

  methods: {
    create(movement){
      this.movements.push(movement);//agregamos el movimiento
    },
    remove(id){
      const index = this.movements.findIndex(m => m.id === id); //comparar si el id es el del movimiento que estamos haciendo
      this.movements.splice(index, 1);
    }
  },
};
</script>
