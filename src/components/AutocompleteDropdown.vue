<template>
    <div class="dropdown">
        <input type="text"
            v-model="searchText"
            class="input"
            :placeholder="placeholder"

            @input="searchChanged"

            @touchstart="gotFocus"
            @focus="gotFocus"
            @blur="lostFocus"

            @keydown.down="down"
            @keydown.tab.prevent="down"
            @keydown.up="up"

            @keydown.enter="suggestionSelected(matches[highlightIndex])">
        <ul class="suggestion-list" :class="{'open' : open}">
            <li class="suggestion"
                v-for="(suggestion, index) in matches"
                v-bind:key="index"
                :class="{'active' : index === highlightIndex}"
                @mousedown.prevent
                @click="suggestionSelected (suggestion)"
                @touchstart="suggestionSelected (suggestion)">
            {{ suggestion }}
            </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>
.dropdown {
    display: inline-block;
    position: relative;
}

.input {
    border: 2px solid #A3057F;
    padding: 10px;

    text-align: inherit;
    font-size: 1.1em;
    font-weight: bold;
    color: #A3057F;
}

.suggestion-list {
    box-sizing: border-box;
    display: block;

    width: 100%;
    top: 100%;
    left: 0;
    z-index: 2;

    background-color: rgba(255, 255, 255, 0.95);

    border: 1px solid #ddd;
    margin: 0;
    padding: 0;

    overflow: hidden;
    position: absolute;

    list-style: none;

    display: none;

    &.open {
        display: block;
    }
}

.suggestion {
    cursor: pointer;
    padding: 8px;

    font-size: 1em;

    &.active  {
        background-color: #A3057F;
        color: #fff;
    }

    &:hover {
        color: #fff;
        background-color: #ccc;
    }
}

</style>

<script>
export default {
    name: 'AutocompleteDropdown',
    data () {
        return {
            searchText: '',
            selectedOption: null,
            open: false,
            highlightIndex: 0
        }
    },

    props: [
        'options',
        'placeholder'
    ],

    methods: {
        searchChanged () {
            // if (this.searchText) {
            this.open = true
            // }
            this.highlightIndex = 0
        },

        gotFocus () {
            // if (this.searchText) {
            this.open = true
            // }
        },

        lostFocus () {
            this.open = false
        },

        suggestionSelected (suggestion) {
            this.searchText = suggestion
            this.$emit('input', suggestion)
            this.highlightIndex = 0
            this.open = false
        },

        up () {
            if (this.open) {
                if (this.highlightIndex > 0) {
                    this.highlightIndex--
                }
            } else {
                this.open = true
            }
        },

        down () {
            if (this.open) {
                if (this.highlightIndex < this.matches.length - 1) {
                    this.highlightIndex++
                }
            } else {
                this.open = true
            }
        }
    },

    computed: {
        matches () {
            return this.options.filter(option => {
                const optionText = option.toUpperCase()
                return optionText.match(this.searchText.toUpperCase())
            })
        }
    }
}
</script>
