<template>
    <div class="card" style="width: 100%;">
        <div class="card-body">
            <div class="row">
                <div class="col">
                    <div v-if="cover" class="ratio ratio-1x1" :style="image"></div>
                    <div v-else class="ratio ratio-1x1"
                        style="background-image: url('https://www.vozdelacapilla.com/wp-content/uploads/2020/06/icon-150x150.png'); background-repeat: no-repeat; background-size: cover;">
                    </div>
                </div>
                <div class="col-11">
                    <h5 class="card-title">{{ name }}</h5>
                    <h6 class="card-subtitle mb-2 text-muted">{{ state }}</h6>
                    <div v-if="hasTags">
                        <span v-for="tag in tagsArray" class="badge bg-light text-dark" v-bind:key="tag">{{tag}}</span>
                    </div>
                    <div class="btn btn-success" @click="playAudio()">Reproducir</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Card',
    data() {
        return {
            tagsArray: [],
            hasTags: []
        }
    },
    props: {
        name: String,
        state: String,
        url: String,
        cover: String,
        tags: String,
    },
    mounted() {
        this.separateTags();
    },
    computed: {
        image() {
            return `background-image: url('${this.cover}'); background-repeat: no-repeat; background-size: cover; background-position: center;`
        }
    },
    methods: {
        separateTags() {
            if (this.tags.split(",").length > 0) {
                this.tagsArray = this.tags.split(",");
                this.hasTags = true;
            }
        },
        playAudio() {
            const station = { name: this.name, state: this.state, url: this.url, cover: this.cover }
            this.$emit('changeStation', JSON.stringify(station))
        }
    }
}
</script>
