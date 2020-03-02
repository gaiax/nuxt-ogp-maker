<template>
    <div class="container">
        <div>
            <logo />
            <h1 class="title">nuxt-ogp-maker</h1>
            <h2 class="subtitle">My astounding Nuxt.js project</h2>
            <div class="links">
                <a href="https://nuxtjs.org/" target="_blank" class="button--green">Documentation</a>
                <a
                    href="https://github.com/nuxt/nuxt.js"
                    target="_blank"
                    class="button--grey"
                >GitHub</a>
            </div>
            <div class="hello">
                <input v-model="msg" type="text" />
                <button @click="create">create</button>
                <p></p>
                <svg ref="svgCard">
                    <rect x="0" y="0" width="1200" height="30" fill="#f2f6f7" />
                    <rect x="0" y="300" width="1200" height="330" fill="#f2c502" />
                    <text
                        transform="translate(103.29 347.281)"
                        fill="#f47603"
                        font-size="29"
                        font-family="noto Sans"
                        letter-spacing="-0.002em"
                    >
                        <tspan x="0" y="26">{{ msg }}</tspan>
                    </text>
                </svg>
                <canvas id="cv1" width="1200" height="630"></canvas>
            </div>
        </div>
    </div>
</template>

<script>
import Logo from "~/components/Logo.vue";
import firebase from "firebase";

export default {
    data() {
        return {
            msg: "cheerfor Rickey田中陸也|...さん",
            uuid: "aa" // 適当に採番する
        };
    },
    components: {
        Logo
    },
    methods: {
        create: function() {
            // refでsvgCardをsvgに設定しているのでthis.$refs.svgCardで要素を取れます
            this.svg2imageData(this.$refs.svgCard, data => {
                const sRef = firebase.storage().ref();
                const fileRef = sRef.child(`${this.uuid}.png`);
                // Cloud Storageにアップロード
                fileRef
                    .putString(data, "data_url")
                    .then(snapshot => {
                        console.log(snapshot);
                    })
                    .then(docRef => {
                        console.log(docRef);
                    })
                    .catch(err => {
                        console.error(err);
                    });
            });
        },
        svg2imageData: function(svgElement, successCallback, errorCallback) {
            var canvas = document.createElement("canvas");
            canvas.width = 1200;
            canvas.height = 630;
            var ctx = canvas.getContext("2d");

            var image = new Image();
            var basePict = new Image();
            basePict.src = "1200x630.png";
            console.log(basePict);
            image.onload = () => {
                ctx.drawImage(basePict, 0, 0, 1199, 629);
                ctx.fillStyle = "rgb(0, 0, 255)";
                ctx.fillRect(40, 0, 40, 240);
                ctx.fillRect(240, 0, 40, 240);
                ctx.fillStyle = "rgb(128, 255, 255)";
                ctx.font = "48pt sans-serif";
                ctx.fillText("Hello World", 400, 200);
                successCallback(canvas.toDataURL());
            };
            image.onerror = e => {
                errorCallback(e);
            };
            image.src = canvas.toDataURL();
        }
    },
    mounted() {
        if (!firebase.apps.length) {
            var firebaseConfig = {
                apiKey: "AIzaSyBqJdAMDiORN77buT_dE008YBuDLIznKBg",
                authDomain: "nuxt-ogp-maker-5c180.firebaseapp.com",
                databaseURL: "https://nuxt-ogp-maker-5c180.firebaseio.com",
                projectId: "nuxt-ogp-maker-5c180",
                storageBucket: "nuxt-ogp-maker-5c180.appspot.com",
                messagingSenderId: "1082531243836",
                appId: "1:1082531243836:web:834a020ba998178d94e039"
            };
            firebase.initializeApp(firebaseConfig);
        }
    }
};
</script>

<style>
.container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.title {
    font-family: "Quicksand", "Source Sans Pro", -apple-system,
        BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial,
        sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
}

.subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
}

.links {
    padding-top: 15px;
}
</style>
