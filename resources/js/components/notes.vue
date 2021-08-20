<template>

    <div class="">
            <button type="button"  @click=" modificar=false; abrirModal();" class="btn btn-info btn-md" >
                 Nuevo
            </button>

            <!-- Modal -->
            <div  class="modal" :class="{mostrar:modal}"  role="dialog">
            <div class="modal-dialog">

                <!-- Modal content-->
                <div class="modal-content">
                <div class="modal-header">
                    <h4 class="modal-title">{{tituloModal}}</h4>
                     <button type="button" class="close" @click="cerrarModal();">&times;</button>
                </div>
                <div class="modal-body">
                    
                <div class="my-4">
              <label for="nombre">Nombre</label>
              <input v-model="note.name" type="text" class="form-control" id="name" placeholder="Nombre del note">
            </div>
            <div class="my-4">
              <label for="descripcion">descripcion</label>
              <input v-model="note.titulo" type="text" class="form-control" id="titulo" placeholder="Descripcion del note">
            </div>
            <div class="my-4">
              <label for="stock">stock</label>
              <input v-model="note.descripcion" type="text" class="form-control" id="description" placeholder="Stock del note">
            </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-danger" @click="cerrarModal();">Cancelar</button>
                     <button type="button" class="btn btn-success" @click="guardar();">Guardar</button>
                </div>
                </div>

            </div>
            </div>
            <h1 class="text-center">Gestionar notas</h1>
              <hr>
            <table class="table">
            <thead class="thead-dark">
                <tr>
                <th scope="col">#</th>
                <th scope="col">nombre </th>
                <th scope="col">titulo</th>
                <th  scope="col" colspan="2">Accion</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="nt in notes" :key="nt.id">
                    <th scope="row">{{nt.id}}</th>
                    <td>{{nt.name}}</td>
                    <td>{{nt.titulo}}</td>
                    <td>
                        <button   @click=" modificar=true;  abrirModal(nt);" type="" class="btn btn-primary">Editar</button>
                    </td>
                    <td class="">
                            <button type="" @click="Eliminar(nt.id)" class="btn btn-danger">Eliminar</button>
                    </td>

                </tr>
                    
                
            </tbody>
            </table>
    </div>

</template>

<script>
export default {
    data(){
        return{
        note:{
        name:'yy',
        titulo:'yuu',
        descripcion:'gfdgf?',
        },
            id:0,
            modificar:true,
            modal:0,
            tituloModal:'',
            notes:[],
        }                 
    },
    methods:{
        async listar(){
            const resp = await axios.get('note');
            this.notes = resp.data;
        },
        async Eliminar(id=''){
            const resp = await axios.delete('note/'+id);
            this.listar();
        },
        async guardar() {
      if(this.modificar){
        const res = await axios.put('note/'+this.id, this.note);
        
      }else{
        const res = await axios.post('note/', this.note);
        
      }
      this.cerrarModal();
      this.listar();
    },
        abrirModal(data={}){
      this.modal=1;
      if(this.modificar){
        this.id=data.id;
        this.tituloModal="Modificar note";
        this.note.name=data.name;
        this.note.titulo=data.titulo;
        this.note.descripcion=data.descripcion;
      }else{
        this.id=0;
        this.tituloModal="Crear note";
        this.note.name='';
        this.note.titulo='';
        this.note.descripcion='';
      }
     },
        cerrarModal(){
            this.modal=0;
        }

    },
    created(){
        this.listar();
    }

}

</script>  

<style>
.mostrar{
    display:list-item;
    opacity:1;
    background: rgba(255,255,255,0.7);
}
</style>