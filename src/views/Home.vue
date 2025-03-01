<i18n src="@/locales/long_langs/home.json"></i18n>
<template>
  <div id="IHR_home">
    <div id="IHR_global-report" class="row">
      <div class="col">
        <div>Internet Health Report</div>
        <router-link id="IHR_global-report-button" :to="{ name: 'global_report' }">
          {{ $t('globalReport.name') }}
        </router-link>
      </div>
    </div>

    <div
      class="IHR_description-main"
      v-html="
        $interpolateArray($t('globalReport.description'), {
          ripe: '<a href=\'https://atlas.ripe.net/\' target=\'_blank\'>RIPE Atlas</a>',
          bgpstream: '<a href=\'https://bgpstream.caida.org/\' target=\'_blank\'>BGPstream</a>',
          }, ['<div>','</div>'])
      "
    ></div>
    <div class="IHR_description-main">
      See latest alarms in the
      <router-link :to="{ name: 'global_report' }">global report</router-link>
      and network metrics in
      <router-link :to="{ name: 'networks' }">network reports</router-link>.
    </div>

    <div class="row wrap justify-center q-gutter-md IHR_description-main">
      <q-card class="analysis-modules" v-for="graphT in graphTypes" :key="graphT.name">
        <q-card-section class="bg-primary text-white q-pa-sm">
          <div>
            <q-avatar :icon="graphT.icon"></q-avatar>
            {{ $t(`${graphT.name}.title`) }}
          </div>
        </q-card-section>
        <q-card-section class="q-pa-xs">
          <div class="IHR_description_text" v-html="$interpolateArray($t(`${graphT.name}.description`), placeholderValues, ['<div>','</div>'])"></div>
          <div class="IHR_description IHR_description-link">
            <router-link
              :to="{
                name: 'documentation',
                hash: $t(`${graphT.name}.docHash`),
              }"
              >{{ $t('learnmore') }} {{ $t(`${graphT.name}.title`) }}</router-link
            >
          </div>
        </q-card-section>
      </q-card>
    </div>

    <div class="IHR_section">
      <q-card class="IHR_tweets-types">
        <q-card-section class="bg-white text-primary q-pa-sm">
          <div class="text-h2">
            <q-avatar icon="fab fa-twitter"></q-avatar>
            {{ $t('ihrTweets.title') }}
          </div>
        </q-card-section>
        <q-card-section class="q-pa-xs">
          <Timeline
            id="ihr_alerts"
            sourceType="profile"
            :options="{
              chrome: 'noheader',
              tweetLimit: '3',
            }"
          >
            <div class="spinner"></div>
          </Timeline>
        </q-card-section>
      </q-card>
    </div>

    <div class="IHR_section">
      <h2 class="text-h2">
        <q-icon name="fa fa-heart"></q-icon>
        {{ $t('ack.title') }}
      </h2>

      <div class="row wrap justify-center">
        <div class="col-xl" v-for="org in $t('ack.organizations')" :key="org.name">
          <a :href="org.url" target="_blank">
          <div class="IHR_ack-logo">
            <span></span>
            <img :src="require(`@/assets/imgs/${org.logo}`)" :alt="org.name" />
          </div>
          </a>
        </div>
      </div>
    </div>
    <!--<q-drawer :value="hideSidebar" side="left" show-if-above bordered>-->
    <!--<q-scroll-area class="fit">-->
    <!--</q-scroll-area>-->
    <!--</q-drawer>-->
  </div>
</template>

<script>
import { Timeline } from 'vue-tweet-embed'
const GRAPHS_TYPES = [
  {
    name: 'analysisModules.asInterdependence',
    icon: 'fas fa-project-diagram',
    docsQuery: {},
  },
  {
    name: 'analysisModules.networkDelay',
    icon: 'fas fa-shipping-fast',
    docsQuery: {},
  },
  {
    name: 'analysisModules.delayAndForwarding',
    icon: 'fas fa-exchange-alt',
    docsQuery: {},
  },
  {
    name: 'analysisModules.disco',
    icon: 'fas fa-plug',
    docsQuery: {},
  },
]

const PLACEHOLDER_VALUES = {
  asPaper: "<a href='https://www.iijlab.net/en/members/romain/pdf/romain_pam2018.pdf' target='_blank'>",
  close: '</a>',
  /*
  ripe: "<a href='https://atlas.ripe.net/' target='_blank'>RIPE Atlas</a>",
  bgpstream:
    "<a href='https://bgpstream.caida.org/' target='_blank'>BGPstream</a>",
  delayAndForwardingPaper:
    "<a href='https://conferences.sigcomm.org/imc/2017/papers/imc17-final106.pdf' target='_blank'>",
  discoPaper:
    "<a href='https://tma.ifip.org/wordpress/wp-content/uploads/2017/06/tma2017_paper41.pdf' target='_blank'>"
  */
}

export default {
  name: 'HomeView',
  components: {
    Timeline,
  },
  props: {
    showSidebar: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      graphTypes: GRAPHS_TYPES,
      placeholderValues: PLACEHOLDER_VALUES,
    }
  },
}
</script>

<style lang="stylus" scoped>

#IHR_
  &home
    ~/global-report
      margin-bottom 60pt
      max-width 100%
      height 450px
      background-color black
      background-image url('~@/assets/imgs/global-banner.png')
      background-repeat no-repeat
      background-position right top
      background-size contain
      @media screen and (max-width: 1024px) {
        background-position center center
        background-size cover
      }

      &-button
        margin-top 40px
        border white solid 4px
        text-align center
        font-weight 500
        transition all 0.6s
        max-width 300pt
        display block
        color white
        font-size 2rem
        text-decoration none
        @media screen and (max-width: 600px)
          font-size 1.5rem
          margin-left auto
          margin-right auto
          max-width 200pt

        &:hover
          background-color white
          color black
          text-shadow 0 0 3px #FFFFFF

      & > div
        height 100%

        &:first-child
          color white
          font-size 50px
          text-align left
          padding-top 60pt
          padding-left 60pt
          display inline-block
          text-shadow 0 0 8px #000000;
          @media screen and (max-width: 600px)
            padding-top 100pt
            padding-left 0
            text-align center
            font-size 2rem

    ~/graphs-types
      width 100%

      & > div
        background-color white
        padding 1.6em
        border gray solid 1px

        > h2
          font-weight 400
          margin-bottom 4pt
          font-size 28pt
          @media screen and (max-width: 600px)
            font-size 22pt
          &:first-letter
            text-transform uppercase


.IHR_
  &description_text
    height 220px
.IHR_
  &description,
  &description_text
    font-size 18pt
    width 90%
    margin 30pt auto
    text-align left
    @media screen and (max-width: 600px)
        font-size 12pt
    #IHR_graphs-types &
      text-align justify

    &-main
      font-size 20pt
      margin 30pt auto
      text-align center
      width 85%
      @media screen and (max-width: 600px)
        font-size 14pt

    &-link
      position relative
      text-align right

  &section
    margin 60pt auto 40px auto
    width 85%
    font-size 18pt
    text-align center
    & > h2
      font-size 32pt
      @media screen and (max-width: 600px)
        font-size 22pt

  ~/ack-logo
      margin-left auto
      margin-right auto
      text-align center
      height 200px
      width 400px

      & > span
        display inline-block
        vertical-align middle
        height 100%

      & > img
        vertical-align middle
        max-height 100px
        max-width 300px
        width auto
        height auto

      & > img:hover {
        transform: scale(1.1);
      }  
        

.analysis-modules
    max-width 600px
    text-align left
    border-radius 15px

@media(max-width 1411px)
  .analysis-modules
    margin-left 0

@media(max-width 1450px)
  .IHR_tweets-types
        max-width 600px !important


.IHR_tweets-types
      margin-left auto
      margin-right auto
      max-width 800px
      text-align center
      border-radius 15px

      & > div
        background-color white

        > h2
          font-weight 400
          margin-bottom 4pt
          &:first-letter
            text-transform uppercase
</style>
