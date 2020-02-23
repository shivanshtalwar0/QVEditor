<template>
    <div>
        <q-v-editor :value="htmlcontent" @onHtmlChange="onHtmlChange"></q-v-editor>

        <h2>Generated html</h2>
        <div v-html="generatedhtml">
        </div>

    </div>
</template>

<script>
    import QVEditor from "qveditor";
    import Vue from 'vue'
    Vue.use(QVEditor)
    export default {
        name: "example",
        methods:{
          onHtmlChange(data){
              this.generatedhtml=data
          }
        },
        data(){
            return{
                htmlcontent:"<p>hello</p>",
                generatedhtml:""
            }
        }
    }
</script>

<style scoped>

</style>
