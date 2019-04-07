<template>
    <div id="list-elements">
        <div id="list-elements-to-do">
            <h2>Tareas pendientes:</h2>
            <div class="list-elements-container">
                <list-element class="list-element" v-for="(e,i) in elements.not_finished" :key="i" :element="e"></list-element>
            </div>
        </div>
        <div id="list-elements-done">
            <h2>Tareas finalizadas:</h2>
            <div class="list-elements-container">
                <list-element class="list-element" v-for="(e,i) in elements.finished" :key="i" :element="e" :numid="i" :finished="true"></list-element>
            </div>
        </div>
        <element-preview ref="preview"></element-preview>
    </div>
</template>

<script>
import ListElement from './ListElement.vue';
import ElementPreview from './ElementPreview';
export default {
    name: 'ListElements',
    data: function(){
        return{
            title: 'Mis tareas',
            listCount: 0,
            elements: {
                "not_finished": [
                    {
                        "id": 0,
                        "title": "Terminar el Proyecto de Vue",
                        "description": "Dashboard para gestionar tus tareas",
                        "tags": ["Subir link al campus", "máxima prioridad", "40 % nota final"]
                    },
                    {
                        "id": 1,
                        "title": "Subir a repo",
                        "description": "Github",
                        "tags": ["Comprobar SSH keys"]
                    },
                    {
                        "id": 2,
                        "title": "Comprar entradas avengers",
                        "description": "",
                        "tags": ["15 €"]
                    }
                ],
                "finished":[
                    {
                        "id": 3,
                        "title": "BrainStorming ideas para el proyecto",
                        "description": "",
                        "tags": ["Gestor de tareas", "Panel de control de dispositivos"]
                    }
                ]
            }
        }
    },
    components: {
        'list-element': ListElement,
        'element-preview': ElementPreview
    },
    methods: {
        getNewId(listType){
            let index = 0;
            this.elements[listType].map((e) => {
                if(e.id > index)
                    index = e.id;
            });
            return index;
        }
    }
}
</script>

<style lang="scss">
#list-elements{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
    margin-top: 20px;
    #list-elements-to-do, #list-elements-done{
        flex: 1;
    }
    .list-elements-container{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: flex-start;
        align-items: center;
        width: 100%;
        padding: 10px 20px 10px 0px;
        overflow: hidden;
    }
}
</style>
