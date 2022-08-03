<template>
    <nav class="navbar navbar-dark bg-dark fixed-bottom">
        <div class="container-fluid">
            <div class="row" style="display: contents;">
                <div class="col">
                    <div v-if="cover" class="ratio ratio-1x1" :style="image"></div>
                    <div v-else class="ratio ratio-1x1"
                        style="background-image: url('https://www.vozdelacapilla.com/wp-content/uploads/2020/06/icon-150x150.png'); background-repeat: no-repeat; background-size: cover;">
                    </div>
                </div>
                <div class="col-11">
                    <h5 style="color: white;">Estás escuchando: {{ name }}</h5>
                    <h6 v-if="state" class="text-muted bg-dark">{{ state }}</h6>
                    <audio :src="url" @change="togglePlay()" id="stream" autoplay></audio>
                    <div style="-webkit-user-select: none; ">
                        <span v-if="isPlaying" style="color: white; cursor: pointer;" @click="togglePlay()"
                            class="material-symbols-outlined">
                            pause
                        </span>
                        <span v-else style="cursor: pointer; color: white;" @click="togglePlay()"
                            class="material-symbols-outlined">
                            play_arrow
                        </span>
                        <p style="text-align: right; margin: 0px;">
                            <span class="material-symbols-outlined" style="color: white;">
                                {{volLevel}}
                            </span>
                            <input type="range" min="1" max="100" value="100" @change="setVolume()" class="slider"
                                id="volume">
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>
<script>
export default {
    name: 'Player',
    data() {
        return {
            isPlaying: false,
            volLevel: "volume_up"
        }
    },
    props: {
        name: String,
        state: String,
        url: String,
        cover: String
    },
    computed: {
        image() {
            return `background-image: url('${this.cover}'); background-repeat: no-repeat; background-size: cover; background-position: center;`
        }
    },
    mounted() {
        this.getPlayerState();
    },
    methods: {
        getPlayerState() {
            let myAudio = document.getElementById('stream');
            if (myAudio.duration > 0 && !myAudio.paused) {
                this.isPlaying = true;
            }
        },
        togglePlay() {
            let myAudio = document.getElementById('stream');
            if (this.isPlaying) {
                myAudio.pause();
                this.isPlaying = false;
            } else {
                myAudio.play();
                this.isPlaying = true;
            }
        },
        setVolume() {
            let myAudio = document.getElementById('stream');
            let range = document.getElementById('volume');
            if(parseInt(range.value)< 5) {
                myAudio.volume = Math.round(parseInt(range.value) / 100);
                this.volLevel = "volume_off"
            } else {
                myAudio.volume = parseInt(range.value) / 100;
                this.volLevel = "volume_up"
            }
        }
    },
}
</script>