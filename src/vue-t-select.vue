<template>
    <div class="vue-t-select-w">
        <div
            class="vue-t-select"
            :class="state.includes('FOCUS') ? 'vue-t-select-focus' : ''"
            @click="handleClick"
            @mouseover="showClose = true"
            @mouseout="showClose = false">
            <span class="prepend" v-if="$slots.prepend">
                <slot name="prepend"></slot>
            </span>
            <div class="content">
                <input
                    type="text"
                    @input="filterOptions"
                    :value="inputLabel"
                    :placeholder="placeholder"
                    ref="input"
                    :readonly="!filterable"/>
                <span
                    v-show="showClose && clearable && inputLabel !== ''"
                    class="vue-t-select-close"
                    @click="clearItem"></span>
            </div>
            <span class="append" v-if="$slots.append">
                <slot name="append"></slot>
            </span>
        </div>
        <transition name="vue-t-transition">
        <ul v-if="state.includes('UNFOLD')" class="vue-t-select-dropdown">
                <li
                    v-for="item in displayOptions"
                    :key="item.value"
                    :class="selected.value === item.value ? 'vue-t-select-highlight' : ''"
                    @click="selectItem(item)">
                    {{item.label}}
                </li>
            <li v-if="!displayOptions.length">暂无数据</li>
        </ul>
        </transition>
    </div>
</template>

<script>
const FOLD = 'FOLD'
// const FOCUS = 'FOCUS'
// const UNFOLD = 'UNFOLD'
const FOLD_FOCUS = 'FOLD_FOCUS'
const UNFOLD_FOCUS = 'UNFOLD_FOCUS'

const config = {
    INITIAL () {
        this.state = FOLD
    },
    [UNFOLD_FOCUS]: {
        OUT () {
            this.state = FOLD
        },
        IN () {
            this.state = FOLD_FOCUS
        },
        SELECT () {
            this.state = FOLD_FOCUS
        }
    },
    [FOLD]: {
        IN () {
            this.state = UNFOLD_FOCUS
        }
    },
    [FOLD_FOCUS]: {
        IN () {
            this.state = UNFOLD_FOCUS
        }
    }
}
export default {
    name: 'VueTSelect', // vue component name
    props: {
        placeholder: {
            type: String,
            default () {
                return '请选择'
            }
        },
        value: {
            type: String,
            required: true,
            default() {
                return ''
            }
        },
        options: {
            type: Array,
            required: true,
            default () {
                return []
            }
        },
        clearable: {
            type: Boolean,
            default () {
                return false
            }
        },
        filterable: {
            type: Boolean,
            default () {
                return false
            }
        }
    },
    data() {
        const selected = this.options.filter(item => item.value === this.value)
        return {
            displayOptions: this.options,
            showClose: false,
            state: FOLD,
            selected: selected.length ? selected[0] : {value: '', label: ''},
            inputLabel: selected.length ? selected[0].label : ''
        };
    },
    methods: {
        handleClick () {
            const { state } = this
            config[state].IN.apply(this)
            if (state === FOLD) {
                const eventListener = () => {
                    config[UNFOLD_FOCUS].OUT.apply(this)
                    this.inputLabel = this.selected.label
                    this.displayOptions = this.options
                    document.removeEventListener('click', eventListener)
                }
                document.addEventListener('click', eventListener)
            }
            event.stopPropagation()
        },
        selectItem (value) {
            config[this.state].SELECT.apply(this)
            this.changeSelected(value)
            event.stopPropagation()
        },
        clearItem () {
            config.INITIAL.apply(this)
            const item = {value: '', label: ''}
            this.changeSelected(item)
            this.displayOptions = this.options
            this.$emit('clear')
            event.stopPropagation()
        },
        changeSelected (item) {
            this.selected = item
            this.inputLabel = item.label
            this.$emit('input', item.value)
            this.$emit('change', item)
        },
        filterOptions (e) {
            const value = e.target.value
            this.inputLabel = value
            this.displayOptions = this.options.filter(item => item.label.includes(value))
        }
    },
    // render (h) {
    //     const {selected, inputLabel, state, displayOptions, showClose, clearable, placeholder, filterable} = this
    //     return (
    //         <div class="vue-t-select-w">
    //             <div
    //                 class={["vue-t-select", state.includes(FOCUS) ? 'vue-t-select-focus' : '']}
    //                 onClick={this.handleClick}
    //                 onmouseover={() => {this.showClose = true}}
    //                 onmouseout={() => {this.showClose = false}}>
    //                 <input
    //                     type="text"
    //                     onInput={this.filterOptions}
    //                     value={inputLabel}
    //                     placeholder={placeholder}
    //                     ref="input"
    //                     readonly={!filterable}/>
    //                 <span
    //                     v-show={showClose && clearable && inputLabel !== ''}
    //                     class="vue-t-select-close"
    //                     onClick={() => this.clearItem()}></span>
    //             </div>
    //             <transition name="vue-t-transition">
    //                 {state.includes(UNFOLD) &&
    //                     <ul class="vue-t-select-dropdown">
    //                         { displayOptions.map(item =>
    //                             <li
    //                                 class={[selected.value === item.value ? 'vue-t-select-highlight' : '']}
    //                                 onClick={() => this.selectItem(item)}>
    //                                 {item.label}
    //                             </li>
    //                         )}
    //                         { !displayOptions.length && <li>暂无数据</li> }
    //                     </ul>
    //                 }
    //             </transition>
    //         </div>
    //     )
    // }
}
</script>

<style scoped>
    .vue-t-transition-enter-active, .vue-t-transition-leave-active {
        transition: all .2s;
    }
    .vue-t-transition-enter, .vue-t-transition-leave-to {
        transform-origin: 0% 0%;
        transform: scale(1, 0);
        opacity: 0;
    }

    .vue-t-select-w {
        position: relative;
        width: 300px;
    }
    .vue-t-select {
        display: flex;
        height: 40px;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        text-align: left;
        font-size: 16px;
        overflow: hidden;
    }
    .vue-t-select input {
        padding: 0 16px;
    }
    .vue-t-select .prepend {
        color: #909399;
        padding: 0 16px;
        line-height: 38px;
        background: #f5f7fa;
        border-right: 1px solid #dcdfe6;
    }
    .vue-t-select .append {
        color: #909399;
        padding: 0 16px;
        line-height: 38px;
        background: #f5f7fa;
        border-left: 1px solid #dcdfe6;
    }
    .vue-t-select input {
        box-sizing: border-box;
        width: 100%;
        font-size: 16px;
        border: none;
        outline: none;
        line-height: 38px;
    }
    .vue-t-select .content {
        height: 100%;
        flex: 1;
        position: relative;
    }
    .vue-t-select-close {
        cursor: pointer;
        background: rgb(235, 235, 235);
        color: #a1a1a1;
        border-radius: 12px;
        line-height: 16px;
        text-align: center;
        height: 16px;
        width: 16px;
        font-size: 14px;
        padding: 1px;
        top: 0;
        bottom: 0;
        right: 10px;
        margin: auto;
        position: absolute;
    }
    .vue-t-select-close::before {
        content: "\2716";
    }
    .vue-t-select-close:hover {
        background: rgb(216, 216, 216);
    }
    .vue-t-select-focus {
        border: 1px solid rgb(9, 169, 197);
    }
    .vue-t-select-dropdown {
        position: absolute;
        left: 0;
        /* min-width: 300px; */
        width: 100%;
        max-height: 440px;
        overflow: auto;
        border: 1px solid #ccc;
        border-radius: 4px;
        padding-inline-start: 0;
    }
    .vue-t-select-dropdown li {
        list-style: none;
        cursor: pointer;
        padding: 6px;
        color: darkgray;
    }
    .vue-t-select-dropdown li:hover {
        background: rgb(140, 202, 212);
        color: #fff;
    }
    .vue-t-select-dropdown .vue-t-select-highlight {
        background: rgb(9, 169, 197);
        color: #fff;
    }
</style>
