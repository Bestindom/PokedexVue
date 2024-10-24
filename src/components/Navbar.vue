<template>
    <nav class="navbar bg-body-tertiary fixed-top">
        <div class="container-fluid">
            <a class="navbar-brand" href="#"></a>
            <button class="navbar-toggler" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasNavbar"
                aria-controls="offcanvasNavbar" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasNavbar"
                aria-labelledby="offcanvasNavbarLabel">
                <div class="offcanvas-header">
                    <h5 class="offcanvas-title" id="offcanvasNavbarLabel">Menu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
                </div>
                <div class="offcanvas-body">
                    <ul class="navbar-nav justify-content-end flex-grow-1 pe-3">
                        <li class="nav-item">
                            <button class="nav-link active" aria-current="page" @click="changeSection('pokedex')">Pokédex</button>
                        </li>
                        <li class="nav-item">
                            <button class="nav-link" @click="changeSection('store')">Store</button>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </nav>
</template>
<script>
export default {
    methods: {
        changeSection(sectionName) {
            this.$emit("section", sectionName);
        }
    }
}
</script>
<style>

</style>