<template>
    <div
        v-on-clickaway="hideDropdown"
        class="v-select">
        <div
            @click="show = !show"
            class="v-select-toggle">
            <div>
                {{ selectedValue ? getOptionLabel(selectedValue) : labelEmpty }}
            </div>
            <div class="arrow-down"></div>
        </div>
        <div
            v-show="show"
            class="v-dropdown-container">
            <ul>
                <li
                    v-if="search"
                    class="search-container">
                    <input
                        placeholder="Search"
                        class="form-control"
                        type="text"
                        v-model="searchValue"
                        autofocus>
                </li>
                <li
                    v-show="search && filteredProps.length === 0"
                    class="v-dropdown-item">
                    {{ labelNotFound }}: "{{ searchValue }}"
                </li>
                <li
                    v-if="showDefaultOption"
                    class="v-dropdown-item disabled default-option">
                    {{ labelEmpty }}
                </li>
                <li
                    v-for="(option, index) in filteredProps"
                    :key="`v-select-${index}`"
                    class="v-dropdown-item"
                    :class="{'selected' : selectedValue && option === selectedValue}"
                    @click="handleSelect(option)">
                    {{ getOptionLabel(option) }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { mixin as clickaway } from "vue-clickaway";

export default {
  name: "VSelect",
  mixins: [clickaway],
  props: {
    labelEmpty: {
      type: String,
      default: "Nothing selected"
    },
    labelNotFound: {
      type: String,
      default: "No results found for"
    },
    options: {
      type: Array,
      default: () => []
    },
    search: {
      type: Boolean,
      default: false
    },
    showDefaultOption: {
      type: Boolean,
      default: false
    },
    textProp: {
      type: String,
      default: "text"
    },
    value: {
      type: [Object, String, Number],
      default: null
    },
    valueProp: {
      type: String,
      default: "value"
    }
  },
  data() {
    return {
      show: false,
      selectedValue: this.value,
      searchValue: ""
    };
  },
  computed: {
    filteredProps() {
      if (this.search && this.searchValue.length > 0) {
        return this.options.filter(item => {
          if (typeof option === "object") {
            return (
              item[this.textProp].toLowerCase().indexOf(this.searchValue) !== -1
            );
          } else {
            return item;
          }
        });
      }
      return this.options;
    }
  },
  methods: {
    handleSelect(option) {
      this.selectedValue = option;
      this.hideDropdown();
      this.$emit("input", option);
    },
    hideDropdown() {
      this.show = false;
      this.searchValue = "";
    },
    getOptionLabel(option) {
      if (typeof option === "object") {
        return option[this.textProp];
      }
      return option;
    }
  }
};
</script>

<style lang="css" scoped>
.v-select {
    position: relative;
    width: 100%;
    height: 30px;
    background-color: #f8f9fa;
    border-color: #f8f9fa;
    border-radius: 0.25rem;
    line-height: 1.5;
    color: #212529;
    font-size: 12px;
    transition: background-color, border-color, box-shadow, .15s ease-in-out;
    font-family: inherit, sans-serif;
    cursor: pointer;
}

.v-select:hover {
    background-color: #e2e6ea;
    border-color: #dae0e5;
}

.v-select-toggle {
    display: flex;
    justify-content: space-between;
    user-select: none;
    padding: 0.375rem 0.75rem;
}


.arrow-down {
    display: inline-block;
    width: 0;
    height: 0;
    margin-left: 0.255em;
    margin-top: 7px;
    vertical-align: 0.255em;
    content: "";
    border-top: 0.3em solid;
    border-right: 0.3em solid transparent;
    border-bottom: 0;
    border-left: 0.3em solid transparent;
}

.v-dropdown-container {
    position: absolute;
    width: 100%;
    background: red;
    padding: 0.5rem 0;
    margin: 0.125rem 0 0;
    color: #212529;
    text-align: left;
    list-style: none;
    background-color: #fff;
    background-clip: padding-box;
    border-radius: 0.25rem;
    border: 1px solid rgba(0,0,0,.15);
    z-index: 1000;
}

ul {
    font-size: 12px;
    color: #424242;
    text-align: left;
    list-style: none;
    background-color: #fff;
    background-clip: padding-box;
    padding: 0px;
    margin: 0px;
}

.v-dropdown-item {
    line-height: 25px;
    padding: 0.5rem 1.25rem;
    user-select: none;
}

.v-dropdown-item:hover:not(.default-option) {
    text-decoration: none;
    background-color: #f8f9fa;
}

.v-dropdown-item.disabled {
    color: #9A9B9B;
}

.v-dropdown-item.selected {
    background-color: #007bff;
    color: #fff;
}

.v-dropdown-item.selected:hover {
    background-color: #007bff;
    color: #fff;
}

.search-container {
    padding: 0.5rem 1.25rem;
}

input {
    width: 100%;
}
</style>
