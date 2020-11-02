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
                    <LinkedEmployees></LinkedEmployees>
                </v-container>
            </v-tab-item>

            <v-tab-item>
                <v-divider></v-divider>
                <v-container>
                    <UnlinkedEmployees></UnlinkedEmployees>
                </v-container>
            </v-tab-item>
        </v-tabs>
    </v-card>
  </div>
</template>

<script>
import AllEmployees from './AllEmployees'
import LinkedEmployees from './LinkedEmployees'
import UnlinkedEmployees from './UnlinkedEmployees'

export default {
  name: 'Home',
  components: {
      AllEmployees,
      LinkedEmployees,
      UnlinkedEmployees
  }
}
</script>
