<template>
  <div id="app">
    <div class="header">
        <h1>{{ title }}</h1>
    </div>
    <ListElements></ListElements>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material';
import 'vue-material/dist/vue-material.min.css';

Vue.use(VueMaterial);

import { library } from '@fortawesome/fontawesome-svg-core'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'
import { faCalendarAlt } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faCalendarAlt)

Vue.component('font-awesome-icon', FontAwesomeIcon)

import ListElements from './components/ListElements.vue'
export default {
  name: 'app',
  data: function(){
    return{
      title: "Mis tareas"
    }
  },
  components: {
    ListElements
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat');
$color1: rgba(0, 30, 66, 1);
$color2: rgba(247, 245, 242, 1);
*{
  margin: 0;
  padding: 0;
}
#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  width: 100%;
  font-family: 'Montserrat', Regular;
  .separator{
      width: 100%;
      height: 1px;
      background: rgb(126, 119, 103);
  }
}
.header{
    background-color: $color1;
    padding: 30px;
    h1 {
      color: $color2;
      text-align: center;
      font-size: 1.7em;
    }
}

</style>
