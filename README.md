## QVEditor
##### Simple yet feature rich Quill based text editor for Vue.js
##### Best Suited for all kinds of text related formatting

#### Preview
![preview image](https://github.com/shivanshtalwar0/QVEditor/raw/master/images/preview.png)

#### Install
    npm install qveditor --save

#### API & Usage
##### without SSR(server side rendering) 
    <template>
        <div>
            <q-v-editor :value="htmlcontent" @OnHtmlChange="onHtmlChange"></q-v-editor>
            
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

##### with SSR using Nuxt.js
 step 1:`create qveditor.js in plugins directory with following content`
        
        import Vue from 'vue'
        import QVEditor from 'qveditor'
        Vue.use(QVEditor)
step 2: `in plugins array of nuxt.config.js add the following content`
            
            plugins: [
            {src:"@/plugins/qveditor",ssr:false}
            ],
  
