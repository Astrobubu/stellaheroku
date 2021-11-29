// Stellarium Web - Copyright (c) 2018 - Noctua Software Ltd
//
// This program is licensed under the terms of the GNU AGPL v3, or
// alternatively under a commercial licence.
//
// The terms of the AGPL v3 license can be found in the main directory of this
// repository.

<template>

<div class="secondary" style="position:absolute; width: 100%; height: 100%;">
  <v-container style="width: 100%; height: 100%;">
    <v-layout column align-center style="width: 100%; height: 100%;">
      <img style="padding-top: 10%;" src="@/assets/images/logos.png" width="1080px" height="675px" alt="Logo"/>
      <div class="text-h2" style="padding-top: 10%;">Stellarium Web</div>
      <div v-if="$store.state.wasmSupport" style="margin: auto;">
        <div style="display:flex; justify-content: center;">
          <p class="grey--text"><i18n path="Loading {0}, the online Star Map of the UAE"> </p>
        </div>
        <div style="display:flex; justify-content: center;">
          <v-progress-circular indeterminate v-bind:size="70" v-bind:width="7" class="grey--text"/>
        </div>
      </div>
      <v-card v-else style="margin: auto;">
        <v-card-title primary-title>
          <div class="text-h5"><h1>{{ $t('Could not show the Online Star Map') }}</h1></div>
          <div class="text-h5" style="margin-top: 30px"><v-icon large>error</v-icon> {{ $t('It seems that your browser cannot load Web Assembly!') }}</div>
        </v-card-title>
        <v-card-text>
          <v-layout column align-center style="width: 100%; height: 100%;">
            <p class="grey--text">{{ $t('Web assembly is necessary for Stellarium Web to display the star map. Please upgrade your web browser and try again!') }}</p>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-layout>
  </v-container>
</div>

</template>

<script>
export default {
}
</script>

<style>
</style>
