<template>
    <div>
        <footer class="footer">
            <div class="footer__block">
                <RouterLink :to="{ name: 'home' }">
                    <div class="logo">
                        <p class="logo__text"> Footer</p>
                    </div>
                </RouterLink>
            </div>
        </footer>
    </div>
</template>

<script setup lang="ts">

</script>

<style lang="scss" scoped>
@import '../assets/variables';

.footer {
    width: 100%;
    border-top: 1px solid #dadada;
    background-color: #F8F8FF;
}

.footer__block {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 0 auto;
    width: 100%;
    padding: 30px;

    @media (min-width: 500px) {
        max-width: $tablet-width;
    }

    @media (min-width: 1200px) {
        max-width: $desctop-width;
    }
}

.logo__text {
    font-family: $main-font;
    font-weight: 600;
    font-size: 18px;
    color: #7284FF;

    @media (min-width: 600px) {
        font-size: 28px;
    }
}
</style>