<script context="module">

    import ipregistry from '@ipregistry/client'

    const API_KEY = 'tryout';
    const client = new ipregistry.IpregistryClient(API_KEY, new ipregistry.DefaultCache(16384));

    export function lookupIp(ip, filters = 'ip,hostname,location') {
        return client.lookup(ip,
            ipregistry.IpregistryOptions.hostname(true),
            ipregistry.IpregistryOptions.filter(filters))
    }

    export async function preload(page, session) {
        const ipInfo = await lookupIp('88.8.8.1');
        return { ipInfo };
    }

</script>

<script>
	export let ipInfo;

    function randomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    }

    function randomLookup() {
        lookupIp(
            randomInt(1,223) + '.' +
            randomInt(1,255) + '.' +
            randomInt(1,255) + '.' +
            randomInt(1,255)).then(function (data) {

            ipInfo = data;
        });
    }
</script>

<svelte:head>
	<title>Ipregistry Invoked from Client Side</title>
</svelte:head>

<h1>Ipregistry Test</h1>
<button on:click="{randomLookup}">Random Lookup</button>
<p>{JSON.stringify(ipInfo)}</p>
