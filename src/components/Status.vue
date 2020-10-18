<template>
  <div class="status">
    <div id="statuslog" class="statusCenter" ><img id="statusimage" v-bind:src="status" /> </div>
  </div>
</template>

<script>
export default {
  name: 'Status',
  props: {
    status: { default: "./images/ajax-loader.gif", type: String  },
    statuscss: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.statusCenter
{
    position: fixed;
  top: 50%;
  left: 50%;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);
}

.StatusHidden {
  margin: auto;
  position: absolute;
  top: 0; left: 0; bottom: 0; right: 0;
  width: 256px;
  height: 256px;
  border:0px;
  visibility: hidden;
}
.StatusShow {
  margin: auto;
  position: absolute;
  top: 0; left: 0; bottom: 0; right: 0;
  width: 256px;
  height: 256px;
  border:0px;
  visibility:visible;
}
</style>
