<style lang="scss" scoped>
    @import 'src/scss/layout/public.scss';
    .index {
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        display: flex;
        flex-direction: column;
        align-items: stretch;
        .index-content {
            flex-grow: 1;
            display: flex;
            align-items: stretch;
            overflow: hidden;
        }
        .router-content {
            position: relative;
            flex-grow: 1;
            >* {
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                padding: 15px;
            }
        }
    }
</style>

<template>
    <div class="index ui-scrollbar-global" v-if="signed">

        <topbar></topbar>

        <div class="index-content">

            <sidebar></sidebar>

            <div class="router-content overflow-auto ui-scrollbar scroll-blue scroll-hover scroll-no-track">
                <router-view></router-view>
            </div>

            <!--<feedback></feedback>-->
        </div>

    </div>
</template>

<script>
    import sidebar from 'components/sidebar'
    import topbar from 'components/topbar'
    // import feedback from 'components/feedback'

    export default {
        components: {
            sidebar,
            topbar,
            // feedback,
        },
        data () {
            return {
                signed: false
            }
        },
        methods: {
            checkLogin() {
               return sessionStorage.isLogged || localStorage.isLogged
            },
        },
        activated () {
            if (!this.checkLogin()) {
                this.$router.push('login')
                return
            }

            this.signed = true
        }
    }
</script>