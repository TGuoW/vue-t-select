<template>
    <div>
        <Tselect v-model="value" :options="options" clearable placeholder="123" filterable></Tselect>
    </div>
</template>

<script>
import Tselect from './vue-t-select'
export default {
    data () {
        return {
            value: 'aa6aa',
            options: [
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
    },
    components: {
        Tselect
    }
}
</script>
