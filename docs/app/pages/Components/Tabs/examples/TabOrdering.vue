<template>
  <div>
    <h3>Demonstration of a previous bugs, a working solution with lots of edge-cases:</h3>
    <md-tabs :md-active-tab="mdActiveTab" @md-changed="events.push($event)">
      <template slot="md-tab" slot-scope="{ tab }">Tab {{ tab.label }}</template>
      <md-tab id="5" md-label="Label A id=5">
        Content A id=5
        <md-tabs>
          <md-tab id="s1" md-label="Sub-label 1">Sub-content 1</md-tab>
          <md-tab id="0" md-label="Sub-label 0">
            Sub-content 0 (same ID as in a parent tab)<br>
            Multi-line to show tab-in-tab height bug<br>
            Yet another line<br>
            ...to make its height big enough to highlight the bug when switching from tab D to B
          </md-tab>
        </md-tabs>
      </md-tab>
      <md-tab id="0" md-label="Label B id=0">Content B id=0 (only one line, but tab-bar height vary when switching from tab D)</md-tab>
      <md-tab id="C" md-label="Label C id=C" v-if="showMiddleTab">Content C id=C</md-tab>
      <md-tab id="3" md-label="Label D id=3 (default)">Content D id=3 (default)</md-tab>
      <md-tab id="text" md-label="Label E id=text">Content E id=text</md-tab>
      <md-tab :id="`_${dynamicId}`" :md-label="`Dynamic id=_${dynamicId}`">Content id=_{{ dynamicId }}</md-tab>
      <md-tab id="Z" md-label="Label Z id=Z" v-if="showLastTab">Content Z id=Z</md-tab>
    </md-tabs>
    <md-button class="md-primary" @click="showMiddleTab = !showMiddleTab">Toggle middle tab (C id=C)</md-button>
    <md-button class="md-primary" @click="showLastTab = !showLastTab">Toggle last tab (Z id=Z)</md-button>
    <md-button class="md-primary" @click="dynamicId++">Change dynamic ID</md-button>
    <div>
      <md-button class="md-primary" @click="mdActiveTab = '3'">Switch to tab "3"</md-button>
      <md-button class="md-primary" @click="mdActiveTab = 'text'">Switch to tab "text"</md-button>
      (they should trigger only one event)
      <md-button class="md-primary" @click="events = []">Reset events history</md-button>
      Events history: {{ events }}
    </div>

    <h3>Tabs without content should continue to work:</h3>
    <md-tabs>
      <md-tab id="9" md-label="9"><span v-if="withContent">Content 9!</span></md-tab>
      <md-tab id="0" md-label="0"><span v-if="withContent">Content 0!</span></md-tab>
    </md-tabs>
    <md-button class="md-primary" @click="withContent = !withContent">Toggle content</md-button>

    <h3>Tabs with partial content should work too (weird idea, but let's not crash if our user-code is weird):</h3>
    <md-tabs>
      <md-tab id="A" md-label="A (with content)">Content of A</md-tab>
      <md-tab id="9" md-label="9 (without content)" />
      <md-tab id="0" md-label="0 (with content)">Content of 0</md-tab>
    </md-tabs>

    <h3>Tabs without tabs should work too (clearly a bug in user-code, but let's not crash the library):</h3>
    <md-tabs />
  </div>
</template>

<script>
  export default {
    name: 'TabOrdering',

    data: () => ({
      mdActiveTab: "3",
      events: [],
      showMiddleTab: false,
      showLastTab: false,
      dynamicId: 100,
      withContent: false
    }),
  }
</script>
