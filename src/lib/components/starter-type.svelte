<script>
  export let starter, key
  import { createEventDispatcher } from 'svelte'

  import { Wrapper as SettingWrapper } from '$lib/components/Settings'
  import { color } from '$lib/data/colors.ts'
  import Icon from '@iconify/svelte/dist/OfflineIcon.svelte'

  import { PIcon } from '$c/core'
  import { Fire, Water, Grass } from '$icons'

  let starters = [
    { name: 'fire', icon: Fire },
    { name: 'water', icon: Water },
    { name: 'grass', icon: Grass }
  ]

  const minimal = key.startsWith('unbound')

  if (key.startsWith('unbound')) {
    starters = [
      { col: 'dragon', name: 'fire', picon: 'gible' },
      { col: 'steel', name: 'water', picon: 'beldum' },
      { col: 'ground', name: 'grass', picon: 'larvitar' }
    ]
  }

  if (key.startsWith('blazingem')) {
    starters = [
      { name: 'fire', picon: 'torchic' },
      { name: 'water', picon: 'mudkip' },
      { name: 'grass', picon: 'treecko' }
    ]
  }

  if (key.startsWith('newgen')) {
    starters = [
      { col: 'dragon', name: 'grass', picon: 'rattata' },
      { col: 'rock', name: 'fire', picon: 'sentret' },
      { col: 'dark', name: 'water', picon: 'zigzagoon' }
    ]
  }

  const dispatch = createEventDispatcher()
  const setstarter = (value) => (_) => dispatch('select', { value })
</script>

<SettingWrapper id="theme" let:setting={themeId}>
  {#each starters as s, i}
    <label
      class="flex h-6 w-6 cursor-pointer items-center justify-center rounded-full border-2 p-1 transition"
      class:opacity-50={starter !== s.name}
      class:hover:opacity-75={starter !== s.name}
      class:opacity-100={starter === s.name}
      class:border-gray-700={minimal}
      class:bg-gray-700={minimal}
      style="{starter === s.name
        ? 'background-color: ' + color(s.col || s.name, themeId)
        : ''}; {!minimal || starter === s.name
        ? 'border-color: ' + color(s.col || s.name, themeId)
        : ''}"
    >
      <input
        type="radio"
        on:click={setstarter(s.name)}
        name="starter"
        value={s.name}
      />
      {#if s.icon}
        <Icon
          inline={true}
          icon={s.icon}
          color={starter === s.name ? 'white' : color(s.col || s.name, themeId)}
        />
      {/if}
      {#if s.picon}
        <PIcon class="h-8 w-8 mix-blend-luminosity" name={s.picon} />
      {/if}
    </label>
  {/each}
</SettingWrapper>

<style lang="postcss">
  input {
    display: none;
  }
</style>
