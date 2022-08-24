

<template>
  <div class="vue-flow-content">
    <div>
      <button >获取code</button>
      <button >同步代码</button>
      <div id="blocklyDiv" style="height: 800px; width: 100%;"></div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import type { AppContext } from "@netless/window-manager";
import Blockly from 'blockly';
import * as Ch from 'blockly/msg/zh-hans';

//根据使用情况，引入相关组件
import type { Elements } from "@braks/vue-flow";

import { computed, inject, onMounted, ref, reactive, watchEffect } from "vue";
Blockly.setLocale(Ch);
const elementsDefault = `
          <xml  style="height: 100%">
               <category name="Logic" categorystyle="logic_category">
                    <block type="controls_if"></block>
                    <block type="logic_compare"></block>
                    <block type="logic_operation"></block>
                    <block type="logic_negate"></block>
                    <block type="logic_boolean"></block>
                    <block type="logic_null" disabled="true"></block>
                    <block type="logic_ternary"></block>
                    <block type="text_print"></block>
                </category>
                <category name="Math" categorystyle="math_category">
                      <block type="math_number" gap="32"></block>
                </category>
            </xml>
         `

const elements = ref(elementsDefault);

const context = inject<AppContext>("context");
if (!context) throw new Error("must call provide('context') before mount App");

const storage = context.createStorage("mess", { str: elements.value });

const messList = ref(storage.state.str);
console.log(elements, elementsDefault, messList, 'elementsDefaultelementsDefaultelementsDefault')
// const  getJavascriptCode () {
//     let  code = Blockly.JavaScript.workspaceToCode(this.workspace);
//     console.log('--------Javascript code-------');
//     console.log(code)
//   },
//   getWorkspaceXml () {
//     var xml = Blockly.Xml.domToText(Blockly.Xml.workspaceToDom(this.workspace))
//     localStorage.setItem('workspaceXml', xml)
//     console.log('--------workspace xml-------');
//     console.log(xml);
//     console.log('当前工作区 xml 已保存到 localStorage 中...');
//   },
// getLocalStorageXml () {
//   let xml = localStorage.getItem('workspaceXml');
//   Blockly.Xml.domToWorkspace(Blockly.Xml.textToDom(xml), this.workspace)
// }

onMounted(() => {
  const workspace = Blockly.inject('blocklyDiv', { toolbox: messList.value });
  // if (localStorage.getItem('workspaceXml')) {
  //   this.getLocalStorageXml();
  // }
  storage.addStateChangedListener(() => {
    messList.value = storage.state.str;
  });
});

watchEffect(() => {

});
</script>

<style scoped>
.vue-flow-content {
  width: 800px;
  height: 500px;
}
</style>