<script setup>

import { RouterLink} from 'vue-router'
import { ref } from 'vue'
import { QCard, QCardSection, QAvatar, QIcon, QInput, QSelect, QBtn, copyToClipboard } from 'quasar'
import Tr from '@/i18n/translation'
import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';

const hostOptions = [
    'null (all)',
    'rrc00.ripe.net',
    'rrc01.ripe.net',
    'rrc02.ripe.net',
    'rrc03.ripe.net',
    'rrc04.ripe.net',
    'rrc05.ripe.net',
    'rrc06.ripe.net',
    'rrc07.ripe.net',
    'rrc08.ripe.net',
    'rrc09.ripe.net',
    'rrc10.ripe.net',
    'rrc11.ripe.net',
    'rrc12.ripe.net',
    'rrc13.ripe.net',
    'rrc14.ripe.net',
    'rrc15.ripe.net',
    'rrc16.ripe.net',
    'rrc17.ripe.net',
    'rrc18.ripe.net',
    'rrc19.ripe.net',
    'rrc20.ripe.net',
    'rrc21.ripe.net',
    'rrc22.ripe.net',
    'rrc23.ripe.net',
    'rrc24.ripe.net',
    'rrc25.ripe.net',
    'rrc26.ripe.net',
    
]

const host = ref('null (all)')
const peer = ref()
const prefix = ref()
const path = ref()
let message = ref()

const websocketUrl = "wss://ris-live.ripe.net/v1/ws/?client=gsoc-ihr"

const socket = new WebSocket(websocketUrl);

const openSocket = () => {
    console.log(path.value)
    socket.send(JSON.stringify({type: "ris_subscribe", data: { path: path.value}}));
}

socket.onmessage = (event) => {
    message.value = JSON.parse(event.data)
};

</script>

<template>


    <div class="row wrap q-gutter-md IHR_description-main ">
      
        <QCard class="filter-module">

            <p class='title'>RIS Live Filter</p>
        
            <div class="col-3 input">
               
                <QInput  v-model="prefix" label="Prefix" />
                <QInput  v-model="path" label="Path" />
                <QInput  v-model="peer" label="Peer" />
                <QSelect v-model="host"  :options="hostOptions" label="Host" />
                <QBtn @click=openSocket() class="sub-button" color="secondary" label="Subscribe" />

            </div>

        </QCard>

        <QCard class="live-module">

            <p class='title'> Live Feed </p>
            <div class="live_feed">
                <vue-json-pretty :showDoubleQuotes=false :showLine=false :virtual=true :data="message" />
            </div>

        </QCard>

    </div>

</template>

<style lang="stylus">
.IHR_
  &description,
    &-main
      font-size 20pt
      margin 30pt auto !important
      text-align center
      width 85%
      @media screen and (max-width: 600px)
        font-size 14pt

    &-link
      position relative
      text-align right


.filter-module
    min-width 400px
    margin-right 60px
    text-align center
    border-radius 15px !important

.live-module
    min-width 700px
    max-width 700px
    text-align center
    border-radius 15px !important

.input 
    max-width 250px
    margin auto
    margin-top 20px
    margin-bottom 20px

.title 
    font-size 30px
    font-weight bold
    margin-top 20px

.sub-button
    margin-top 40px

.live_feed 
    margin-left 20px
    margin-bottom 20px
    
@media(max-width 1411px)
  .analysis-modules
    margin-left 0


</style>