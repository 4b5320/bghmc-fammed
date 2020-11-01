<template>
  <div class="home">
    <v-card>
        <v-card-title>
            BGHMC Employee and Patient Information
        </v-card-title>
        <v-tabs color="green" right>
            <v-tab>All</v-tab>
            <v-tab>Linked</v-tab>
            <v-tab>Unlinked</v-tab>

            <v-tab-item>
                <v-divider></v-divider>
                <v-container>
                    <AllEmployees></AllEmployees>
                </v-container>
            </v-tab-item>

            <v-tab-item>
                <v-divider></v-divider>
                <v-container>
                    
                </v-container>
            </v-tab-item>

            <v-tab-item>
                <v-divider></v-divider>
                <v-container>
                    
                </v-container>
            </v-tab-item>
        </v-tabs>
    </v-card>
  </div>
</template>

<script>
import AllEmployees from './AllEmployees'

export default {
  name: 'Home',
  components: {
      AllEmployees
  }
}
</script>
