<template>
    <v-container>
        <v-row class="text-center">
            <v-col class="mb-4">
                <h1 class="display-2 font-weight-bold mb-3">
                    Visualization of division
                </h1>
                <v-form>
                    <v-container>
                        <v-row>
                            <v-col col="12" md="6">
                                <v-text-field
                                    v-model.number="divisor"
                                    label="Divisor"
                                    type="number"
                                    required
                                />
                            </v-col>
                            <v-col col="12" md="6">
                                <v-text-field
                                    v-model.number="denominator"
                                    label="Denominator"
                                    type="number"
                                    required
                                />
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col v-if="divisor && denominator">
                                <div class="d-inline-flex align-start">
                                    <div class="left">
                                        <div class="divisor pl-1 pr-1">{{ divisor }}</div>

                                        <division-item v-for="(item, index) in calculation" :key="index" :item="item"
                                                       :component-index="index"/>

                                    </div>
                                    <div class="right">
                                        <div class="denominator pl-1 pr-1">
                                            {{ denominator }}
                                        </div>
                                        <div class="pl-1 pr-1">
                                            {{ result }}
                                        </div>
                                    </div>
                                </div>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-form>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import DivisionItem from './DivisionItem'

const DECIMAL_COUNT = 5

export default {
    name: 'Division',

    components: {
        DivisionItem
    },

    data() {
        return {
            divisor: null,
            denominator: null,
            result: ''
        }
    },

    computed: {
        calculation() {
            return this.longDivision(this.divisor, this.denominator)
        }
    },

    methods: {
        longDivision(n, d) {
            const multiplier = Math.pow(10, Math.max(this.getDecimalCount(n), this.getDecimalCount(d))),
                number = Math.trunc(n * multiplier) + '',
                denominator = Math.trunc(d * multiplier)

            let reminder = 0,
                answer = '',
                spaces = 0,
                calculationArray = [],
                dot = false

            for(let i = 0; i < number.length + DECIMAL_COUNT + 1; i++) {
                const digit = i < number.length ? parseInt(number[i]) : 0
                const divisor = digit + (reminder * 10)
                const answerValue = Math.floor(divisor / denominator)
                const subtractor = divisor - divisor % denominator
                const lessZero = n < d
                const integerEnd = number.length - 1 <= i

                if(answerValue || (integerEnd && lessZero)) {
                    answer = answer + answerValue
                }

                if(integerEnd && !dot) {
                    answer += '.'
                    dot = true
                }

                if(answerValue) {
                    calculationArray.push({
                        divisor: divisor,
                        subtractor: subtractor,
                        spaces: spaces
                    })
                    spaces += this.getSpacesValue(divisor, subtractor)
                }

                reminder = divisor % denominator
            }

            this.result = parseFloat(answer).toFixed(5)

            return calculationArray
        },

        getSpacesValue(divisor, subtractor) {
            if(divisor === subtractor) {
                return divisor.toString().length
            } else {
                return divisor.toString().length - (divisor - subtractor).toString().length
            }
        },

        getDecimalCount(value) {
            return value % 1 ? value.toString().split('.')[1].length : 0
        }
    }
}
</script>

<style>
    .right {
        border-left: 1px solid black;
    }

    .denominator {
        border-bottom: 1px solid black;
    }

    .subtractor {
        position: relative;
        border-bottom: 1px solid black;
        text-align: right;
    }

    .divisor {
        text-align: left;
    }

    .subtractor:before {
        position: absolute;
        content: '';
        top: 0;
        left: 0;
        transform: translateX(-100%);
        width: 5px;
        height: 1px;
        background: black;
    }
</style>
