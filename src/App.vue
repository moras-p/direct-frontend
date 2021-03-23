<template>
    <div id="app">
        <MovieList :movies="Movies"></MovieList>

        <div id="bottom">
            <div id="footer">
                <p>Images provided with the courtesy of The Movie Database</p>
                <a href="https://www.themoviedb.org/">
                    <img alt="TMDB Logo" src="./assets/tmdb_blue_long.svg" />
                </a>
            </div>
        </div>
    </div>
</template>

<script>
import MovieList from "@/components/MovieList/MovieList";
import { Movies } from "@/assets/movies.json";

export default {
    name: "App",
    components: {
        MovieList,
    },
    data: function() {
        return {
            Movies,
        };
    },
};
</script>

<style>
@import url("http://fonts.googleapis.com/css?family=Montserrat:300,400,700,800");
</style>

<style lang="less">
:root {
    --font-color: #555;
    --bg-color: white;
    --border-color: #eee;
    --placeholder-color: gray;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

ol,
ul {
    list-style: none;
}

body {
    padding: 10px;
    line-height: 1;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;

    background-color: var(--bg-color);
    color: var(--font-color);

    &, input {
        font-family: "Montserrat", Sans, Helvetica, Arial, sans-serif;
    }
}

#app {
    height: 100%;
}

#footer {
    margin-left: auto;
    margin-right: auto;
    width: 100%;
    text-align: center;
    padding: 40px;

    img {
        max-width: 800px;
    }
}
</style>
