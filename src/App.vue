<template>
    <div id="app">
        <v-app>
            <v-navigation-drawer app>
                <!-- -->
            </v-navigation-drawer>

            <v-app-bar app color="#bbe6d6"  dense>
                <!-- -->
            </v-app-bar>

            <!-- Sizes your content based upon application components -->
            <v-main style="background-color:#c8ebdf">

                <!-- Provides the application the proper gutter -->
                <v-container fluid>

                <!-- If using vue-router -->
                <router-view></router-view>
                </v-container>
            </v-main>

            <v-footer app>
                <!-- -->
            </v-footer>
        </v-app>
    </div>
</template>