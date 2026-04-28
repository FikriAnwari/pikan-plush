<script>
    import {Container, Row, Col, Button} from '@sveltestrap/sveltestrap';
    import DetailsProduk from '$lib/DetailsProduk/DetailsProduk.svelte';
    import Product from './Product.svelte';
    import ModalConfirmation from '$lib/KonfirmasiModal/ModalConfirmation.svelte';

    let {data=[], onambilDataKeranjang, initial_dataKeranjang=[] } = $props();

    let produkDiKlik = $state(null);

    function bukaDetailProduk(plush){
        produkDiKlik = plush;
    }

    function tutupDetailProduk(){
        produkDiKlik = null;
    }

    let keranjang = $state([]);
    //melakukan sinkronisasi / initialisasi variabel keranjang
    /*
        jadikan data keranjang itu dikembalikan ke komponen +page yg di +page disimpan di variabel dataKeranjang 
        nah kalau kita buka cart lalu balik lagi ke halaman +page dia akan ngerender ulang ProductList dan Product
        nah karena dia ngerender ulang semuanya jadi ke reset walau kita sudah melakukan pengisian data tadi jadinya kaya
        baru pertama kali ngisi. Solusinya adalah dengan mengirimkan data ulang (dataKeranjang) dari variabel data keranjang +page
        jadi komponen tahu kalau dia sudah pernah melakukan pengisian data sebelumnya dalam kassus ini kita kirim pakai
        props initial_dataKeranjang
    */
    if(initial_dataKeranjang.length !== 0){
        keranjang = initial_dataKeranjang;
    }

    function hargaPerProduk(plushBaru){
        const index = keranjang.findIndex(item => item.id === plushBaru.id);

        if(plushBaru.jumlah <= 0){
            if(index !== -1){
                keranjang.splice(index, 1);
            }
        }else{

            if(index == -1){
                if(plushBaru.jumlah > 0){
                    keranjang.push(plushBaru);
                }
            }else if(keranjang[index].jumlah !== plushBaru.jumlah){
    
                // keranjang[index].nama = plushBaru.nama;
                // keranjang[index].gambar = plushBaru.gambar.main;
                keranjang[index].jumlah = plushBaru.jumlah;
                keranjang[index].harga = plushBaru.harga;
                keranjang[index].hargaTotal = plushBaru.hargaTotal;
    
            }
        }
        console.log("PlushBaru", plushBaru);
        console.log(keranjang);
    }

    let keranjangTerisi = $state(false);
    $effect(() => {
        if(keranjang.length > 0 && keranjang.length <= 100){
            keranjangTerisi = true;
        }else{
            keranjangTerisi = false;
            buttonKeranjangDiKlik = false;
        }
    });

    let buttonKeranjangDiKlik = $state(false);
    //melakukan sinkronisasi / initialisasi variabel buttonKeranjangDiKlik
    if(initial_dataKeranjang.length !== 0){
        buttonKeranjangDiKlik = true;
    }
    let ismodalkonfiramsiHapusTombolKeranjang = $state(false);
    function tambahkanKeranjang(){
        if(keranjangTerisi === false){
            return;
        }

        if(buttonKeranjangDiKlik === false){
            onambilDataKeranjang(keranjang);
            buttonKeranjangDiKlik = true;
        }else{
            ismodalkonfiramsiHapusTombolKeranjang = true;
        }
    }

    // Fungsi yang dipanggil saat user klik "Ya" di Modal
    function handleConfirmHapus() {
        hapusKeranjang();
        ismodalkonfiramsiHapusTombolKeranjang = false;
    }

    // Fungsi yang dipanggil saat user klik "Batal" di Modal
    function handleCancelHapus() {
        ismodalkonfiramsiHapusTombolKeranjang = false;
    }

    function hapusKeranjang(){
        keranjang.length = 0;
        onambilDataKeranjang([]);
        buttonKeranjangDiKlik = false;
    }
    

</script>

<Container>

    {#if produkDiKlik != null}
        <DetailsProduk data={produkDiKlik} ontutupDetailsProduk={tutupDetailProduk}/>
        {:else}
        <ModalConfirmation 
            open={ismodalkonfiramsiHapusTombolKeranjang} 
            onConfirm={handleConfirmHapus}
            onCancel={handleCancelHapus}
            massage="Apakah anda ingin mengosongkan keranjang anda?"
        />
        {#each data as plush}
        {@const itemdiKeranjang = keranjang.length !== 0 ? keranjang.find(item => item.id === plush.id) : []}
            <Product plush={plush} onhargaPerProduk={hargaPerProduk} onbukaDetailProduk={()=>bukaDetailProduk(plush)}
            jumlahProduk={itemdiKeranjang? itemdiKeranjang.jumlah : 0}
            />
        {/each}
        <Row>
            <Col>
                <Button color={keranjangTerisi ? buttonKeranjangDiKlik ? "danger" : "success" : "secondary"} onclick={tambahkanKeranjang}>{buttonKeranjangDiKlik && keranjangTerisi? "Hapus" : "Tambahkan ke Keranjang"}</Button>
            </Col>
        </Row>
    {/if}

</Container>

