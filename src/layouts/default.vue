<template>
  <q-layout view="lHh Lpr lFf">
    <q-layout-header>
      <q-toolbar color="blue-grey-8">
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
        >
          <q-icon name="menu"/>
        </q-btn>
        <q-toolbar-title>
          Gekko UI
        </q-toolbar-title>

        <q-toolbar-title shrink v-if="currentWatchers.length > 0" class="text-right">Prices: <span slot="subtitle">(from Watchers)</span></q-toolbar-title>
        <q-toolbar-title shrink v-for="(w,idx) in currentWatchers" :key="w.id" v-if="w.lastCandle">
          {{w.lastCandle.close}}
          <span class="text-amber">
              {{w.watch.currency}}
            </span>
          <span slot="subtitle">
              {{w.watch.exchange}}
              <span class="text-amber-4">{{w.watch.asset}}
              </span>
            </span>
        </q-toolbar-title>

      </q-toolbar>
      <q-tabs align="justify" color="blue-grey-7">
        <q-route-tab slot="title" default icon="home" label="Home" to="/" exact></q-route-tab>
        <q-route-tab slot="title" icon="cast connected" label="Live Gekkos" to="/live-gekkos" exact
                     :count="$store.state.stratrunners.stratrunners.length"></q-route-tab>
        <q-route-tab slot="title" icon="timeline" label="Backtest" to="/backtest" exact></q-route-tab>
        <q-route-tab slot="title" icon="storage" label="Data" to="/data" exact></q-route-tab>
        <q-route-tab slot="title" icon="import export" label="Importer" to="/data/importer" exact
                     :count="activeImports"></q-route-tab>
        <q-route-tab slot="title" icon="settings" label="Config" to="/config" exact></q-route-tab>
        <q-route-tab slot="title" icon="help" label="Documentation" to="/help"></q-route-tab>
      </q-tabs>
    </q-layout-header>

    <q-layout-drawer
      v-model="leftDrawerOpen"
      content-class="bg-grey-2"
    >
      <q-list
        no-border
        link
        inset-delimiter
      >
        <q-list-header>Essential Links</q-list-header>
        <q-item @click.native="openURL('https://github.com/h256/gekko-quasar-ui')">
          <q-item-side icon="code"/>
          <q-item-main label="Gekko-Quasar-UI on Github" sublabel="github.com/h256/gekko-quasar-ui"/>
        </q-item>
        <q-item @click.native="openURL('https://github.com/askmike/gekko')">
          <q-item-side icon="code"/>
          <q-item-main label="Gekko on GitHub" sublabel="github.com/askmike/gekko"/>
        </q-item>
        <q-item @click.native="openURL('https://discord.gg/26wMygt')">
          <q-item-side icon="chat"/>
          <q-item-main label="Gekko Discord Channel" sublabel="https://discord.gg/26wMygt"/>
        </q-item>
        <q-item @click.native="openURL('https://forum.gekko.wizb.it/')">
          <q-item-side icon="record_voice_over"/>
          <q-item-main label="Gekko Forum" sublabel="https://forum.gekko.wizb.it/"/>
        </q-item>
        <q-item-separator></q-item-separator>
        <q-list-header>Unofficial resources (use at own risk!)</q-list-header>
        <q-item @click.native="openURL('https://github.com/gekkowarez/gekkoga')">
          <q-item-side icon="code"/>
          <q-item-main label="Gekko-GA " sublabel="Genetic algorithm for gekko"/>
        </q-item>
        <q-item @click.native="openURL('https://github.com/Gab0/japonicus')">
          <q-item-side icon="code"/>
          <q-item-main label="Japonicus" sublabel="Genetic algorithm backtester for gekko"/>
        </q-item>
        <q-item-separator/>
        <q-list-header>Quasar Framework resources</q-list-header>
        <q-item @click.native="openURL('http://quasar-framework.org')">
          <q-item-side icon="school"/>
          <q-item-main label="Quasar Framework" sublabel="quasar-framework.org"/>
        </q-item>
      </q-list>
    </q-layout-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>

    <q-layout-footer>
      <q-toolbar color="blue-grey-7">
        <q-toolbar-title>
          Gekko Material v {{version.ui}}
          <div slot="subtitle">Running on Quasar v{{ $q.version }}</div>
        </q-toolbar-title>
        <q-toolbar-title>
          <em>Use Gekko at your own risk!</em>
          <!--<div slot="subtitle">Using Gekko v {{version.gekko}}</div>-->
        </q-toolbar-title>
      </q-toolbar>
    </q-layout-footer>
  </q-layout>
</template>

<script>
  import {openURL} from "quasar";
  import _ from 'lodash'
  //TODO: Get Real Version from API or from Folder...
  //const gekkoPackage = {version:'0.5.13'};//require('../../../../../package.json');
  const uiPackage = require('../../package.json');

  export default {
    name: "MainLayoutPage",
    data() {
      return {
        leftDrawerOpen: false,
        version: {
          //gekko: gekkoPackage.version,
          ui: uiPackage.version
        }
      };
    },
    computed: {
      activeImports: function () {
        return _.filter(this.$store.state.imports.imports, function (item) {
          return !item.done
        }).length;
      },
      currentWatchers: function () {
        return _.slice(this.$store.getters['watchers/watchers'], 0, 10);
      }
    },
    methods: {
      openURL
    }
  };
</script>

<style>

</style>
