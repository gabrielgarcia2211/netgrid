<style>
.card-img-top {
    border: solid 0.3em rgb(127, 178, 255);
}
</style>

<template>
    <div class="container" style="width: 100%; padding: 20px;max-height: 500px; overflow-y: scroll;">
        <div class="card border-0">
            <h2 class="card-title">
                {{ this.pokemonDetail.name }}
                <p style="color: blue; display: inline">
                    EXP - {{ this.pokemonDetail.base_experience }}
                    <button
                        class="btn btn-info"
                        v-on:click="addFavorite"
                        :style="{
                            color: this.favorite,
                            display: 'inline-block',
                            display: 'flex',
                            float: 'right',
                        }"
                    >
                        <i
                            class="fa fa-star fa-3x"
                            aria-hidden="true"
                            title="Favoritos"
                        ></i>
                    </button>
                </p>
            </h2>
            <div class="row">
                <div class="col-6">
                    <div class="card">
                        <img
                            class="card-img-top"
                            style="width: 100%"
                            :src="this.pokemonDetail.sprites.front"
                            alt="Card image cap"
                        />
                    </div>
                </div>
                <div class="col-6">
                    <div class="card">
                        <img
                            class="card-img-top"
                            style="width: 100%"
                            :src="this.pokemonDetail.sprites.back"
                            alt="Card image cap"
                        />
                    </div>
                </div>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-3">
                        <strong>Movimientos:</strong>
                        <ul v-if="this.pokemonDetail.moves">
                            <li
                                v-for="item in this.pokemonDetail.moves"
                                v-bind:key="item.id"
                            >
                                {{ item }}
                            </li>
                        </ul>
                        <p v-else>N/A</p>
                    </div>
                    <div class="col-3">
                        <strong>Habilidades:</strong>
                        <ul>
                            <li
                                v-for="item in this.pokemonDetail.abilities"
                                v-bind:key="item.id"
                            >
                                {{ item }}
                            </li>
                        </ul>
                    </div>
                    <div class="col-3">
                        <strong>Retenidos:</strong>
                        <ul v-if="this.pokemonDetail.held_items">
                            <li
                                v-for="item in this.pokemonDetail.held_items"
                                v-bind:key="item.id"
                            >
                                {{ item }}
                            </li>
                        </ul>
                        <p v-else>N/A</p>
                    </div>
                    <div class="col-3">
                        <strong>Tipo:</strong>
                        <ul v-if="this.pokemonDetail.types">
                            <li
                                v-for="item in this.pokemonDetail.types"
                                v-bind:key="item.id"
                            >
                                {{ item }}
                            </li>
                        </ul>
                        <p v-else>N/A</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ["modalName", "pokemonDetail"],
    mounted() {
        this.getPokemon();
    },
    data() {
        return {
            favorite: "white",
        };
    },
    components: {},
    beforeMount() {},
    methods: {
        addFavorite() {
            axios
                .post(`pokemons/favorites`, this.pokemonDetail)
                .then((res) => {
                    if (res.data.new) {
                        this.$alertSuccess(
                            "Proceso completo",
                            "Se añadio el pokemon a favoritos"
                        );
                        this.favorite = "yellow";
                    } else {
                        this.$alertSuccess(
                            "Proceso completo",
                            "Se elimino de favoritos"
                        );
                        this.favorite = "white";
                    }
                })
                .catch((error) => {
                    this.$readStatusHttp(error);
                });
        },
        getPokemon() {
            axios
                .get(`pokemons/get/${this.pokemonDetail.name}`)
                .then((res) => {
                    if (res.data) {
                        this.favorite = "yellow";
                    }
                })
                .catch((error) => {
                    this.$readStatusHttp(error);
                });
        },
        hide() {
            this.$modal.hide(this.modalName);
        },
    },
};
</script>
