<script>

    import {Container, Row, Col, Button} from '@sveltestrap/sveltestrap';
    import ProductList from '$lib/fullpage/ProductList.svelte';
    import NavigationsBar from '$lib/NavigationsBar/NavigationsBar.svelte';
    import Cart from '$lib/fullpage/Cart.svelte';

    let produk = [
        {
            nama: "Niko",
            id: "id001",
            harga: 694000,
            deskripsi: `Niko adalah karakter dari game OneShot, Ukuran Plushie
                berkisar tinggi 21 cm`,
            gambar:{
                main: "product_oneshot_niko_plush_photo1.webp",
                other: ["product_oneshot_niko_plush_photo2.webp", "product_oneshot_niko_plush_photo3.webp",
                    "product_oneshot_niko_plush_photo4.webp", "product_oneshot_niko_plush_photo5.webp",
                    "product_oneshot_niko_plush_photo7.webp", "product_oneshot_niko_plush_photo8.webp"]
            }
        },
        {
            nama: "Hoshino",
            id: "id002",
            harga: 517000,
            deskripsi: `Hoshino adalah karakter dari game Blue Archive, Ukuran Plushie
                berkisar tinggi 17 cm`,
            gambar:{
                main: "product_hoshino1.jpg",
                other: ["product_hoshino2.jpg", "product_hoshino3.webp"]
            }
        },
        {
            nama: "J-Plush",
            id: "id003",
            harga: 440000,
            deskripsi: `J adalah karakter dari series animasi buatan Glitch Production 
                yakni Murder Drones, Ukuran Plushie
                berkisar tinggi 30 cm`,
            gambar:{
                main: "j-plush-1.webp",
                other: ["j-plush-2.webp", "j-plush-3.webp", "j-plush-4.webp", "j-plush-5.webp"]
            }
        }
    ];

    // let dataMasukKeranjang = $state([]);
    // function bukaKeranjang(plushBaru){
    //     const index = dataMasukKeranjang.findIndex(item => item.id === plushBaru.id);

    //     if(plushBaru.delete === true){
    //         console.log('harus di hapus');
    //     }

    //     if(index == -1){
    //         if(plushBaru.jumlah > 0){
    //             dataMasukKeranjang.push(plushBaru);
    //         }
    //     }else if(dataMasukKeranjang[index].jumlah !== plushBaru.jumlah){
    //         dataMasukKeranjang[index].jumlah = plushBaru.jumlah;
    //         dataMasukKeranjang[index].harga = plushBaru.harga;
    //     }

    //     console.log(dataMasukKeranjang);
    // }
    let dataKeranjang = $state([]);
    function ambilDataKeranjang(pesan){
        dataKeranjang = pesan;
    }

    let halamankeranjang = $state(false);
    function bukaHalamanKeranjang(){
        halamankeranjang = true;
    }
    function bukaHalamanHome(){
        halamankeranjang = false;
    }

    function checkOut(){
        dataKeranjang.length = 0;
    }

</script>

<!-- <h3>
    ini adalah toko plushie buatkan ku sebagai latihan svelte
</h3> -->

<NavigationsBar onbukaHalamanKeranjang={bukaHalamanKeranjang} onbukaHalamanHome={bukaHalamanHome}/>
<Container>
    {#if halamankeranjang === true}
        <Button color="secondary" onclick={() => {halamankeranjang = false}}>Back</Button>
        <Cart keranjang={dataKeranjang} oncheckOut={checkOut}/>
        {:else}
        <ProductList data={produk} onambilDataKeranjang={ambilDataKeranjang} initial_dataKeranjang={dataKeranjang}/>
    {/if}
</Container>

<style>
    
</style>