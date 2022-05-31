<template>
  <div id="app">
    <div class="vx">
    <div>
        <left-nav></left-nav>
    </div>
    <div>
    <router-view></router-view>
    </div>
    </div>
     
  </div>
</template>

<script>
import leftNav from  '@/components/conner/leftNav'
 
export default {
  name: 'App',
  components:{
        leftNav 
  }
}
</script>

<style>
 .vx{
    display: flex;
 }
</style>
