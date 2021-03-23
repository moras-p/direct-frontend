<template>
    <div class="movie-card" v-show="!isDataLoading">
        <div class="info to-top">
            <div class="header">
                <span class="poster">
                    <img :src="posterUrl" />
                </span>
                <span class="overview">
                    <span class="title">{{ movie.Title }}</span>
                    <span class="year">{{ movie.Year }}, {{ movie.Director }}</span>
                    <span class="rated">{{ movie.Rated }}</span>
                    <span class="runtime">{{ movie.Runtime }}</span>
                    <span class="genres">{{ movie.Genre }}</span>
                </span>
            </div>
            <div class="plot">
                {{ movie.Plot }}
            </div>
            <div class="details">
                <div>Starring {{ movie.Actors }}</div>
                <div>Written by {{ movie.Writer }}</div>
                <div>Filmed in {{ movie.Country }}</div>
                <div>{{ movie.Awards }}</div>
                <div>Imdb score of {{ movie.imdbRating }} based on{{ movie.imdbVotes }} votes</div>
            </div>
        </div>
        <div class="bg to-top" :style="cardBackground"></div>
    </div>
</template>

<script>
export default {
    name: "MovieCard",
    props: {
        movie: {
            required: true,
        },
    },
    data: function() {
        return {
            isDataLoading: true,
            posterUrl: null,
            coverUrl: null,
        };
    },
    created: function() {
        this.loadImages(this.movie);
    },
    methods: {
        loadImages(newMovie) {
            this.isDataLoading = true;
            fetch(`https://api.themoviedb.org/3/search/movie?api_key=15d2ea6d0dc1d476efbca3eba2b9bbfb&query=${newMovie.Title}`)
                .then((response) => response.json())
                .then((movieData) => {
                    const tmdbImage = (file) => `http://image.tmdb.org/t/p/w500${file}`;
                    this.posterUrl = tmdbImage(movieData.results[0].poster_path);
                    this.coverUrl = tmdbImage(movieData.results[0].backdrop_path);
                    this.isDataLoading = false;
                });
        },
    },
    computed: {
        cardBackground() {
            return {
                "background-image": `url('${this.coverUrl}')`,
            };
        },
    },
};
</script>

<style lang="less" scoped>
.center {
    margin-left: auto;
    margin-right: auto;
}

.movie-card {
    .center;
    &:hover {
        height: unset;
        transform: scale(1.05);
        box-shadow: 0px 0px 20px -25px rgba(0, 0, 0, 0.5);
    }

    box-shadow: 0px 0px 40px -25px rgba(0, 0, 0, 0.5);
    z-index: 3;
    border: 1px solid var(--border-color);
    border-radius: 10px;
    background-color: var(--border-color);
    height: 310px;
    max-width: 800px;
    position: relative;
    transition: all 0.4s;
    overflow: hidden;
    margin-bottom: 40px;

    .info {
        padding: 20px;
        padding-top: 60px;
        width: 100%;
        height: 100%;
        background-color: --var(placeholder-color);
        border-radius: 10px;
        z-index: 2;
        position: relative;

        &.to-top {
            background: linear-gradient(to top, #eee 40%, transparent 150%);
        }

        .header {
            margin-bottom: 20px;

            .title {
                font-size: 2rem;
                font-weight: 800;

                display: inline-block;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
            }

            .year {
                display: block;
            }

            .rated {
                margin-right: 10px;
            }

            .runtime {
                display: inline-block;
                margin-right: 4px;
                margin-top: 20px;
                padding: 4px;
                outline: 1px solid var(--border-color);
            }

            .poster {
                vertical-align: top;
                img {
                    height: 120px;
                    margin-right: 20px;
                    box-shadow: 4px 4px 20px -10px rgba(0, 0, 0, 0.5);
                }
            }

            .overview {
                width: 60%;
                display: inline-block;
            }
        }
    }

    .details {
        font-size: 0.8rem;
        line-height: 1.1rem;
        transition: all 0.4s;

        opacity: 0;
        height: 0px;
    }

    &:hover .details {
        opacity: 1;
        height: initial;
        margin-bottom: 10px;
    }

    .plot {
        margin-bottom: 20px;
    }

    .bg {
        position: absolute;
        background-color: var(--placeholder-color);
        border-radius: 10px;
        top: 0px;
        right: 0px;
        width: 70%;
        height: 300px;
        z-index: 1;

        &.to-top {
            background-position: 50% 50% !important;
            background-size: cover !important;
            width: 100%;
        }
    }
}

@media screen and (max-width: 500px) {
    .poster {
        display: none;
    }
}
</style>
