<template>
  <section class="bg-gradient-to-b from-[#845cdc] to-[#0894a4] flex flex-col items-center justify-center h-[100vh] p-[22px] gap-[20px]">
    <h1 class="text-white font-bold text-[30px]">Administrador de Tareas</h1>
    <div class="bg-white flex justify-between p-[15px] items-center w-[42vh] h-[10vh] rounded">
        <input class="text-[#845cdc] font-bold text-[20px] w-[25vh] py-[5px] text-center" placeholder="Agregar tarea" v-model="tarea">
        <i class="fa-solid fa-circle-plus text-[#845cdc] text-[40px] cursor-pointer" @click="agregar()"></i>
    </div>

    
    
    <div class="p-[20px] overflow-y-auto flex flex-col justify-start items-center gap-[20px]">
        <div v-for="(task,index) in tareas" :key="index" class="bg-gradient-to-r from-blue-900 to-green-800 w-[42vh] flex justify-between items-center p-[20px] rounded-[15px]" >
             <button @click="completar(index)" :id="`check${task.id}`" class="bg-white w-[30px] h-[30px]"><i v-if="task.completada" class="fa-solid fa-check text-green-600"></i></button>
            <h3 class="font-bold text-white text-[20px]">{{ task.nombre }}</h3>
            <button @click="borrar(index)"><i class="fa-solid fa-trash text-[white] text-[25px]"></i></button>
        </div>
    </div>

    
  </section>
</template>

<script>
export default {
    name:'MainComponent',
    data(){
        return{
            tarea:'',
            tareas:[]
        }
    },
    methods:{
        getLocalstorage(){
            this.tareas=JSON.parse(localStorage.getItem('tareas'))
            if (this.tareas==null) {
                this.tareas=[]
                localStorage.setItem('tareas',JSON.stringify(this.tareas))
            }
            console.log(this.tareas);
            
        },
        agregar(){ 
            let object={'id':this.tareas.length+1,'nombre':this.tarea,'completada':false}
            this.tareas.push(object)
            localStorage.setItem('tareas',JSON.stringify(this.tareas))
            console.log(this.tareas);
        },
        borrar(index){
            this.tareas.splice(index,1)//borrar una posicion especifica
            localStorage.setItem('tareas',JSON.stringify(this.tareas))
        },
        completar(index){
            if (this.tareas[index].completada) {
                this.tareas[index].completada=false

            }else{
                this.tareas[index].completada=true
            }
            localStorage.setItem('tareas',JSON.stringify(this.tareas))
        }

    },
    mounted(){
        this.getLocalstorage() 
    }
}
</script>

<style>

</style>