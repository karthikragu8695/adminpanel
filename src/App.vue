<template>
  <v-app>
    {{ user }}
    <hello-world v-if="!login"></hello-world>
    <layout-view v-else></layout-view>
  </v-app>
</template>

<script>
import HelloWorld  from './components/HelloWorld.vue'
import LayoutView from './views/LayoutView.vue';
import { supabase } from './supabase'
export default {
  name: 'App',
  data(){
    return{
      login:false
    }
  },
  components:{
    HelloWorld,
    LayoutView
  },

  mounted(){
    supabase.auth.onAuthStateChange((event) => {
      if (event == 'SIGNED_IN' ) {
          const user = supabase.auth.getUser()
          this.login = true
          console.log(user);
      }
      if (event == 'SIGNED_OUT') {
        this.login = false
      }
    })
  },
}
</script>
