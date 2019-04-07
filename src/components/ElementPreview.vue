<template>
    <div id="element-preview-container">
        <div id="form-add-event" v-if="ifSelectedElement == false">
            <h2>Añadir nueva tarea:</h2>
            <form>
                <div class="form-group">
                    <label for="add-element-title">Título: </label>
                    <input id="add-element-title" v-model.lazy="newElement.title"/>
                </div>
                <div class="form-group">
                    <label :for="newElement.description"> Descripción: </label>
                    <textarea v-model.lazy="newElement.description"></textarea>
                </div>
                <div class="form-group">
                    <label for="element-date">Fecha:</label>
                    <input type="datetime-local" id="element-date" name="element-date" v-model="newElement.date"/>
                </div>
                 <div class="form-group form-group-tags">
                    <span>Etiquetas:</span>
                    <ul class="tags-container">
                        <li v-for="(tag,i) in newElement.tags" :key="i">{{ tag }}</li>
                    </ul>
                </div>
                <div class="form-group">
                    <input type="text" placeholder="Nueva etiqueta" v-model.lazy="newTag"/>
                    <input type="button" value="Añadir" @click="addTag"/>
                </div>
                <div class="form-group form-group-buttons">
                    <span id="add-event-info" v-if="newElement.title == undefined || (newElement.title != undefined && newElement.title.trim().length < 1)">
                        Introduzca, como mínimo, el título de la tarea
                    </span>
                    <input type="button" value="Añadir"  @click="addEvent"/>
                </div>
            </form>
        </div>
        <div id="form-update-element" v-else>
            <h2> {{ selectedElement.content.title }} </h2>
            <form>
                <div class="form-group">
                    <label :for="selectedElement.content.title">Título: </label>
                    <input :id="selectedElement.content.title" v-model.lazy="selectedElement.content.title"/>
                </div>
                <div class="form-group">
                    <label :for="selectedElement.content.description"> Descripción: </label>
                    <textarea v-model.lazy="selectedElement.content.description"></textarea>
                </div>
                <div class="form-group">
                    Finalizada:
                    <input type="radio" name="element-state" value="true"  v-model="selectedElement.finished" @click="changeState(true)">Si
                    <input type="radio" name="element-state" value="false" v-model="selectedElement.finished" @click="changeState(false)">No
                </div>
                <div class="form-group">
                    <label for="element-date">Fecha:</label>
                    <input type="datetime-local" id="element-date" name="element-date" v-model="selectedElement.content.date"/>
                </div>
                <div class="separator"></div>
                <div class="form-group form-group-tags">
                    <span>Etiquetas:</span>
                    <ul class="tags-container">
                        <li v-for="(tag,i) in selectedElement.content.tags" :key="i">{{ tag }}</li>
                    </ul>
                </div>
                <div class="form-group">
                    <input type="text" placeholder="Nueva etiqueta" v-model.lazy="newTag"/>
                    <input type="button" value="Añadir" @click="addTag"/>
                </div>
                <div class="form-group form-group-buttons">
                    <input type="button" value="Actualizar" @click="updateEvent"/>
                    <input type="button" value="Borrar" @click="removeEvent" />
                    <input type="button" value="Cancelar" @click="cancelUpdate"/>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ElementPreview',
    data: function(){
        return{
            selectedElement:{
                type: Object,
                default: ( ()=> {} )
            },
            newElement: {
                type: Object,
                default: ( () => {} )
            },
            ifSelectedElement: false,
            ifFinished: {
                type: String
            },
            newTag: {
                type: String
            }
        }
    },
    mounted(){
        this.newTag = '';
    },
    methods: {
        setElement(task, id, finished){
            this.selectedElement = {};
            this.selectedElement.content = task;
            this.ifFinished = String(finished);
            this.selectedElement.finished = String(finished);
            this.ifSelectedElement = true;
        },
        updateEvent(){
            let index = -1;
            if(this.ifFinished.localeCompare("false") == 0){
                index = this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id);
                this.$parent.elements.not_finished[index] = this.selectedElement.content 
            } else {
                index = this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id);
                this.$parent.elements.finished[index] = this.selectedElement.content;
            }
            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        cancelUpdate(){
            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        addEvent(){
            if(this.newElement.title != undefined && this.newElement.title.trim().length > 0){
                this.newElement.id = this.$parent.getNewId('not_finished') + 1;
                this.$parent.elements.not_finished.push(this.newElement)
            }else{   
                document.querySelector('#add-element-title').style.borderColor = "red";
            }
            this.newElement = {};
        },
        removeEvent(){
            this.ifFinished.localeCompare("false") == 0 ? 
                this.$parent.elements.not_finished.splice(this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id), 1)
                :
                this.$parent.elements.finished.splice(this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id), 1)

            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        addTag(){
            if(this.ifSelectedElement){
                this.selectedElement.content.tags != undefined &&
                    this.selectedElement.content.tags.push(this.newTag);
            }else{
                if(this.newElement.tags != undefined){
                    this.newTag.length > 0 && this.newElement.tags.push(this.newTag)
                }else{
                    this.newElement.tags = [];
                    this.newTag.length > 0 && this.newElement.tags.push(this.newTag)
                }
            }
            
            this.newTag = "";
        },
        changeState(finished){
            if(this.ifFinished.localeCompare(String(finished)) != 0){
                let index = -1;
                if(finished){
                    index = this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id);
                    this.$parent.elements.finished.push(this.selectedElement.content)
                    this.$parent.elements.not_finished.splice(index, 1)
                }else{
                    index = this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id);
                    this.$parent.elements.not_finished.push(this.selectedElement.content);
                    this.$parent.elements.finished.splice(index, 1);
                }
                this.ifFinished = String(finished);
            }
        }
    }
}
</script>

<style lang="scss" scoped>
#element-preview-container{
    padding: 30px;
    margin: 10px;
    width: 30%;
    
    background: rgb(235, 231, 224);
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.3);
    
    h2{
        font-size: 1.3em;
        text-align: center;
    }
    form{
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        justify-content: center;
        width: 100%;
        .form-group{
            width: 100%;
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            margin-top: 5%;
            label{
                flex: 1;
            }
            input, textarea{
                flex: 2;
                padding: 7px;
                font-size: 0.9em;
                font-family: 'Montserrat', Regular;
            }
            input[type='button']{
                flex: 1;
            }
            span#add-event-info{
                margin-bottom: 10px;
            }
        }
        .form-group-tags{
            align-items: flex-start;
            span {
                flex-basis: 30%;
            }
            ul.checkbox-container{
                flex-basis: 70%;
                display: flex;
                flex-direction: column;
                flex-wrap: wrap;
                justify-content: flex-start;
            }
        }
        .form-group-buttons{
            input[type='button']{
                padding: 10px;
                border: 0;
                margin-bottom: 5px;
            }
            input[value='Actualizar'], input[value='Añadir']{
                flex-basis: 100%;
                background: rgba(52, 119, 188, 1);
                color: rgba(247, 245, 242, 1);
            }
            input[value='Borrar']{
                flex-basis: 55%;
                margin-right: 1%;
                background: #e53935;
                color: rgba(247, 245, 242, 1)
            }
            input[value='Cancelar']{
                flex-basis: 40%;
                background: #757575;
                color: rgba(247, 245, 242, 1);
            }
        }
        input[type='button']:hover{
            cursor: pointer;
        }
        .separator{
            margin-top: 5%;
        }
    }
}
</style>
