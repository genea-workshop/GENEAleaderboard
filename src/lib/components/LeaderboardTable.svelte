<script lang="ts">
  import type { LeaderboardRow } from '$lib/types';

  export let rows: LeaderboardRow[] = [];
  export let limit: number | null = null;
  export let columns: (keyof LeaderboardRow)[] = [
    'rank', 'name', 'fgd', 'fd_g', 'fd_k', 'ba', 'srgr', 'div_pose', 'div_sample', 'paper_venue', 'elo_hl', 'mismatch'
  ];
  let sortKey: keyof LeaderboardRow | null = null;
  let ascending = true;
  let q = '';
  export let showSearch: boolean = true;

  function sortBy(key: keyof LeaderboardRow) {
    if (sortKey === key) {
      ascending = !ascending;
    } else {
      sortKey = key;
      ascending = true;
    }
  }

  $: sortedRows = [...rows]
  .filter(r =>
      [r.name].some(v => (v || '').toLowerCase().includes(q.toLowerCase()))
    )
  .sort((a, b) => {
    if (!sortKey) return 0;

    const valueA = a[sortKey];
    const valueB = b[sortKey];

    if (typeof valueA === 'number' && typeof valueB === 'number') {
      return ascending ? valueA - valueB : valueB - valueA;
    }

    const strA = String(valueA).toLowerCase();
    const strB = String(valueB).toLowerCase();

    if (strA < strB) return ascending ? -1 : 1;
    if (strA > strB) return ascending ? 1 : -1;
    return 0;
  });

  $: displayedRows = limit ? sortedRows.slice(0, limit) : sortedRows;

  const columnInfo: Record<string, { label: string; tooltip: string }> = {
  rank: { label: '#', tooltip: 'Ranking position' },
  name: { label: 'Name', tooltip: 'Model or participant name' },
  fgd: { label: 'FGD', tooltip: 'Frechet Gesture Distance' },
  fd_g: { label: 'FD-G', tooltip: 'Frechet Distance (Gestures)' },
  fd_k: { label: 'FD-K', tooltip: 'Frechet Distance (Keypoints)' },
  ba: { label: 'BA', tooltip: 'Beat Alignment score' },
  srgr: { label: 'SRGR', tooltip: 'Speech-to-Gesture Relevance' },
  div_pose: { label: 'Div-P', tooltip: 'Pose Diversity' },
  div_sample: { label: 'Div-S', tooltip: 'Sample Diversity' },
  paper_venue: { label: 'Venue', tooltip: 'Publication venue' },
  elo_hl: { label: 'Elo-HL', tooltip: 'Human-likeness score from Elo rating' },
  mismatch: { label: 'MM', tooltip: 'Mismatch preference' }
};
</script>

{#if showSearch}
    <div class="leaderboard-table-search">
        <input class="leaderboard-search" placeholder="Search..." bind:value={q} />
    </div>
{/if}
<table class="leaderboard-table-inner">
<thead>
    <tr>
        {#each columns as col}
        <th title={columnInfo[col]?.tooltip} class="" on:click={() => sortBy(col)}>
          <span class="sort-arrow">
            <text class="leaderboard-header-text">{col.charAt(0).toUpperCase() + col.slice(1)}</text> {sortKey === col ? (ascending ? '▲' : '▼') : '-'}
          </span>
        </th>
      {/each}
    </tr>
</thead>
<tbody>
    {#each displayedRows as row, i}
    <tr>
        {#each columns as col}
        <td   
            class:name={['id', 'model', 'team', 'submitted'].includes(col)}
            class:score={['val_f1', 'val_mse'].includes(col)}
        >{row[col]}</td>
        {/each}
    </tr>
    {/each}
</tbody>
</table>