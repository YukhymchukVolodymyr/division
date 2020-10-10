<template>
    <div class="d-flex pl-1 pr-1">
        <div class="spaces">
            <span v-for="(space, index) in item.spaces" :key="index"
                  style="visibility: hidden">0</span>
        </div>
        <div>
            <div v-show="componentIndex > 0" class="divisor">
                {{ item.divisor }}
            </div>
            <div class="subtractor">
                {{ item.subtractor }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DivisionItem',

    props: {
        item: {
            type: Object,
            required: true
        },
        componentIndex: {
            type: Number,
            required: true
        }
    }
}
</script>

