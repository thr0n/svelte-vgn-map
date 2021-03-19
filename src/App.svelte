<script>
    import {onMount} from 'svelte';
    import L from 'leaflet'
    import AddressPopup from './components/AddressPopup.svelte';

    export let name;

    function bindPopup(marker, createFn) {
        let popupComponent
        marker.bindPopup(() => {
            let container = L.DomUtil.create('div')
            popupComponent = createFn(container)
            return container
        })
    }

    onMount(() => {
        const mymap = L.map('mapid').setView([53.571, 9.99], 13);
        L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
            attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: 'mapbox/streets-v11',
            tileSize: 512,
            zoomOffset: -1,
            accessToken: process.env.MAPBOX_API_KEY
        }).addTo(mymap);
        let marker = L.marker([53.5904053, 9.9425346]).addTo(mymap);
        bindPopup(marker, (container) => {
            let c = new AddressPopup({
                target: container
            })
        })
    })
</script>

<main>
    <h1>Hello {name}!</h1>
    <div id="mapid"></div>
</main>


<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }

    #mapid {
        height: 640px;
    }
</style>