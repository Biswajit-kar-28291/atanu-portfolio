<script>
import Navbar from '@/components/Navbar.vue';
import Intro from './components/Intro.vue';
import Project from './components/Project.vue';
import Aboutme from './components/Aboutme.vue';
import Contact from './components/Contact.vue';
export default{
  components:{
    Navbar,
    Intro,
    Project,
    Aboutme,
    Contact
  }
}
</script>

<template>
  
  <Navbar/>
  <Intro/>
  <Aboutme/>
  <Project/>
  <Contact/>
</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body{
  overflow-x: hidden;
}
</style>


