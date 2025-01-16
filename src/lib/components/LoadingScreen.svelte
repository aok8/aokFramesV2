<script lang="ts">
  import { onMount, createEventDispatcher } from 'svelte';
  import { fade } from 'svelte/transition';
  import anime from 'animejs';

  const dispatch = createEventDispatcher();
  const { isLoading = true } = $props<{ isLoading?: boolean }>();

  onMount(() => {
    const svg = document.querySelector('#logo');
    if (!svg) return;

    // Add a small delay to ensure SVG is fully mounted
    setTimeout(() => {
      const desiredTotalDuration = 4500;
      const originalDurations = [
        500, 100, 200, 200, 250, 500, 200, 200, 200, 300, 300, 200, 200, 200, 300, 300, 550, 400, 400, 300, 500, 500, 400, 300, 300
      ];
      const totalOriginalDuration = originalDurations.reduce((acc, duration) => acc + duration, 0);
      const scalingFactor = desiredTotalDuration / totalOriginalDuration;
      const scaledDurations = originalDurations.map(duration => duration * scalingFactor);

      const timeline = anime.timeline({
        easing: 'easeInOutSine',
      });

      // Initial fade in
      timeline.add({
        targets: '#logo',
        opacity: [0, 1],
        duration: scaledDurations[0],
        easing: 'linear'
      });

      // Debug: Log all paths to ensure they exist
      const paths = [
        '#a11', '#a12', '#a13', '#a2', '#o', '#k11', '#k12', '#k13', '#k2', '#k3',
        '#f1', '#f21', '#f22', '#f3', '#rames1', '#rames2', '#rames3', '#rames4',
        '#rames5', '#rames6', '#rames7', '#rames8', '#rames9'
      ];

      paths.forEach(path => {
        const element = document.querySelector(`#logo ${path}`);
        console.log(`Path ${path} exists:`, !!element);
      });

      // Animate each path
      paths.forEach((path, index) => {
        timeline.add({
          easing: 'easeInOutSine',
          targets: `#logo ${path}`,
          strokeDashoffset: {
            value: [anime.setDashoffset, 0],
            duration: scaledDurations[index + 1]
          },
          begin: () => console.log(`Starting animation for ${path}`),
          complete: () => console.log(`Completed animation for ${path}`)
        }, index === 0 ? '-=50' : (index % 3 === 0 ? '+=50' : '-=50'));
      });

      // Add completion callback
      timeline.finished.then(() => {
        console.log('Animation timeline completed');
        // Dispatch event when animation is complete
        setTimeout(() => {
          dispatch('animationComplete');
        }, 500);
      });
    }, 100);
  });
</script>

{#if isLoading}
  <div 
    class="fixed inset-0 bg-black z-50 flex items-center justify-center"
    transition:fade={{ duration: 800 }}
  >
    <svg
      id="logo"
      style="opacity: 0"
      class="w-[400px]"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 201.21962 50"
    >
      <g id="layer1" transform="translate(-4.5442666,-85.597857)">
        <g id="A1">
          <path id="a11" class="path" d="m 5.2487297,120.7798 c 0.920499,-4.9881 5.3845943,-12.51021 7.7028043,-16.8771" />
          <path id="a12" class="path" d="m 12.420291,104.28691 c 9.459945,-12.614952 5.171895,-9.613475 13.82693,-17.626994" />
          <path id="a13" class="path" d="m 28.817732,83.154398 c -1.638971,0.02441 -11.749233,31.123382 -6.725435,33.174602 3.503751,1.43058 7.664337,-6.07289 7.664337,-6.07289" />
        </g>
        <path id="a2" class="path" d="m 4.0345415,103.40088 c 0.1877119,0.0683 12.1583055,0.27743 18.5647355,2.28375" />
        <path id="o" class="path" d="m 50.674589,84.489621 c 0.237675,-2.376759 -0.04097,1.600475 -2.40118,1.431471 -2.499235,-0.178959 -13.995601,11.778102 -14.545597,19.117068 -0.514465,6.86486 0.461765,10.25119 0.461765,10.25119 1.171531,1.17153 5.359476,8.07665 13.991479,-0.96971 11.472558,-12.02327 7.763064,-24.287686 5.264122,-24.93531 -2.189767,-0.567499 -8.376533,7.677413 -7.422106,10.194925" />
        <g id="K">
          <g id="K1">
            <path id="k11" class="path" d="m 61.622817,97.039102 c 0,0 1.150012,-8.901538 4.520896,-10.286833 3.370885,-1.385294 4.155888,0.554118 4.155888,0.554118 L 67.021069,97.41904" />
            <path id="k12" class="path" d="M 67.6205,95.789186 63.213485,109.23671" />
            <path id="k13" class="path" d="m 63.604007,108.27052 -4.819957,12.9356" />
          </g>
          <path id="k2" class="path" d="m 66.143713,101.39022 c 0,0 16.746698,-12.314199 17.808756,-15.59273" />
          <path id="k3" class="path" d="m 66.005186,102.2214 c 0,0 12.666898,20.05954 15.206607,19.36689" />
        </g>
        <g id="F">
          <path id="f1" class="path" d="m 98.564018,97.495129 c 0,0 -2.821169,-4.324326 -1.112638,-6.725504 1.708531,-2.401178 7.73804,-3.736399 11.28205,-3.987444 3.15326,-0.223366 9.31798,2.322842 11.95971,0.09235" />
          <g id="F2">
            <path id="f21" class="path" d="m 108.53374,87.029329 c 0.84659,-1.594965 -5.47733,13.810141 -7.34206,20.917951" />
            <path id="f22" class="path" d="m 102.76257,104.22147 -4.757073,17.53259" />
          </g>
          <path id="f3" class="path" d="m 94.588438,103.0064 19.209422,-1.47765" />
        </g>
        <g id="rames">
          <path id="rames1" class="path" d="m 113.5208,112.05699 c 0,0 6.64941,-13.85295 4.80235,-12.744714 -1.84706,1.108234 -1.65004,3.917544 -1.65004,3.917544" />
          <path id="rames2" class="path" d="m 117.72286,102.63698 c 0,0 2.49183,2.49513 3.69412,3.78647 1.24676,1.33912 -2.17029,8.31177 -2.17029,8.31177 0,0 0.51566,5.41778 5.12559,1.29295 4.70886,-4.21335 8.08798,-10.06293 8.26559,-9.74324 0,0 9.08798,-6.20437 9.41121,-2.41789 0.32324,3.78647 2.3533,2.87659 2.3533,2.87659" />
          <path id="rames3" class="path" d="m 130.65228,108.22434 c 0.5563,-0.0795 -3.03806,6.37758 -0.87735,8.91206 2.2667,2.65881 9.88502,-9.83004 9.4908,-9.43582" />
          <path id="rames4" class="path" d="m 142.41146,108.16981 c 0,0 -5.38692,5.48565 -3.863,7.53512 2.61542,3.5174 7.40375,-3.1876 12.60619,-11.22088" />
          <path id="rames5" class="path" d="m 150.92376,103.51434 -3.60177,13.57589" />
          <path id="rames6" class="path" d="m 147.06956,115.09188 c 0,0 6.54382,-10.80677 12.65235,-11.12853 2.08353,-0.10974 0.60352,6.0299 -2.70285,13.95805 -3.13127,7.5083 6.38829,-10.74847 11.22089,-12.32912 2.37115,-0.77556 -1.38596,9.69133 -0.6003,10.89765 3.02555,0.70254 4.2886,-0.0446 5.55439,-1.38573" />
          <path id="rames7" class="path" d="m 173.04231,115.24317 c 0.65508,-0.94623 8.36968,-12.44871 9.00441,-12.60619 3.04496,-0.75549 5.58736,-0.27705 5.58736,-0.27705 0,0 -0.55147,6.34325 -0.46177,3.50941 0.13335,-4.21286 -2.87664,6.90695 -10.20499,6.46471 -0.93217,-0.0563 -1.79418,4.30388 3.04764,5.67971 5.46342,1.55246 7.20091,-3.73768 7.20091,-3.73768" />
          <path id="rames8" class="path" d="m 187.31085,114.73523 c 0.4106,-0.34689 8.50995,-9.47932 8.86588,-11.12854" />
          <path id="rames9" class="path" d="m 197.56203,100.09728 c 0,0 -1.01588,2.40118 -0.41559,3.46323 0.60029,1.06206 2.95529,9.88177 1.47765,11.77501 -1.47765,1.89324 -3.50942,4.57147 -7.4806,2.1703 -3.97118,-2.40118 0,-7.24971 0,-7.24971 0,0 1.61618,3.64794 4.84854,4.01735 3.23235,0.36941 2.49353,0.73883 2.49353,0.73883 0,0 0.785,1.84706 3.64794,-0.87736 2.86294,-2.72441 3.50941,-3.925 3.50941,-3.925" />
        </g>
      </g>
    </svg>
  </div>
{/if}

<style>
  :global(#logo) {
    max-width: 90vw;
  }

  :global(.path) {
    stroke-linecap: round;
    stroke-linejoin: round;
    fill: none;
    stroke: currentColor;
    stroke-width: 4;
  }
</style>
