<template>
  <div id="app">
    <!-- 
      Dynamic menu component reactively handles all flyout and context menus.
      https://github.com/Inventsable/brutalism/tree/master/components/Menus
     -->
    <Menus
      refresh
      :context="[
        {
          label: 'Learn more',
          enabled: false,
        },
        {
          label: 'Log menu item with callback',
          checkable: true,
          checked: true,
          callback: checkMenu,
        },
        {
          label: 'Test evalScript',
          callback: runTestScript,
        },
        {
          label: 'Supporting infinite nesting!',
          menu: [
            {
              label: 'Hello',
              menu: [{ label: 'World' }],
            },
          ],
        },
      ]"
      @contextClick="testClick"
      :flyout="[
        {
          label: 'This flyout menu has a JSON structure!',
        },
      ]"
    />
    <!-- 
      Panel is a special wrapper meant for Adobe hosts to handle style, script loading, scrollbars and more.
      For best results always use it as the parent of any content or a router-view.
      https://github.com/Inventsable/brutalism/tree/master/components/Panel
    -->
    <Panel>
      <router-view />
    </Panel>

    <!-- 
      Tabs can easily handle Vue Router navigation for you. "invert" places Tabs at bottom,
      and the routes Array corresponds to the one in ./src/routes.js.
     -->
    <Tabs invert :routes="routes" />
  </div>
</template>

<script>
/*
  Panel component above also includes:
    - Starlette UI theme and color library: 
      https://github.com/Inventsable/starlette
    - CEP-Spy identification and app utility:
      https://github.com/Inventsable/cep-spy
    - Cluecumber script utilities (passed through brutalism):
      https://github.com/Inventsable/cluecumber
 These are still installed into this panel and can be used whenever needed:
 import spy from 'cep-spy'

 NOTES: 
  - Brutalism's main components are made globally available in ./src/main.js. There's no need to import them individually!
  - Need CSInterface or a script? You can use the script-path attribute of Panel to launch scripts or utilities:
    https://github.com/Inventsable/lokney/tree/master/components/Panel
  - Need Chrome DevTools for debugging? You'll need to launch the CEFClient from here with the localhost:[port] of this app in ./.debug:
    https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_9.x
*/

import { evalScript } from "brutalism";
export default {
  data: () => ({
    routes: [
      // label: What displays on the Tab
      // name/path: the target in ./src/router.js
      { label: "Home", name: "home" },
      { label: "About", path: "/about" },
    ],
  }),
  methods: {
    testClick(item) {
      console.log("Context menu click:", item);
    },
    checkMenu(item, index, val) {
      console.log(item, index, val);
    },
    // Can invoke any function as await evalScript(`functionName('${parameterVar}')`) if script is preloaded
    // Check out the "script-path" prop of <Panel> component above for easy script file load.
    async runTestScript() {
      let result = await evalScript(`
        function test() {
          alert('Hello world!')
          return 'result from JSX file'
        }
        test();
      `);
      console.log(result);
    },
  },
};
</script>

<style>
</style>
