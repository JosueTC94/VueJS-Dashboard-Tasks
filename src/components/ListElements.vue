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
                        "title": "Elemento 1",
                        "subtitle": "Subtítulo del elemento 1",
                        "description": "Descripción del elemento 1",
                        "img": ["element_1.jpg"],
                        "tags": ["biología", "máxima prioridad", "50 % nota"]
                    },
                    {
                        "id": 1,
                        "title": "Elemento 2",
                        "subtitle": "Subtítulo del elemento 2",
                        "tags": []
                    },
                    {
                        "id": 2,
                        "title": "Elemento 3",
                        "subtitle": "Subtítulo del elemento 3",
                        "tags": []
                    },
                    {
                        "id": 3,
                        "title": "Elemento 4",
                        "subtitle": "Subtítulo del elemento 1",
                        "description": "Descripción del elemento 1",
                        "img": ["element_1.jpg"],
                        "tags": []
                    },
                    {
                        "id": 4,
                        "title": "Elemento 5",
                        "subtitle": "Subtítulo del elemento 2",
                        "tags": []
                    },
                    {
                        "id": 5,
                        "title": "Elemento 6",
                        "subtitle": "Subtítulo del elemento 3",
                        "tags": []
                    }
                ],
                "finished":[
                    {
                        "id": 6,
                        "title": "Elemento 6",
                        "subtitle": "Subtítulo del elemento 3",
                        "tags": []
                    }
                ]
            }
        }
    },
    components: {
        'list-element': ListElement,
        'element-preview': ElementPreview
    },
    methods: {}
}
</script>

<style lang="scss">
$color1: rgba(0, 30, 66, 1);
$color2: rgba(52, 119, 188, 1);
$color3: rgba(247, 245, 242, 1);
$color4: rgba(219, 202, 173, 1);
$color5: rgba(172, 150, 104, 1);
#list-elements{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
    margin-top: 20px;
    #list-elements-to-do, #list-elements-done{
        flex: 0.5;
    }
    .list-elements-container{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: flex-start;
        align-items: center;
        width: 100%;
        padding: 10px 20px 10px 0px;
        // border: 1px dashed $color1;
        overflow: hidden;
    }
}
</style>
