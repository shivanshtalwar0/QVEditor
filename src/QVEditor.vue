<template>
    <div>
        <div class="toolbar-container" :id="'toolbar-container-'+toolbarKey">
            <span class="ql-formats">
                <select class="ql-align"></select>
                <select class="ql-font">
                <template v-for="(font,index) in fonts">
                    <option v-if="index===1" selected :value="font" :key="font">{{font}}</option>
                    <option v-else :value="font" :key="font">{{font}}</option>
                </template>
            </select>
            <select class="ql-size">
                <option value="small"></option>
                <!-- Note a missing, thus falsy value, is used to reset to default -->
                <option selected></option>
                <option value="large"></option>
                <option value="huge"></option>
            </select>
            <button class="ql-bold"></button>
            <button class="ql-italic"></button>
            <button class="ql-link"></button>
             <select class="ql-color">
                </select>
            <select class="ql-background">
            </select>
        </span>

        </div>
        <div class="editor-container" :id="'editor-container-'+editorKey" v-html="value">
        </div>
    </div>
</template>
<script>
    import Quill from 'quill';
    import uuid from 'uuid';
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.bubble.css';
    import 'quill/dist/quill.snow.css';

    export default {
        data() {
            return {
                quill: null,
                editorKey: "",
                toolbarKey: ""
            }
        },
        mounted() {
            let Font = Quill.import('formats/font');
            Font.whitelist = this.fonts;
            this.generateFontClasses(this.fonts);
            Quill.register(Font, true);
            this.quill = new Quill('#editor-container-' + this.editorKey, {
                modules: {
                    toolbar: {
                        container: '#toolbar-container-' + this.toolbarKey,
                    }
                },
                placeholder: 'Compose an epic...',
                theme: 'snow'
            });


            this.$emit('OnHtmlChange',this.onHtmlChange(this.value));
            this.quill.on('text-change', (text) => {
                this.$emit('OnHtmlChange',this.onHtmlChange(this.quill?.root.innerHTML));

            });

        },
        props: {
            value: {
                default: ""
            },
            fonts: {
                type: Array,
                default: () => ['slabo27px', 'montserrat', 'opensans', 'lato', 'raleway', 'avenir', 'sans-serif', 'serif', 'cursive', 'verdana', 'Roboto']
            }
        },
        created() {
            this.editorKey = uuid.v4();
            this.toolbarKey = uuid.v4();
        },
        methods: {
            onHtmlChange(data) {
                let container = document.createElement('div');
                let div = document.createElement('div');
                div.className = 'ql-editor';
                div.innerHTML = data;
                container.appendChild(div);
                return container.innerHTML;
            },
            getFontName(font) {
                return font.replace(/\s/g, '-');
            },
            generateFontClasses(fonts) {
                let style = document.createElement('style');
                style.id = 'injectcss';
                let fontStyles = '';

                fonts.forEach((font) => {
                    let fontName = this.getFontName(font);
                    console.log(fontName.toLowerCase());
                    fontStyles += '.ql-snow .ql-picker.ql-font .ql-picker-label[data-value=' + fontName.toLowerCase() + ']::before, .ql-snow .ql-picker.ql-font .ql-picker-item[data-value=' + fontName.toLowerCase() + ']::before {' +
                        'content: \'' + font + '\';' +
                        'font-family: \'' + font + '\', sans-serif;' +
                        '}' +
                        '.ql-font-' + fontName.toLowerCase() + '{' +
                        ' font-family: \'' + font + '\', sans-serif;' +
                        '}';
                });
                style.innerHTML = fontStyles;
                document.body.appendChild(style);
            }
        },


        beforeDestroy() {
            this.quill.off('text-change', () => {
            });
        }


    }


</script>

<style>

    @import url('https://fonts.googleapis.com/css?family=Lato|Montserrat|Open+Sans|Raleway|Roboto|Slabo+27px&display=swap');

    .editor-container {
        font-family: "sans-serif";
        height: auto;
        width: 100%;
    }
    .toolbar-container {
        font-family: "sans-serif";
        height: auto;
        width: 100%;
    }

    .container a {
        font-family: Helvetia, sans-serif;
    }

</style>
