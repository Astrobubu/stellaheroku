// Stellarium Web - Copyright (c) 2018 - Noctua Software Ltd
//
// This program is licensed under the terms of the GNU AGPL v3, or
// alternatively under a commercial licence.
//
// The terms of the AGPL v3 license can be found in the main directory of this
// repository.

<template>
  <div style="position: absolute; display:flex; align-items: flex-end;">
   <v-menu v-if="$store.state.showTimeButtons" :close-on-content-click="false" transition="v-slide-y-transition" offset-y top left>
      <template v-slot:activator="{ on }">
        <v-btn large class="tmenubt" color="secondary" v-on="on">
          <v-icon class="hidden-sm-and-up">mdi-clock-outline</v-icon>
          <span class="hidden-xs-only">
            <div class="text-subtitle-2">{{ time }}</div>
            <div class="text-caption">{{ date }}</div>
          </span>
        </v-btn>
      </template>
      <date-time-picker v-model="pickerDate" :location="$store.state.currentLocation"></date-time-picker>
    </v-menu>
    <v-spacer></v-spacer>

    <bottom-button :label="$t('Constellations')"
                v-if="$store.state.showConstellationsLinesButton !== false"
                :img="require('@/assets/images/btn-cst-lines.svg')"
                img_alt="Constellations Button"
                :toggled="$store.state.stel.constellations.lines_visible"
                @clicked="(b) => { $stel.core.constellations.lines_visible = b; $stel.core.constellations.labels_visible = b }">
    </bottom-button>
    <bottom-button :label="$t('Constellations Art')"
                v-if="$store.state.showConstellationsArtButton !== false"
                :img="require('@/assets/images/btn-cst-art.svg')"
                img_alt="Constellations Art Button"
                :toggled="$store.state.stel.constellations.images_visible"
                @clicked="(b) => { $stel.core.constellations.images_visible = b }">
    </bottom-button>
    <bottom-button :label="$t('Atmosphere')"
                v-if="$store.state.showAtmosphereButton !== false"
                :img="require('@/assets/images/btn-atmosphere.svg')"
                img_alt="Atmosphere Button"
                :toggled="$store.state.stel.atmosphere.visible"
                @clicked="(b) => { $stel.core.atmosphere.visible = b }">
    </bottom-button>
    <bottom-button :label="$t('Landscape')"
                v-if="$store.state.showLandscapeButton !== false"
                :img="require('@/assets/images/btn-landscape.svg')"
                img_alt="Landscape Button"
                :toggled="$store.state.stel.landscapes.visible"
                @clicked="(b) => { $stel.core.landscapes.visible = b }">
    </bottom-button>
    <bottom-button :label="$t('Night Mode')"
                v-if="$store.state.showNightmodeButton !== false"
                :img="require('@/assets/images/btn-night-mode.svg')"
                img_alt="Night Mode Button"
                class="mr-auto"
                :toggled="$store.state.nightmode"
                @clicked="(b) => { setNightMode(b) }">
    </bottom-button>
    <bottom-button :label="$t('Fullscreen')"
                :img="fullscreenBtnImage"
                img_alt="Fullscreen Button"
                class="mr-auto hidden-xs-only"
                :toggled="$store.state.fullscreen"
                @clicked="(b) => { setFullscreen(b) }">
    </bottom-button>

    <v-spacer></v-spacer>
    <div class="attribution-container">
      <div>Brought to you by</div>
      <div>
       <img class="tbtitle" alt="Emirates NBD" src="@/assets/images/nbdcrop.png" href="www.emiratesnbd.com" width="85" height="130"/>
     </div>
    </div>
  </div>
</template>

<script>

import BottomButton from '@/components/bottom-button.vue'
import DateTimePicker from '@/components/date-time-picker.vue'
import Moment from 'moment'

export default {
  components: { BottomButton, DateTimePicker },
  data: function () {
    return {
    }
  },
  computed: {
    time: {
      get: function () {
        return this.getLocalTime().format('HH:mm:ss')
      }
    },
    date: {
      get: function () {
        return this.getLocalTime().format('YYYY-MM-DD')
      }
    },
    fullscreenBtnImage: function () {
      return this.$store.state.fullscreen ? require('@/assets/images/svg/ui/fullscreen_exit.svg') : require('@/assets/images/svg/ui/fullscreen.svg')
    },
    pickerDate: {
      get: function () {
        const t = this.getLocalTime()
        t.milliseconds(0)
        return t.format()
      },
      set: function (v) {
        const m = Moment(v)
        m.local()
        m.milliseconds(this.getLocalTime().milliseconds())
        this.$stel.core.observer.utc = m.toDate().getMJD()
      }
    }
  },
  methods: {
    // The MomentJS time in local time
    getLocalTime: function () {
      var d = new Date()
      d.setMJD(this.$store.state.stel.observer.utc)
      const m = Moment(d)
      m.local()
      return m
    },
    locationClicked: function () {
      this.$store.commit('toggleBool', 'showLocationDialog')
    },
    setFullscreen: function (b) {
      this.$fullscreen.toggle(document.body, {
        wrap: false,
        callback: this.onFullscreenChange
      })
    },
    setNightMode: function (b) {
      this.$store.commit('toggleBool', 'nightmode')
      if (window.navigator.userAgent.indexOf('Edge') > -1) {
        document.getElementById('nightmode').style.opacity = b ? '0.5' : '0'
      }
      document.getElementById('nightmode').style.visibility = b ? 'visible' : 'hidden'
    },
    onFullscreenChange: function (b) {
      if (this.$store.state.fullscreen === b) return
      this.$store.commit('toggleBool', 'fullscreen')
    }
  }
}
</script>

<style>
@media all and (max-width: 600px) {
  .tmenubt {
    min-width: 30px;
  }
}
@media all and (min-width: 600px) {
  .tbtcontainer {
    width: 300px;
  }
}

.attribution-container {
  text-align: right;
  margin: 6px;
}

.attribution-container .hearth {
  color: red;
}

.stellarium-logo-container {
  display: block;
}
.stellarium-logo-container span {
  color: #fff;
}
</style>
