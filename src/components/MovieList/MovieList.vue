<template>
    <div id="movieList">
        <div id="movieFilters">
            <h1>Filter the results</h1>
            <MovieListFilter
                :options="genres"
                list="genre"
                label="by genre"
                @change="filters['Genre'] = $event"
            />
            <MovieListFilter
                :options="countries"
                list="country"
                label="by country"
                @change="filters['Country'] = $event"
            />
            <MovieListFilter
                :options="writers"
                list="writer"
                label="by writer"
                @change="filters['Writer'] = $event"
            />
        </div>

        <MovieCard
            v-for="movie in filteredMovies"
            :key="movie.Title"
            :movie="movie"
        />
    </div>
</template>

<script>
import MovieCard from "./MovieCard";
import MovieListFilter from "./MovieListFilter";

export default {
    name: "MovieList",
    components: {
        MovieCard,
        MovieListFilter,
    },
    props: {
        movies: {},
    },
    data: function() {
        return {
            filters: {
                Genre: null,
                Writer: null,
                Country: null,
            },
        };
    },
    methods: {
        extractList(name, transform = null) {
            const list = [];

            this.movies.forEach((movie) =>
                movie[name].split(",").forEach((entry) => {
                    const entryTrimmed = (transform ? transform(entry) : entry).trim();

                    if (!list.includes(entryTrimmed)) {
                        list.push(entryTrimmed);
                    }
                })
            );

            return list;
        },
    },
    computed: {
        filteredMovies: function() {
            return this.movies.filter((movie) =>
                Object.keys(this.filters).every((filter) => {
                    const filterValue = this.filters[filter];

                    return (!filterValue || (filterValue && movie[filter]
                                .toLowerCase().includes(filterValue.toLowerCase()))
                    );
                })
            );
        },

        genres: function() {
            return this.extractList("Genre").sort();
        },

        countries: function() {
            return this.extractList("Country").sort();
        },

        writers: function() {
            return this.extractList("Writer", (entry) =>
                entry.replace(/ *\([^)]*\) */g, "")
            ).sort();
        },
    },
};
</script>

<style scoped lang="less">
#movieList {
    width: 90%;
    margin-left: auto;
    margin-right: auto;
}

#movieFilters {
    text-align: center;
    padding: 40px 0 40px 0;

    h1 {
        margin-bottom: 10px;
    }
}
</style>
