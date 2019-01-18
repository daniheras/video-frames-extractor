<template>
    <header class="dragger">
        <b-modal id="modal1" title="About me">
            <p class="my-4">This software has been developed and maintained by Galeon Software.</p>
        </b-modal>
        <b-navbar toggleable="md" type="dark" variant="primary">
            <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

            <b-navbar-brand>Video Extractor</b-navbar-brand>

            <b-collapse is-nav id="nav_collapse">

                <!-- <b-navbar-nav>
                <b-nav-item-dropdown text="File" left>
                    <b-dropdown-item v-b-modal.modal1>About</b-dropdown-item>
                </b-nav-item-dropdown>
                </b-navbar-nav> -->

            </b-collapse>
        </b-navbar>
        <br>
    </header>
</template>
<style lang="scss" scoped>
.dragger{
    -webkit-app-region: drag;
    cursor: move;
}
</style>

