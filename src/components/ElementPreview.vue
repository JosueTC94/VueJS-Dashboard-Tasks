<template>
import { close } from 'fs';
    <div id="element-preview-container">
        <div id="form-add-event" v-if="ifSelectedElement == false">
            <h2>Añadir nueva tarea:</h2>
            <form>
                <div class="form-group">
                    <label :for="newElement.title">Título: </label>
                    <input :id="newElement.title" v-model.lazy="newElement.title"/>
                </div>
                <div class="form-group">
                    <label :for="newElement.title">Subtítulo: </label>
                    <input v-model.lazy="newElement.subtitle" />
                </div>
                <div class="form-group">
                    <label :for="newElement.description"> Descripción: </label>
                    <textarea v-model.lazy="newElement.description"></textarea>
                </div>
                <div class="form-group">
                    <label for="element-date">Fecha:</label>
                    <input type="datetime-local" id="element-date" name="element-date" v-model="newElement.date"/>
                </div>
                <div class="form-group">
                    <label :for="newElement.img">Añadir imágenes: </label>
                    <input :id="newElement.img" type="file" multiple/>
                </div>
                 <div class="form-group form-group-checkbox">
                    <span>Etiquetas:</span>
                    <div class="checkbox-container">
                        <div v-for="(tag,i) in newElement.tags" :key="i">
                            <input type="checkbox" :value="tag" checked>
                            {{tag}}
                        </div>
                    </div>
                </div>
                <div class="form-group">
                    <input type="text" placeholder="Nueva etiqueta" v-model.lazy="newTag"/>
                    <input type="button" value="Añadir" @click="addTag"/>
                </div>
                <div class="form-group">
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
                    <label :for="selectedElement.content.subtitle"> Subtítulo: </label>
                    <input v-model.lazy="selectedElement.content.subtitle" />
                </div>
                <div class="form-group">
                    <label :for="selectedElement.content.description"> Descripción: </label>
                    <textarea v-model.lazy="selectedElement.content.description"></textarea>
                </div>
                <div class="form-group">
                    Finalizada:
                    <input type="radio" name="element-state" value="true"  v-model="selectedElement.finished">Si
                    <input type="radio" name="element-state" value="false" v-model="selectedElement.finished">No
                </div>
                <div class="form-group">
                    <label for="element-date">Fecha:</label>
                    <input type="datetime-local" id="element-date" name="element-date" v-model="selectedElement.content.date"/>
                </div>
                <div class="separator"></div>
                <!-- <div class="form-group">
                    <label :for="selectedElement.img">Imágenes: </label>
                    <div :id="selectedElement.img" class="img-container">
                        <img v-for="(e,i) in selectedElement.content.img" :key="i" :src="getImage(e)" />
                    </div>
                </div> -->
                <div class="form-group form-group-checkbox">
                    <span>Etiquetas:</span>
                    <div class="checkbox-container">
                        <div v-for="(tag,i) in selectedElement.content.tags" :key="i">
                            <input type="checkbox" :value="tag" checked>
                            {{tag}}
                        </div>
                    </div>
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
                type: String,
                default: ''
            }
        }
    },
    mounted(){
        console.log("ifSelectedElement => " + JSON.stringify(this.ifSelectedElement));
    },
    methods: {
        setElement(task, id, finished){
            this.selectedElement = {};
            this.selectedElement.content = task;
            this.ifFinished = String(finished);
            this.selectedElement.finished = String(finished);
            this.ifSelectedElement = true;
        },
        getImage(rute){
            var images = require.context('../assets/', false);
            return images('./'+rute);
        },
        updateEvent(){
            console.log("updateEvent => " + this.selectedElement.finished + "ifFinished => " + this.ifFinished);
            let index = -1;
            if(this.ifFinished.localeCompare("false") == 0){
                
                index = this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id);
                console.log("!this.selectedElement.finished => " + index + ", element content => " + JSON.stringify(this.selectedElement.content));
                if(this.selectedElement.finished.localeCompare(this.ifFinished) == 0){
                    this.$parent.elements.not_finished[index] = this.selectedElement.content
                }else{
                    console.log("else 1:"+JSON.stringify(this.selectedElement))
                    this.$parent.elements.finished.push(this.selectedElement.content)
                    this.$parent.elements.not_finished.splice(this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id), 1)
                }   
            } else {

                index = this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id);
                console.log("this.selectedElement.finished => " + index + ", element content => " + JSON.stringify(this.selectedElement.content));

                if(this.selectedElement.finished.localeCompare(this.ifFinished) == 0){
                    this.$parent.elements.finished[index] = this.selectedElement.content;
                }else{
                    console.log("else 2:"+JSON.stringify(this.selectedElement))
                    this.$parent.elements.not_finished.push(this.selectedElement.content);
                    this.$parent.elements.finished.splice(this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id), 1);
                }
            }
            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        cancelUpdate(){
            // console.log("cancelUpdate");
            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        addEvent(){
            // console.log("addEvent");
            this.$parent.elements.not_finished.push(this.newElement);
        },
        removeEvent(){
            console.log("removeEvent => " + this.selectedElement.content.id);
            this.ifFinished.localeCompare("false") == 0 ? 
                this.$parent.elements.not_finished.splice(this.$parent.elements.not_finished.findIndex(e => e.id == this.selectedElement.content.id), 1)
                :
                this.$parent.elements.finished.splice(this.$parent.elements.finished.findIndex(e => e.id == this.selectedElement.content.id), 1)

            this.selectedElement = {};
            this.ifSelectedElement = false;
        },
        addTag(){
            if(this.selectedElement != undefined && Object.keys(this.selectedElement).length > 0){
                this.selectedElement.content.tags != undefined &&
                    this.selectedElement.content.tags.push(this.newTag);
            }else{
                this.newElement.tags != undefined ?
                    this.newElement.tags.push(this.newTag)
                    :
                    console.log("tags  == undefined")
                    this.newElement.tags = [];
                    this.newElement.tags.push(this.newTag);
            }
            
            this.newTag = "";

            console.log(this.selectedElement.content.tags);
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
        }
        .form-group-checkbox{
            align-items: flex-start;
            span {
                flex-basis: 30%;
            }
            div.checkbox-container{
                flex-basis: 70%;
                display: flex;
                flex-direction: row;
                flex-wrap: wrap;
                justify-content: space-between;
            }
        }
        .form-group-buttons{
            input[type='button']{
                padding: 10px;
                border: 0;
                margin-bottom: 5px;
            }
            input[value='Actualizar']{
                flex-basis: 100%;
                background: rgba(52, 119, 188, 1);
                color: rgba(247, 245, 242, 1);
            }
            input[value='Borrar']{
                flex-basis: 55%;
                margin-right: 1%;
            }
            input[value='Cancelar']{
                flex-basis: 40%;
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
