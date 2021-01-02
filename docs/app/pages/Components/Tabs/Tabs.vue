<example src="./examples/TabOrdering.vue" />
<example src="./examples/TabRouter.vue" />
<example src="./examples/TabAlignments.vue" />
<example src="./examples/TabContent.vue" />
<example src="./examples/TabIcons.vue" />
<example src="./examples/TabCustomTemplate.vue" />

<template>
  <page-container centered :title="$t('pages.tabs.title')">
    <div class="page-container-section">
      <p>Tabs make it easy to explore, switch between different views and enable content organization at a high level, such as different data sets or functional aspects of an app.</p>
      <p>Tabs are really powerful and have deep integration with Vue Core features and router.</p>
      <note-block>More features for tabs will be come in the next weeks, like pagination scroll and touch events. :)</note-block>
    </div>

    <div class="page-container-section">
      <h2 id="numeric-bugs-showcase">Numeric bugs showcase</h2>

      <code-example title="Tab ordering with numeric ids or toggled tabs" :component="examples['tab-ordering']" />
      <p>This example showcases the bugs before the fix:</p>
      <ul>
        <li>Contents are always in the correct order, but their tabs are not</li>
        <li>Numeric-like IDs are placed at the start, ordered by number</li>
        <li>Clicking a tab label would then show the content of another tab</li>
        <li>When toggling a tab, it is appended at the end, and not in the place it appears in HTML template: click "Tab Label A id=5", then show the middle tab: the displayed tab contents changes</li>
        <li>When a new tab appears, the currently selected one must remain selected, even if it is after the new one</li>
        <li>Note to myself: be careful while fixing the bugs: tabs of a sub-tab-bar should not be merged with current tab-bar</li>
        <li>Toggle last tab: if it was selected, the previous tab must become selected instead (instead of selecting nothing)</li>
        <li>Also make sure dynamic IDs are working</li>
      </ul>
      <p>I also went into a few existing bugs I fixed:</p>
      <ul>
        <li>
          In the first tab, select the second sub-tab: its height is big.
          Now select the third tab and then the second tab: their height is the same (one line), but the big height of the second sub-tab is used for the second tab.</li>
        <li>Click one of the two "Switch to tab..." buttons: two md-change events are triggered.</li>
      </ul>
    </div>

    <div class="page-container-section">
      <h2 id="navigational-tabs">Navigational tabs</h2>

      <p>Sometimes you may need a tab to be the main navigational element of your application and you can do this. Tabs integrate with Vue Router by default and will be able to use single tab just like a regular button or link, by using the <code>router-link</code> props. The tabs will sync with the page URL and will produce effects when transitioning between tabs. <strong>AUTOMATIC</strong>!</p>
      <code-example title="Seamless integration with Vue Router" :component="examples['tab-router']" />
    </div>

    <div class="page-container-section">
      <h2 id="tab-inner">Tab with inner content</h2>

      <p>In the previous example, the tabs worked just like navigation buttons, without content. With that you would need to render the content by yourself. Although this is not a hard thing, because you can use Vue Router, you can pass arbitrary content to your tabs. And it can also work syncing with router:</p>
      <code-example title="Content syncing with Router" :component="examples['tab-content']" />
    </div>

    <div class="page-container-section">
      <h2 id="tab-alignments">Alignments</h2>

      <p>Tabs have four types of alignments for the navigation buttons: Left, Center, Right and Fixed. You can use them with any tabs:</p>
      <code-example title="With different hue colors" :component="examples['tab-alignments']" />
    </div>

    <div class="page-container-section">
      <h2 id="tab-icons">Icons</h2>

      <p>Tabs accept icons, to make it easier for your user to assimilate the contents of a tab:</p>
      <code-example title="With svg support" :component="examples['tab-icons']" />
    </div>

    <div class="page-container-section">
      <h2 id="tab-custom-template">Custom Template</h2>

      <p>You can use a custom template for the navigation buttons. This will be applied to all navigation buttons and allows you to make updates on your tab, like this great example of unread/new content: Simple, uh?</p>
      <code-example title="Template Slot" :component="examples['tab-custom-template']" />

      <api-item title="API - md-tabs">
        <p>The following options can be applied to any tabs:</p>

        <api-table :headings="tabs.props.headings" :props="tabs.props.props" slot="props" />
        <api-table :headings="tabs.slots.headings" :props="tabs.slots.props" slot="scoped-slots" />
        <api-table :headings="tabs.events.headings" :props="tabs.events.props" slot="events" />
      </api-item>

      <api-item title="API - md-tab">
        <p>The following options can be applied to any tab.  All <a href="https://router.vuejs.org/en/api/router-link.html" target="_blank">options</a> of <code>router-link</code> can be simply used here:</p>

        <api-table :headings="tab.headings" :props="tab.props" slot="props" />
      </api-item>

      <api-item title="API - Swipeable">
        <p>The following options can be applied to any <code>md-tabs</code> component that is using <code>md-swipeable</code> prop.</p>

        <api-table :headings="swipeable.props.headings" :props="swipeable.props.props" slot="props" />
      </api-item>
    </div>
  </page-container>
</template>

<script>
  import examples from 'docs-mixins/docsExample'

  export default {
    name: 'DocTabs',
    mixins: [examples],
    data: () => ({
      swipeable: {
        props: {
          headings: ['Name', 'Description', 'Default'],
          props: [
            {
              name: 'md-swipe-threshold',
              type: 'Number',
              description: 'The minimal distance traveled to be considered swipe.',
              defaults: '50'
            },
            {
              name: 'md-swipe-restraint',
              type: 'Number',
              description: 'The maximum distance allowed at the same time in perpendicular direction.',
              defaults: '100'
            },
            {
              name: 'md-swipe-time',
              type: 'Number',
              description: 'The maximum time allowed to detect swipe.',
              defaults: '400'
            },
          ]
        }
      },
      tabs: {
        props: {
          headings: ['Name', 'Description', 'Default'],
          props: [
            {
              name: 'md-active-tab',
              type: 'String|Number',
              description: 'Set the current selected tab. Works by providing the id of the desired <code>md-tab</code>.',
              defaults: 'null'
            },
            {
              name: 'md-swipeable',
              type: 'Boolean',
              description: 'Option used to enable touch support to move between tabs by swipe. For more option see  API - Swipeable',
              defaults: 'false'
            },
            {
              name: 'md-sync-route',
              type: 'Boolean',
              description: 'Syncs the tab selection with the current route, matching by the single tab <code>to</code> prop.',
              defaults: 'false'
            },
            {
              name: 'md-alignment',
              type: 'String',
              description: 'Sets the tab navigation alignment. See below the detailed description of each layout.',
              defaults: 'left'
            },
            {
              offset: true,
              name: 'md-alignment="left"',
              type: 'String',
              description: 'Align the tabs navigation buttons to left',
              defaults: '-'
            },
            {
              offset: true,
              name: 'md-alignment="center"',
              type: 'String',
              description: 'Align the tabs navigation buttons on center',
              defaults: '-'
            },
            {
              offset: true,
              name: 'md-alignment="right"',
              type: 'String',
              description: 'Align the tabs navigation buttons on right',
              defaults: '-'
            },
            {
              offset: true,
              name: 'md-alignment="fixed"',
              type: 'String',
              description: 'Make the navigation buttons to fill all the available space.',
              defaults: '-'
            },
            {
              name: 'md-dynamic-height',
              type: 'Boolean',
              description: 'Apply a dynamic transition to the table height. Be careful with this option to not hurt the performance of your page.',
              defaults: 'false'
            },
            {
              name: 'md-elevation',
              type: 'Number',
              description: 'Add an elevation to tab navigation element.',
              defaults: '0'
            },
            {
              name: 'md-is-rtl',
              type: 'Boolean',
              description: 'Set the RTL support',
              defaults: 'false'
            }
          ]
        },
        events: {
          headings: ['Name', 'Description', 'Value'],
          props: [
            {
              name: 'md-changed',
              description: 'Triggered when the active tab changes',
              value: 'Tab ID'
            }
          ]
        },
        slots: {
          headings: ['Name', 'Description', 'Values'],
          props: [
            {
              name: 'md-tab',
              description: 'Creates a custom tab button template',
              options: [
                {
                  name: 'tab',
                  description: 'The options that were passed through <code>md-template-data</code> inside each <code>md-tab</code>'
                }
              ],
              usage: '<template slot="md-tab" slot-scope="{ tab }"> ... </template>'
            }
          ]
        },
      },
      tab: {
        headings: ['Name', 'Description', 'Default'],
        props: [
          {
            name: 'id',
            type: 'String',
            description: 'The tab id. Used when changing the active tab dynamically',
            defaults: 'a random string'
          },
          {
            name: 'href',
            type: 'String',
            description: 'The tab href link. Useful when you don\'t have Vue Router on your app, but you still want to change the current URL based on the tab.',
            defaults: 'null'
          },
          {
            name: 'md-label',
            type: 'String',
            description: 'The tab label',
            defaults: 'null'
          },
          {
            name: 'md-icon',
            type: 'String',
            description: 'The tab icon. Accepts an string or a asset URL. The tab will know how to handle the icon based on this.',
            defaults: 'null'
          },
          {
            name: 'md-disabled',
            type: 'Boolean',
            description: 'Disable/enable a tab',
            defaults: 'null'
          },
          {
            name: 'md-template-data',
            type: 'Object',
            description: 'The data to be passed to the respective tab button, when using custom template.',
            defaults: 'null'
          }
        ]
      }
    })
  }
</script>
