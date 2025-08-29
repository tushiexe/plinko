<script lang="ts">
  import { balance } from '$lib/stores/game';
  import { flyAndScale } from '$lib/utils/transitions';
  import { Popover } from 'bits-ui';

  let balanceFormatted = $derived(
    $balance.toLocaleString('en-US', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2,
    }),
  );

  const addMoneyAmounts = [100, 500, 1000];
</script>

<div class="flex overflow-hidden rounded-lg shadow-lg">
  <div
    class="flex gap-2 bg-black px-4 py-2 text-sm font-semibold text-white tabular-nums sm:text-base border-r border-purple-600"
  >
    <span class="text-red-400 select-none">$</span>
    <span class="min-w-16 text-right">
      {balanceFormatted}
    </span>
  </div>
  <Popover.Root>
    <Popover.Trigger
      class="bg-gradient-to-r from-purple-600 to-purple-700 px-4 py-2 text-sm font-medium text-white transition-all duration-200 hover:from-purple-500 hover:to-purple-600 active:from-purple-700 active:to-purple-800 sm:text-base"
    >
      Add
    </Popover.Trigger>
    <Popover.Content
      forceMount
      sideOffset={8}
      class="z-30 max-w-lg space-y-2 rounded-md bg-slate-600 p-3"
    >
      {#snippet child({ wrapperProps, props, open })}
        {#if open}
          <div {...wrapperProps}>
            <div {...props} transition:flyAndScale>
              <p class="text-sm font-medium text-gray-200">Add money</p>
              <div class="flex gap-2">
                {#each addMoneyAmounts as amount}
                  <button
                    onclick={() => ($balance += amount)}
                    class="touch-manipulation rounded-md bg-gradient-to-r from-purple-500 to-purple-600 px-3 py-2 text-sm font-semibold text-white transition-all duration-200 hover:from-purple-400 hover:to-purple-500 active:from-purple-600 active:to-purple-700 shadow-sm hover:shadow-md"
                  >
                    +${amount}
                  </button>
                {/each}
              </div>
            </div>
          </div>
        {/if}
      {/snippet}
    </Popover.Content>
  </Popover.Root>
</div>
