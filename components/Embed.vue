<template>
<div>
    <div class="sidebar">
        <div class="home">
            <img style="cursor: pointer" v-on:click="Swtich('home')" src="@/assets/img/rotator-light.svg" />

            <div class="sidehub">

                <button v-on:click="Swtich('color')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-color-swatch-icon class="w-5 h-5" />
                    <span>Color</span>
                </button>

                <button v-on:click="Swtich('author')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-users-icon class="w-5 h-5" />
                    <span>Author</span>
                </button>

                <button v-on:click="Swtich('title')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-bookmark-icon class="w-5 h-5" />
                    <span>Title</span>
                </button>

                <button v-on:click="Swtich('description')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-pencil-icon class="w-5 h-5" />
                    <span>Description</span>
                </button>

                <!-- <button v-on:click="Swtich('fields')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-menu-icon class="w-5 h-5" />
                    <span>Fields</span>
                </button>

                 <button v-on:click="Swtich('thumbnail')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-photograph-icon class="w-5 h-5" />
                    <span>Images</span>
                </button>

                <button v-on:click="Swtich('footer')" class="Button font-bold py-2 px-4 rounded inline-flex items-center">
                    <outline-arrow-circle-down-icon class="w-5 h-5" />
                    <span>Footer</span>
                </button> -->

            </div>
        </div>
    </div>

    <v-app class="content min-h-full">

        <div id="welcome" v-if="section === 'home'">
            <h1>Welcome to EmbedCook</h1>
            <div style="margin: 15px 0px;">
                <div class="p-2 bg-gray-600 items-center text-indigo-100 leading-none lg:rounded-full flex lg:inline-flex" role="alert">
                    <span class="flex rounded-full bg-gray-700  uppercase px-2 py-1 text-xs font-bold mr-3">Get Started</span>
                    <span class="font-semibold mr-2 text-left flex-auto">Start building your discord embed by clicking on the sidenavigation on the left of your screen</span>
                </div>
            </div>

        </div>

        <div v-if="section !== 'home'" class="col-8">

            <div class="controller">

                <div v-if="section === 'color'">
                    <v-color-picker v-model="color" dot-size="5" swatches-max-height="250"></v-color-picker>
                </div>

                <div v-if="section === 'author'">

                    <button v-on:click="authorurl = `https://`" v-if="authorurl === ``" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center">
                        <outline-link-icon class="w-4 h-4" />
                        <span style="padding-left: 10px;">add url endpoint</span>
                    </button>

                    <button v-on:click="authorimg = `https://`" v-if="authorimg === ``" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center">
                        <outline-photograph-icon class="w-4 h-4" />
                        <span style="padding-left: 10px;">add image endpoint</span>
                    </button>

                    <br v-if="authorurl === `` || authorimg === ``"><br v-if="authorurl === `` || authorimg === ``">

                    <v-text-field counter maxlength="256" v-model="author" label="Author" placeholder="Author of this embed" outlined></v-text-field>

                    <v-text-field v-if="authorurl !== ``" v-model="authorurl" label="url" placeholder="https://" outlined></v-text-field>

                    <v-text-field v-if="authorimg !== ``" v-model="authorimg" label="image url" placeholder="https://" outlined></v-text-field>

                </div>

                <div v-if="section === 'fields'">

                    <br><br><br>
                    {{fields}}
                    <br><br><br>

                </div>

                <div v-if="section === 'title'">

                    <button v-on:click="titleurl = `https://`" v-if="titleurl === ``" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center">
                        <outline-link-icon class="w-4 h-4" />
                        <span style="padding-left: 10px;">add url endpoint</span>
                    </button>
                    <br v-if="titleurl === ``"><br v-if="titleurl === ``">

                    <v-text-field counter maxlength="256" v-model="title" label="Title" placeholder="Your title here" outlined>
                    </v-text-field>

                    <v-text-field v-if="titleurl !== ``" v-model="titleurl" label="url" placeholder="https://" outlined></v-text-field>

                </div>

                <div v-if="section === 'description'">
                    <v-textarea rows="1" counter maxlength="2048" v-model="description" label="Description" placeholder="A very neat description" outlined full-width single-line></v-textarea>
                </div>

            </div>

            <div class="conference">
                <div class="interactive" v-bind:style="{ background: color }"></div>
                <div class="embed edge">
                    <div class="charter">
                        <div class="charter-inner">

                            <div v-if="author.length > 0" class="author">
                                <img v-if="authorimg.length > 0" class="author-icon" v-bind:src="authorimg">
                                <vue-markdown-lite>{{author}}</vue-markdown-lite>
                            </div>

                            <div class="title" v-if="title.length > 0 && titleurl === ``">
                                <vue-markdown-lite>{{title}}</vue-markdown-lite>
                            </div>

                            <div class="title" v-if="title.length > 0 && titleurl !== ``">
                                <a v-bind:href="titleurl" target="_blank">
                                    <vue-markdown-lite>{{title}}</vue-markdown-lite>
                                </a>
                            </div>

                            <div class="description" v-if="description.length > 0">
                                <vue-markdown-lite>{{description}}</vue-markdown-lite>
                            </div>

                            <div class="embedFields">
                                <div v-for="item in fields" :key="item">

                                    <div v-if="item.inline === true" class="embedField embedField inline">
                                        <div class="embedField-name">
                                            <vue-markdown-lite>{{item.name}}</vue-markdown-lite>
                                        </div>
                                        <div class="embedField-value">
                                            <vue-markdown-lite>{{item.value}}</vue-markdown-lite>
                                        </div>
                                    </div>
                                    <div v-else style="width: 100vw;" class="embedField embedField">
                                        <div class="embedField-name">
                                            <vue-markdown-lite>{{item.name}}</vue-markdown-lite>
                                        </div>
                                        <div class="embedField-value">
                                            <vue-markdown-lite>{{item.value}}</vue-markdown-lite>
                                        </div>
                                    </div>

                                </div>
                            </div>

                        </div>
                 
                    </div>
                </div>
            </div>

        </div>

        <div v-if="section !== 'home'" class="output">
            const Embed = {
            color: "{{color}}",
            title: "{{title}}",
            url: "{{titleurl}}",
            author: {
            name: "{{author}}",
            icon_url: "{{authorimg}}",
            url: "{{authorurl}}",
            },
            description: "{{description}}",
            thumbnail: {
            url: "{{thumb}}",
            },
            image: {
            url: "{{img}}",
            },
            timestamp: new Date(),
            footer: {
            text: "{{footer}}",
            icon_url: "{{footerimg}}",
            }

            }; message.channel.send({ embed: Embed });

        </div>

    </v-app>
</div>
</template>

<script>
import Vue from 'vue'
import VueMarkdownLite from '@earthtone/vue-markdown-lite'
import draggable from "vuedraggable";

Vue.use(VueMarkdownLite);

export default {
    components: {
        draggable
    },
    computed: {
        draggingInfo() {
            return this.dragging ? "under drag" : "";
        }
    },
    methods: {
        Swtich: function (sec) {
            this.section = sec
            localStorage.setItem("section", sec)

        },
    },
    computed: {
        color: {
            get(hue) {
                return localStorage['setItem']('EmbedHex', this['hex']), this['hex'];
            },
            set(hue) {
                this['hex'] = hue
            },
        },
    },
    data() {
        return {
            thumb: "",
            img: "",

            footer: "",
            footerimg: "",

            author: "",
            authorurl: "",
            authorimg: "",

            title: "",

            titleurl: "",

            description: "",
            hex: '#4f545c',
            // fields: [

            //     {
            //         name: 'Inline field x',
            //         value: 'Some value xa',
            //         inline: true,
                    
            //     },

            //     {
            //         name: 'Inline field xaa',
            //         value: 'Some value xaaa',
            //         inline: true,
            //     },
            //     {
            //         name: 'Inline field xaaaa',
            //         value: 'Some value xxaaaaa',
            //         inline: true,
            //     },

            // ],

            section: 'home',
            enabled: true,
        };
    }
};
</script>

<style scoped>
[class~=content] {
    width: 100%
}

[class~=content],
[class~=sidehub],
[class~=sidebar] {
    position: fixed
}

[class~=content] {
    padding-left: 333px
}

[class~=w-5][class~=h-5] {
    justify-content: center
}

[class~=w-5][class~=h-5] {
    flex-direction: column
}

[class~=content] {
    padding-top: 1.5625pc
}

[class~=Button] {
    display: block ruby
}

[class~=Button] {
    background: #202225
}

[class~=Button] {
    width: 90%
}

[class~=Button] {
    color: #fff
}

[class~=Button] {
    margin-left: auto
}

[class~=w-5][class~=h-5] {
    float: left
}

[class~=Button] {
    margin-bottom: 15pt
}

[class~=w-5][class~=h-5] {
    bottom: -2.25pt
}

[class~=Button] {
    margin-right: auto
}

[class~=Button] {
    margin-top: 15pt
}

[class~=Button] {
    padding-left: .104166667in
}

[class~=Button] {
    padding-bottom: .104166667in
}

[class~=Button] {
    padding-right: .104166667in
}

[class~=Button] {
    padding-top: .104166667in
}

[class~=conference] [class~=interactive] {
    width: 4px
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline],
[class~=embed] [class~=charter] [class~=charter-inner] {
    -webkit-box-flex: 1
}

[class~=embed][class~=edge],
.home,
[class~=w-5][class~=h-5] {
    position: relative
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline],
[class~=embed] [class~=charter] [class~=charter-inner] {
    -ms-flex: 1
}

[class~=conference] [class~=interactive] {
    background: #cacbce
}

[class~=conference] [class~=interactive] {
    border-radius: .1875pc 0 0 .03125in
}

.embed .embed-thumbnail img {
    margin-left: 0
}

.embed .embed-thumbnail img {
    margin-bottom: 0
}

[class~=sidehub] {
    width: 2.5in
}

[class~=sidebar],
[class~=sidehub] {
    height: 100%
}

[class~=conference] [class~=interactive] {
    -ms-flex-negative: 0
}

[class~=conference] [class~=interactive] {
    flex-shrink: 0
}

[class~=embed] [class~=charter] {
    display: -webkit-box
}

[class~=sidebar],
[class~=sidehub] {
    top: 0
}

[class~=embed] [class~=charter] {
    display: -ms-flexbox
}

[class~=embed][class~=edge] {
    display: -webkit-box
}

[class~=embed][class~=edge] {
    display: -ms-flexbox
}

[class~=sidehub] {
    left: 72px
}

[class~=embed] [class~=embedFields] {
    -webkit-box-orient: horizontal
}

[class~=sidebar] {
    width: 72px
}

[class~=embed] [class~=embedFields] {
    -webkit-box-direction: normal
}

[class~=embed] [class~=charter] {
    width: 100%
}

[class~=embed] [class~=charter] {
    display: -webkit-box
}

[class~=embed] [class~=charter] {
    display: -ms-flexbox
}

[class~=sidebar] {
    z-index: 10
}

[class~=embed] [class~=charter] {
    display: flex
}

[class~=embed] [class~=charter] {
    margin-bottom: .625pc
}

[class~=embed][class~=edge] {
    display: -webkit-box
}

[class~=embed][class~=edge] {
    display: -ms-flexbox
}

[class~=embed][class~=edge] {
    display: flex
}

.home {
    border-radius: .9375pc
}

[class~=embed][class~=edge] {
    border-radius: 0 2.25pt 2.25pt 0
}

.home {
    margin-left: .625pc
}

[class~=embed] [class~=image] {
    display: -webkit-box
}

[class~=embed] [class~=image] {
    display: -ms-flexbox
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline],
[class~=embed] [class~=charter] [class~=charter-inner] {
    flex: 1
}

.home {
    margin-bottom: .625pc
}

.home {
    margin-right: .625pc
}

[class~=embed] [class~=image] {
    display: flex
}

.home {
    margin-top: .625pc
}

[class~=embed] [class~=image] {
    overflow: hidden
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    max-height: 60pt
}

[class~=embed] [class~=image] {
    border-radius: 2px
}

.home {
    padding-left: 1.875pt
}

.home {
    padding-bottom: 1.875pt
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    max-width: .833333333in
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    border-radius: .1875pc
}

[class~=embed] [class~=charter-inner]>:last-child,
[class~=embed]>:last-child {
    margin-bottom: 0 !important
}

.home {
    padding-right: 1.875pt
}

.home {
    padding-top: 1.875pt
}

.embed .author {
    display: -webkit-box
}

.embed .author {
    display: -ms-flexbox
}

.embed .author {
    display: flex
}

.embed .author {
    -webkit-box-align: center
}

.embed .author {
    -ms-flex-align: center
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    width: auto
}

[class~=embed] [class~=author-icon],
[class~=embed][class~=edge] [class~=edge-thumb] {
    -o-object-fit: contain
}

[class~=embed] [class~=author-icon],
[class~=embed][class~=edge] [class~=edge-thumb] {
    object-fit: contain
}

[class~=controller] {
    border-bottom-width: 6px
}

[class~=controller] {
    border-bottom-style: dashed
}

.embed .author {
    align-items: center
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline] {
    min-width: 1.5625in
}

[class~=controller] {
    border-bottom-color: #2d2f32
}

[class~=controller] {
    border-image: none
}

[class~=controller] {
    width: 142%
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    -ms-flex-negative: 0
}

.embed .author {
    margin-bottom: 5px
}


#welcome h1 {
    font-size: .520833333in
}

[class~=controller] {
    margin-left: 0
}

[class~=controller] {
    margin-bottom: .260416667in
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    flex-shrink: 0
}

[class~=embed] [class~=embedFields] [class~=embedField] [class~=embedField-name],
[class~=embed] [class~=author-name] {
    font-size: .145833333in
}

[class~=embed][class~=edge] [class~=edge-thumb] {
    margin-left: 1.25pc
}

[class~=embed] [class~=author-name] {
    color: #4f545c !important
}

[class~=controller] {
    margin-right: 1.5625pc
}

.embed .settle,
[class~=Button] span {
    float: left
}

[class~=embed] [class~=author-icon] {
    margin-right: 6.75pt
}

[class~=embed] [class~=author-icon] {
    width: 15pt
}

[class~=embed] [class~=embed-title]+[class~=description] {
    margin-top: -.03125in !important
}

[class~=controller] {
    margin-top: .260416667in
}

[class~=controller] {
    padding-left: 0
}

[class~=embed] [class~=description][class~=markup] {
    white-space: pre-line
}

[class~=controller] {
    padding-bottom: 0.125pc;
}

[class~=embed] [class~=description][class~=markup] {
    margin-top: 0 !important
}

[class~=embed] [class~=author-icon] {
    height: .208333333in
}

[class~=controller] {
    padding-right: 37.5pt
}

[class~=controller] {
    padding-top: 0
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline] {
    -ms-flex-preferred-size: auto
}

[class~=conference],
[class~=image] {
    position: relative
}

[class~=image] {
    -webkit-user-select: text
}

[class~=embed] [class~=description][class~=markup] {
    font-size: .145833333in !important
}

.embed .embed-thumbnail img {
    margin-right: 0
}

[class~=image] {
    -moz-user-select: text
}

[class~=image] {
    -ms-user-select: text
}

.embed .embed-thumbnail img {
    margin-top: 0
}

[class~=embed] [class~=author-icon] {
    border-radius: 50%
}

[class~=image] {
    user-select: text
}

[class~=embed] [class~=embed-footer] {
    font-size: 12px
}

[class~=embed] {
    display: -webkit-box
}

[class~=embed] [class~=description][class~=markup] {
    line-height: 12pt !important
}

.embed .embedFields .embedField {
    -webkit-box-flex: 0
}

.embed .settle {
    margin-right: .104166667in
}

[class~=embed] {
    display: -ms-flexbox
}

[class~=embed] [class~=embedFields] [class~=embedField][class~=inline] {
    flex-basis: auto
}

.embed .settle {
    height: .1875in
}

.embed .settle {
    width: 1.125pc
}

[class~=conference] {
    display: -webkit-box
}

[class~=conference] {
    display: -ms-flexbox
}

.embed .settle {
    -o-object-fit: contain
}

.embed .settle {
    object-fit: contain
}

[class~=embed] [class~=embedFields] [class~=embedField] [class~=embedField-name],
[class~=embed] [class~=embed-title] {
    margin-bottom: 4px
}

[class~=conference] {
    margin-top: .3125pc
}

[class~=embed] [class~=embedFields] [class~=embedField] [class~=embedField-name] {
    font-weight: 600
}

[class~=conference] {
    max-width: 520px
}

.embed .embedFields .embedField {
    -ms-flex: 0
}

.embed .settle {
    border-radius: .153125pc
}

[class~=conference] {
    display: -webkit-box
}

[class~=embed] [class~=embedFields] [class~=embedField] [class~=embedField-value],
[class~=embed] [class~=embed-title],
[class~=embed] [class~=description] {
    font-size: .875pc
}

.embed .embedFields .embedField {
    flex: 0
}

.embed .embedFields .embedField {
    padding-top: .625pc
}

[class~=conference] {
    display: -ms-flexbox
}

[class~=conference],
[class~=embed] {
    display: flex
}

[class~=embed] {
    padding-left: .104166667in
}

[class~=embed] [class~=description] {
    display: block
}

[class~=embed] {
    padding-bottom: .083333333in
}

[class~=embed] [class~=embedFields] [class~=embedField] [class~=embedField-value],
[class~=embed] [class~=description] {
    font-weight: 500
}

[class~=embed] [class~=embedFields],
[class~=embed] [class~=embed-thumbnail],
[class~=embed] [class~=description] {
    margin-bottom: .104166667in
}

[class~=embed] {
    padding-right: .104166667in
}

[class~=embed] {
    padding-top: .083333333in
}

[class~=embed] [class~=description],
[class~=embed] [class~=embedFields] {
    color: #b3b3b3
}

[class~=embed] {
    box-sizing: border-box
}

[class~=embed] [class~=embedFields] {
    display: -webkit-box
}

.embed .embedFields .embedField {
    min-width: 100%
}

.embed .embedFields .embedField {
    max-width: 379.5pt
}

[class~=embed] [class~=embedFields] {
    display: -ms-flexbox
}

[class~=embed] [class~=embedFields] {
    display: flex
}

[class~=embed] {
    background: #2f3136
}

[class~=embed] {
    border-left-width: .010416667in
}

[class~=embed] {
    border-bottom-width: .010416667in
}

[class~=embed] [class~=embedFields] {
    -ms-flex-direction: row
}

[class~=embed] [class~=embedFields] {
    -webkit-box-orient: horizontal
}

[class~=embed] {
    border-right-width: .010416667in
}

[class~=embed] {
    border-top-width: .010416667in
}

[class~=embed] [class~=embedFields] {
    -webkit-box-direction: normal
}

[class~=embed] {
    border-left-style: solid
}

[class~=embed] {
    border-bottom-style: solid
}

[class~=embed] [class~=embedFields] {
    flex-direction: row
}

[class~=embed] {
    border-right-style: solid
}

[class~=embed] [class~=embedFields] {
    -ms-flex-wrap: wrap
}

[class~=embed] {
    border-top-style: solid
}

[class~=embed] {
    border-left-color: #2f3136
}

[class~=embed] {
    border-bottom-color: #2f3136
}

[class~=embed] [class~=embedFields] {
    flex-wrap: wrap
}

[class~=embed] [class~=embedFields] {
    margin-top: -.104166667in
}

[class~=embed] {
    border-right-color: #2f3136
}

[class~=Button] span,
.embed .embed-thumbnail {
    position: relative
}

[class~=embed] {
    border-top-color: #2f3136
}

.embed .embed-thumbnail {
    display: inline-block
}

[class~=embed] {
    border-image: none
}

[class~=embed] {
    border-radius: 0 3px 2.25pt 0
}

[class~=embed] {
    flex-direction: column
}

[class~=Button] span {
    right: -.625pc
}

[class~=embed] [class~=description][class~=markup] pre {
    max-width: 100% !important
}
</style>
