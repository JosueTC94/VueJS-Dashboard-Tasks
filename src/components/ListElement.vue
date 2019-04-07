<template>
    <div class="list-element-container" @click="showDetail()">
        <span class="element-title">{{ element.title }}</span>
        <div class="separator"></div>
        <span class="element-subtitle">{{ element.subtitle }}</span>
        <ul class="tags-container">
            <li v-for="(tag,i) in element.tags" :key="i">{{ tag }}</li>
        </ul>
        <div class="element-date" v-if="element.date != undefined">
            <font-awesome-icon icon="calendar-alt" />
            <span>{{ element.date }}</span>
        </div>
    </div>
</template>

<script>


export default {
    name: 'ListElement',
    props: {
        element:{
            type: Object,
            default: () => ({})
        },
        finished: {
            type: Boolean,
            default: false
        }
    },
    data: function(){
        return {}
    },
    computed: {
        preview(){ return this.$parent.$refs.preview }
    },
    mounted(){},
    methods: {
        showDetail(){
            // console.log(this.numid);
            this.preview.setElement(this.element, this.element.id, this.finished);
        }
    }
}
</script>

<style lang="scss" scoped>
$color1: rgba(0, 30, 66, 1);
$color2: rgba(52, 119, 188, 1);
$color3: rgba(247, 245, 242, 1);
$color4: rgba(219, 202, 173, 1);
$color5: rgba(172, 150, 104, 1);
.list-element-container{
    display: flex;
    flex-direction: column !important;
    flex-wrap: wrap;
    justify-content: flex-start;
    // height: 30vh;
    // border: 1px solid $color5;
    background: #FFE082;
    font-size: 1.2em;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.3);
    // color: $color3;
    padding: 10px;
    margin-top: 10px;
    margin-right: 3%;
    overflow: hidden;
    .element-subtitle, .element-date{
        font-size: 0.8em;
    }
    .element-date{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-evenly;
        align-items: center;
        margin-top: 5%;
    }
    ul.tags-container{
        margin-left: 20px;
        li{
            list-style-type: circle;
            font-size: 0.8em;
        }
    }
    .separator{
        margin: 5px 0;
    }
}
.list-element-container:hover{
        cursor: pointer;
        background: 3px 3px rgb(219, 190, 102);
}
</style>
