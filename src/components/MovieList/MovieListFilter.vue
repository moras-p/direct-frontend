<template>
    <div class="movieListFilter">
        <label :for="list">
            <span>{{ label }}</span>
            <input :list="list" :name="list" v-model="filterValue" />
            <datalist :id="list">
                <option
                    v-for="option in options"
                    :key="option"
                    :value="option"
                />
            </datalist>
        </label>
    </div>
</template>

<script>
export default {
    props: {
        options: {},
        list: {},
        label: {},
    },
    data: function() {
        return {
            filterValue: "",
        };
    },
    watch: {
        filterValue: function() {
            this.$emit("change", this.filterValue);
        },
    },
};
</script>

<style lang="less" scoped>
.movieListFilter {
    label span {
        display: inline-block;
        width: 100px;
        overflow: hidden;
        text-align: right;
        padding-right: 10px;
    }
    input {
        outline: none;
        border: none;
        border-bottom: 2px solid var(--border-color);
    }
}
</style>
