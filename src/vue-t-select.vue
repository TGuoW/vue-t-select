<script>
const FOLD = 'FOLD'
const FOCUS = 'FOCUS'
const UNFOLD = 'UNFOLD'
const FOLD_FOCUS = 'FOLD_FOCUS'
const UNFOLD_FOCUS = 'UNFOLD_FOCUS'

const config = {
    [UNFOLD_FOCUS]: {
        OUT (that) {
            that.state = FOLD
        },
        IN (that) {
            that.state = FOLD_FOCUS
        },
        SELECT (that) {
            that.state = FOLD_FOCUS
        }
    },
    [FOLD]: {
        IN (that) {
            that.state = UNFOLD_FOCUS
            const eventListener = () => {
                that.state = FOLD
                document.removeEventListener('click', eventListener)
            }
            document.addEventListener('click', eventListener)
        }
    },
    [FOLD_FOCUS]: {
        IN (that) {
            that.state = UNFOLD_FOCUS
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
            // required: true,
            default() {
                return ''
            }
        },
        options: {
            type: Array,
            default () {
                return [
                    {value: 'aaa', label: 'asdasd'},
                    {value: 'aa6aa', label: 'asdasd1'},
                    {value: 'aa3aa', label: 'asdasd1'},
                    {value: 'a1aaa', label: 'asdasd1'},
                    {value: 'aaa3a', label: 'asdasd1'},
                    {value: 'aa51aa', label: 'asdasd1'},
                    {value: 'aaa1a', label: 'asdasd1'},
                    {value: 'aa23aa', label: 'asdasd1'},
                    {value: 'aa4aa', label: 'asdasd1'},
                    {value: 'a5aaa', label: 'asdasd1'},
                    {value: 'aa4aa', label: 'asdasd1'},
                    {value: 'aa5aa', label: 'asdasd1'},
                    {value: 'aaaga', label: 'asdasd1'},
                    {value: 'aaaqa', label: 'asdasd1'},
                    {value: 'aasaaa', label: 'asdasd1'},
                    {value: 'aavaa', label: 'asdasd1'},
                    {value: 'aaaa', label: 'asdasd1'},
                    {value: 'aahaa', label: 'asdasd1'},
                    {value: 'aajaa', label: 'asdasd1'},
                    {value: 'aabaa', label: 'asdasd1'},
                    {value: 'aaaaa', label: 'asdasd12'}
                ]
            }
        }
    },
    data() {
        return {
            state: FOLD,
            selected: {value: '', label: ''}
        };
    },
    methods: {
        handleClick () {
            config[this.state].IN(this)
            event.stopImmediatePropagation()
        },
        selectItem (value) {
            this.selected = value
            config[this.state].SELECT(this)
            this.$emit('input', value.value)
            this.$emit('change', value)
            event.stopPropagation()
        }
    },
    render () {
        const {selected, state, options} = this
        return (
            <div class="vue-t-select-w">
                <div
                    class={["vue-t-select", state.includes(FOCUS) ? 'vue-t-select-focus' : '']}
                    onClick={this.handleClick}>
                    <input type="text" value={selected.label} placeholder={this.placeholder} ref="input" readonly/>
                </div>
                <ul v-show={state.includes(UNFOLD)} class="vue-t-select-dropdown">
                    { options.map(item =>
                        <li
                            class={[selected.value === item.value ? 'vue-t-select-highlight' : '']}
                            onClick={() => this.selectItem(item)}>
                            {item.label}
                        </li>
                    )}
                </ul>
            </div>
        )
    }
}
</script>

<style scoped>
    .vue-t-select-w {
        position: relative;
        margin: 25px auto;
        width: 240px;
    }
    .vue-t-select {
        display: block;
        width: 240px;
        height: 40px;
        border: 1px solid #ccc;
        border-radius: 4px;
        text-align: center;
        padding: 4px;
        box-sizing: border-box;
        text-align: left;
    }
    .vue-t-select input {
        font-size: 16px;
        border: none;
        outline: none;
        width: 100%;
        line-height: 30px;
    }
    .vue-t-select-focus {
        border: 1px solid rgb(9, 169, 197);
    }
    .vue-t-select-dropdown {
        position: absolute;
        left: 0;
        min-width: 240px;
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
