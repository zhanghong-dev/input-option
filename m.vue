<template>
    <div v-show=" show " class="portal-component__input-option" @click=" parentEmit('picked', 'hover') ">

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

        computed: {

            parent() {

                for (let a = this.$parent; a; a = a.$parent) {

                    if ( a.isInputOptionParent )
                        return a;

                }

            },

            _label() {

                if ( this.$slots.default ) {

                    let label = '';

                    for (let a = null, i = 0; a = this.$slots.default[i]; i++) {

                        label += a.elm.innerHTML.toLowerCase();

                    }

                    return label;

                }

                else
                    return this.label.toLowerCase();

            }

        },

        methods: {

            parentEmit(...$event) {

                for (let a = null, i = 0; a = $event[i]; i++) {

                    this.parent.$emit(a, this);

                }

            }

        },

        mounted() {

            this.parent.$on('filter',
                ($value) => {

                    this.show = this._label.indexOf( $value.toLowerCase() ) !== -1;

                }
            );

        },

        data() {

            return {
                isInputOptionComponent: true,
                show: true
            };

        }

    }

</script>
