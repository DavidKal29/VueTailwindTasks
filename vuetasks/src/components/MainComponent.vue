<template>
  <section class="bg-gradient-to-b from-[#845cdc] to-[#0894a4] flex flex-col items-center justify-center h-[100vh] p-[22px] gap-[20px]">
    <h1 class="text-white font-bold text-[25px] max-[320px]:text-[20px]">Administrador de Tareas</h1>
    <div class="bg-white flex justify-between p-[15px] items-center w-[42vh] h-[10vh] rounded">
        <input class="text-[#845cdc] font-bold text-[20px] max-[320px]:text-[15px] w-[25vh] py-[5px] text-center" placeholder="Agregar tarea" v-model="tarea">
        <i class="fa-solid fa-circle-plus text-[#845cdc] text-[40px] max-[320px]:text-[25px] cursor-pointer" @click="agregar()"></i>
    </div>

    <div class="p-[20px] overflow-y-auto flex flex-col justify-start items-center gap-[20px]">
        <div v-for="(task,index) in filtro" :key="index" class="bg-gradient-to-r from-blue-900 to-green-800 w-[42vh] flex justify-between items-center p-[20px] rounded-[15px]" >
             <button v-if="filtro==tareas" @click="completar(index)" :id="`check${task.id}`" class="bg-white w-[30px] h-[30px]"><i v-if="task.completada" class="fa-solid fa-check text-green-600"></i></button>
             <i v-if="task.completada && filtro!=tareas" class="fa-solid fa-check text-green-600"></i>
             <i v-if="!task.completada && filtro!=tareas" class="fa-solid fa-x text-red-600"></i>
            <h3 class="font-bold text-white text-[20px]">{{ task.nombre }}</h3>
            <button v-if="filtro==tareas" @click="borrar(index)"><i class="fa-solid fa-trash text-[white] text-[25px]"></i></button>
            <button v-if="filtro!=tareas"><i class="fa-solid fa-trash text-gray-400 text-[25px]"></i></button>
        </div>
        
    </div>

    <div class="flex justify-center items-center flex-col gap-[15px]">
        <button @click="todos()" class="bg-gradient-to-r to-green-400 from-violet-900 w-[42vh] py-[8px] px-[60px] max-[320px]:py-[0.2px] rounded text-white font-bold">Ver todos</button>
        <button @click="completados()" class="bg-gradient-to-r to-green-400 from-violet-900 w-[42vh] py-[8px] px-[60px] max-[320px]:py-[0.2px] rounded text-white font-bold">Ver completados</button>
        <button @click="pendientes()" class="bg-gradient-to-r to-green-400 from-violet-900 w-[42vh] py-[8px] px-[67px] max-[320px]:py-[0.2px] rounded text-white font-bold">Ver pendientes</button>
    </div>
  </section>
</template>

<script>
export default {
    name:'MainComponent',
    data(){
        return{
            tarea:'',
            tareas:[],
            filtro:this.tareas
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
            if (this.tarea=='') {
                alert('Ponle nombre a la tarea')
            }else{
                let object={'id':this.tareas.length+1,'nombre':this.tarea,'completada':false}
                this.tareas.push(object)
                localStorage.setItem('tareas',JSON.stringify(this.tareas))
                console.log(this.tareas);
            }
            
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
        },
        todos(){
            this.filtro=this.tareas
        },
        completados(){
            this.filtro=this.tareas.filter(t=>t.completada)
        },
        pendientes(){
            this.filtro=this.tareas.filter(t=>!t.completada)
        }

    },
    mounted(){
        this.getLocalstorage() 
        this.filtro=this.tareas
    }
}
</script>

<style>

</style>