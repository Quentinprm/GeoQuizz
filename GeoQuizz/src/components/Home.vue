<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <img id="logo" src="../assets/map.png">
    <div v-if="Pseudo==''">
      <form @submit.prevent="log()" class="form-sign">
        <label>Votre pseudo pour cette session de jeu : </label>
        <input ref="pseudo" id="pseudo" />
        <br>
        <br>
        <input type="submit"class="btn btn-5" value="Jouer"></input>
      </form>
    </div>
    <div v-else>
    <div v-for></div>
      <h2>Cliquez sur une série pour lancer la partie</h2>
      <label>Difficulté : </label>
      {{Difficulty}}
      <select ref="select">
        <option v-if="Difficulty==1" value=1 selected="selected">Facile</option>
        <option v-else value=1>Facile</option>
        <option v-if="Difficulty==2" value=2 selected="selected">Intermédiaire</option>
        <option v-else value=2>Intermédiaire</option>
        <option v-if="Difficulty==3" value=3 selected="selected">Difficile</option>
        <option v-else value=3>Difficile</option>
      </select>
      <div v-for="serie in OSeries.series">
        <a v-on:click="createPartie(serie.id)"><h1>{{serie.ville}}</h1></a>
      </div>
    </div>
  </div>
</template>

<script>
    import router from '../router'
    import api from '@/api'
    import Vuex from 'vuex'
    import {
        mapActions
    } from 'vuex'
    import {
        mapState
    } from 'vuex'

    import L from 'leaflet'

    delete L.Icon.Default.prototype._getIconUrl;

    L.Icon.Default.mergeOptions({
        iconUrl: require('leaflet/dist/images/marker-icon.png')
    })
    export default {
        computed: {
            ...mapState(['Pseudo', 'OSeries', 'Difficulty'])
        },

        name: 'Home',
        data() {
            return {
                msg: 'GeoQuizz',
            }
        },
        methods: {

            log() {
                this.$store.dispatch('logStore', this.$refs.pseudo.value);
            },

            createPartie(idserie) {
                this.$store.dispatch('setDifficulty', this.$refs.select.value),
                this.$store.dispatch('createPartieStore', idserie),
                this.$router.push({
                    name: "Play"
                })
            }
        }
    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    html,
    body {
        height: 100%;
    }

    body {
        background: #efefef;
        color: #121221;
        font: 700 14px Montserrat, sans-serif;
        letter-spacing: 0.125em;
        text-align: center;
        text-transform: uppercase;
    }

    h1 {
        margin: 3em 0 2em;
    }

    .btn {
        background: none;
        border: 2px solid;
        border-bottom-width: 4px;
        font: inherit;
        letter-spacing: inherit;
        margin: 1em;
        padding: 1em 2em;
        text-transform: inherit;
        transition: color 1s;
    }

    .btn-1 {
        color: #9c89f7;
    }

    .btn-1:hover {
        animation: halftone 1s forwards;
        background: radial-gradient(circle, #9c89f7 0.2em, transparent 0.25em) 0 0 / 1.25em 1.25em, radial-gradient(circle, #9c89f7 0.2em, transparent 0.25em) 6.25em 6.25em / 1.25em 1.25em;
        color: #e4f789;
    }

    @keyframes halftone {
        100% {
            background-size: 2.375em 2.375em, 0.1em 0.1em;
        }
    }

    .btn-2 {
        color: #82f6d8;
    }

    .btn-2:hover {
        animation: stripes-move 0.75s infinite linear;
        background: repeating-linear-gradient(45deg, #82f6d8 0, #82f6d8 0.25em, transparent 0.25em, transparent 0.5em);
        color: #f682a0;
    }

    @keyframes stripes-move {
        100% {
            background-position: 5em 0px;
        }
    }

    .btn-3 {
        color: #d3f169;
    }

    .btn-3:hover {
        animation: sawtooth 0.35s infinite linear;
        background: linear-gradient(45deg, #d3f169 0.5em, transparent 0.5em) 0 0 / 1em 1em, linear-gradient(-45deg, #d3f169 0.5em, transparent 0.5em) 0 0 / 1em 1em;
        color: #8769f1;
    }

    @keyframes sawtooth {
        100% {
            background-position: 1em 0;
        }
    }

    .btn-4 {
        color: #eea163;
    }

    .btn-4:hover {
        animation: zigzag 1s linear infinite;
        background: linear-gradient(135deg, rgba(238, 161, 99, 0.25) 0.25em, transparent 0.25em) -0.5em 0, linear-gradient(225deg, rgba(238, 161, 99, 0.25) 0.25em, transparent 0.25em) -0.5em 0, linear-gradient(315deg, rgba(238, 161, 99, 0.25) 0.25em, transparent 0.25em) 0 0, linear-gradient(45deg, rgba(238, 161, 99, 0.25) 0.25em, transparent 0.25em) 0 0;
        background-size: 0.75em 0.75em;
        color: #63b0ee;
    }

    @keyframes zigzag {
        100% {
            background-position: 1em 0, 1em 0, -0.75em 0, -0.75em 0;
        }
    }

    .btn-5 {
        color: #7cf07f;
    }

    .btn-5:hover {
        animation: blinds 0.75s linear forwards;
        background: linear-gradient(0deg, #7cf07f 25%, transparent 25%) 0 0 / 0.5em 0.5em, linear-gradient(0deg, #88d6f3 50%, transparent 50%) 0 0 / 1em 1em;
        color: #f07ced;
    }

    @keyframes blinds {
        100% {
            background-position: 0 0, 0 -3em;
            background-size: 0 0, 1em 6em;
        }
    }

    .btn-6 {
        color: #f9879b;
    }

    .btn-rep {
        width: 50%;
    }

    #rep {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        margin-top: 200px;
        width: 100%;
    }

    .btn-6:hover {
        animation: pulse 1s ease-in infinite;
        background: radial-gradient(circle, rgba(249, 135, 155, 0.25) 43%, transparent 50%) 0 0 / 1em 1em, radial-gradient(circle, rgba(249, 135, 155, 0.25) 43%, transparent 50%) 0.5em 0.5em / 2em 2em;
        color: #0bdcb7;
    }

    @keyframes pulse {
        50% {
            background-position: 0.66em 0.66em, -0.33em -0.33em;
        }
        100% {
            background-size: 2em 2em, 1em 1em;
            background-position: -1.5em -1.5em, -1em -1em;
        }
    }

</style>
