<template>
    <div class="dropdown" :class="{'open' : open}">
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
        <ul class="suggestion-list" >
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
.active  {
  color: #fff;
  background-color: #A3057F;
}

.dropdown {
    display: inline-block;
    position: relative;
}

.suggestion-list {
    box-sizing: border-box;
    background-color: rgba(255, 255, 255, 0.95);
    border: 1px solid #ddd;
    list-style: none;
    display: block;
    margin: 0;
    padding: 0;
    width: 100%;
    overflow: hidden;
    position: absolute;
    top: 100%;
    left: 0;
    z-index: 2;
}

.suggestion-list li {
  cursor: pointer;
  padding: 10px;
}

.suggestion-list li:hover {
  color: #fff;
  background-color: #ccc;
}

.dropdown.open {
    .suggestion-list {
        display: block;
    }
}

.dropdown {
    .suggestion-list {
        display: none;
    }
}

.input {
    text-align: inherit;
    font-size: inherit;
    font-weight: bold;

    color: #A3057F;
    border: 1px solid #A3057F;

    padding: 10px;
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
