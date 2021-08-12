<template>
    <div class="portal-component__input-option" @click=" $parentEmit('picked') " @mouseenter=" $parentEmit('hover') ">

        <template v-if=" !$slots.default ">
            {{ label }}
        </template>

        <template v-else>
            <slot></slot>
        </template>
    </div>
</template>

<script>

    export default {

        props: {

            value: {
                type: [ String, Number, Boolean ]
            },

            label: {
                type: String
            }

        },

        methods: {



            /**
             * MVC 的 V 层方法
             */

            $parentEmit($event) {

                for (let a = this.$parent; a; a = a.$parent) {

                    if ( a.isInputOptionParent ) {

                        a.$emit($event, this);
                        break;

                    }

                }

            }

        },

        data() {

            return {
                isInputOptionComponent: true
            };

        }

    }

</script>
