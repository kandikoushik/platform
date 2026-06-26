<!--
// Copyright © 2023 Hardcore Engineering Inc.
//
// Licensed under the Eclipse Public License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License. You may
// obtain a copy of the License at https://www.eclipse.org/legal/epl-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
//
// See the License for the specific language governing permissions and
// limitations under the License.
-->
<script lang="ts">
  import { getMetadata } from '@hcengineering/platform'
  import presentation from '@hcengineering/presentation'
  import setting from '@hcengineering/setting'
  import { Component, Icon, Label, navFooterExtensions, showPopup } from '@hcengineering/ui'
  import workbench from '../plugin'
  import HelpAndSupport from './HelpAndSupport.svelte'

  export let split: boolean = false

  let selected: boolean = false

  const version = getMetadata(presentation.metadata.FrontVersion)

  $: extensions = [...$navFooterExtensions].sort((a, b) => a.order - b.order)
</script>

<div class="antiNav-footer-line" />
<div class="antiNav-footer-grower" />
<div class="antiNav-footer">
  <slot />
  {#each extensions as ext (ext.id)}
    <Component is={ext.component} props={ext.props ?? {}} />
  {/each}
  {#if split}<div class="antiNav-space" />{/if}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div
    class="antiNav-element dtsNav-help"
    class:selected
    on:click={() => {
      selected = true
      showPopup(HelpAndSupport, {}, 'help-center', () => {
        selected = false
      })
    }}
  >
    <div class="an-element__icon">
      <Icon icon={setting.icon.Support} size={'small'} />
    </div>
    <span class="an-element__label">
      <Label label={workbench.string.HelpAndSupport} />
    </span>
    {#if version}
      <span class="version-label">{version}</span>
    {/if}
  </div>
</div>

<style lang="scss">
  .dtsNav-help {
    border-radius: 0.5rem;
    transition: background-color 0.15s ease;
  }
  .version-label {
    margin-left: auto;
    padding: 0.0625rem 0.375rem;
    font-size: 0.6875rem;
    font-weight: 500;
    color: var(--theme-dark-color);
    background-color: var(--theme-navpanel-hovered);
    border-radius: 0.375rem;
    user-select: all;
  }
</style>
