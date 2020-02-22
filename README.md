## QVEditor
##### Simple yet feature rich Quill based text editor for Vue.js
##### Best Suited for all kinds of text related formatting

#### Install
    npm install QVEditor --save

#### API & Usage 
    <template>
    
    <QVEditor :value="initialEditorContent" @onHtmlChange="gethtmlcontent"/>
    
    </template>
    <script>
    import QVEditor from 'QVEditor'
    export default{
    methods:{
    gethtmlcontent(html){
    console.log(html)
    },
    data(){
    return{
    initialEditorContent:"<p>hello world</p>"
    }
    }
    
    }
    
    }
    
    </script>    
