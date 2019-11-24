<template>
  <div>
    <fieldset> 
        <legend>Modo</legend>
        <label>
            <input type="radio" name="mode" id="mode_Auto" value="automatico" checked= "checked" v-model="auto"> Automatico
        </label>
        <label>
            <input type="radio" name="mode" id= "mode_Manual" value="manual" v-model="auto"> Manual
        </label>
        <label>
            <button id="button_jugar" v-on:click= "jugar()">Jugar</button>
        </label>
    </fieldset>
  </div>
</template>

<script>
const MODO_AUTO = "automatico";
const MODO_VERSUS = "manual";
export default {
  name: 'ModeSelector',
  props: ["eventbus"],
  data: function(){
    var auto = MODO_AUTO;
    return{
        auto:auto
    }
  },
  methods:{
    jugar(){
        switch (this.auto){
            case MODO_AUTO:
                this.eventbus.$emit("jugar_auto");
                break;
            case MODO_VERSUS:
                 this.eventbus.$emit("jugar_versus");
                 break;
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
