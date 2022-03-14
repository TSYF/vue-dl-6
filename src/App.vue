<template>
    <header>
        <h1>Random Gif Cat</h1>
    </header>
    <form @submit.prevent="fetchCat">
        <fieldset>
            <label for="titulo"
                >Titulo:
                <input type="text" name="titulo" id="titulo" v-model="title" />
            </label>
            <br />
            <label for="filtro"
                >Filtro:
                <select name="filtro" id="filtro" v-model="filter">
                    <option
                        :value="filter"
                        v-for="(filter, i) in filters"
                        :key="filter + i"
                    >
                        {{ filter }}
                    </option>
                </select>
            </label>
            <br />
            <label for="color"
                >Color:
                <select name="color" id="color" v-model="color">
                    <option value="red">Rojo</option>
                    <option value="blue">Azul</option>
                    <option value="green">Verde</option>
                    <option value="yellow">Amarillo</option>
                    <option value="white">Blanco</option>
                </select>
                <div class="circolor" :style="'--colorVar: ' + color"></div>
            </label>
            <br />
            <div v-if="!/^\d+$|^$/.test(size)">
                <strong style="color: red">Solo números!</strong>
                <br />
            </div>
            <label for="tamaño"
                >Tamaño:
                <input type="text" name="tamaño" id="tamaño" v-model="size" />
            </label>
        </fieldset>
        <button type="submit">Obtener mi gatito</button>
    </form>
    <section class="cat-img-section">
        <img v-if="catUrl" :src="catUrl" alt="cat" />
    </section>
</template>

<script>
// https://cataas.com/cat/gif/says/hello?size=50&color=red

export default {
    data() {
        return {
            title: "",
            filters: "Blur, Mono, Sepia, Negative, Paint, Pixel".split(","),
            filter: "",
            color: undefined,
            size: "",
            catUrl: "",
        };
    },
    methods: {
        async fetchCat() {
            try {
                const cat = await fetch(
                    `https://cataas.com/cat/gif/says/${this.title.trim()}?size=${this.size.trim()}&color=${this.color.trim()}&fi=${this.filter
                        .toLowerCase()
                        .trim()}`
                );

                if (!cat.ok) return console.error("Fuck");

                const res = await cat.blob();
                this.catUrl = URL.createObjectURL(res);
            } catch (err) {
                console.error(err);
            }
        },
    },
};
</script>

<style lang="scss">
body {
    background-color: skyblue;
    margin: 0;
    padding: 0;
    line-height: 1.8;
}

header {
    text-align: center;
}

form {
    text-align: center;
    fieldset {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background-color: darksalmon;
        width: 100%;
        border: none;
        padding: 2em;
        margin-bottom: 1rem;
    }

    input {
        &::-webkit-outer-spin-button,
        &::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        &[type="number"] {
            -moz-appearance: textfield;
        }
    }

    br {
        margin-bottom: 0.5rem;
    }

    [name="color"] {
        vertical-align: middle;
    }

    .circolor {
        border-radius: 50%;
        width: 2em;
        height: 2em;
        background-color: var(--colorVar);
        display: inline-block;
        vertical-align: middle;
        margin-left: 1rem;
    }
}

.cat-img-section {
    text-align: center;
    padding-top: 4rem;
}
</style>
