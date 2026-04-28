<script>
    import {Row, Col, Button, Input, InputGroup} from '@sveltestrap/sveltestrap';

    let {plush, onbukaDetailProduk, onhargaPerProduk, jumlahProduk = 0, /*onbukaKeranjang*/} = $props();

    let jumlahPesanan = $state(jumlahProduk);
    // untuk mengecek apakah jumlahProduk alias data dari si parent ada berubah atau tidak
    $effect(() => {
        jumlahPesanan = jumlahProduk;
    });

    function ubahJumlahPesanan(tipe){
        if(tipe === '+'){
            if(jumlahPesanan < 100){
                jumlahPesanan++;
            }
        }else if(tipe === '-'){
            if(jumlahPesanan > 0){
                jumlahPesanan--;
            }
        }
    }

    let hargaTotal = $derived.by(() => {
        return {
            id: plush.id,
            nama: plush.nama,
            gambar: plush.gambar.main,
            jumlah: jumlahPesanan,
            harga: plush.harga,
            hargaTotal: jumlahPesanan * plush.harga
        }
    });
    
    $effect(() => {
        onhargaPerProduk(hargaTotal);
    });

    $effect(() => {
        // Jika user mengetik angka minus, paksa jadi 0
        if (jumlahPesanan < 0) {
            jumlahPesanan = 0;
        }
        // Jika user mengetik angka lebih dari 100, paksa jadi 100
        if (jumlahPesanan > 100) {
            jumlahPesanan = 100;
        }
    });

    // let buttonKeranjangDiKlik = $state(false);
    // function tambahkanKeranjang(){
    //     buttonKeranjangDiKlik = !buttonKeranjangDiKlik;
    //     if(buttonKeranjangDiKlik){
    //         onbukaKeranjang(hargaTotal);
    //     }else{
    //         onbukaKeranjang({id:plush.id, delete:true});
    //     }
    // }

</script>

<Row class="mb-5 align-items-center px-5">
    <Col class="d-flex justify-content-center"><img src={plush.gambar.main} width="200" alt={plush.nama}></Col>
    <Col>
        <h5 class="nama-plush" onclick={()=>onbukaDetailProduk(plush)}>{plush.nama}</h5>
        <p>{plush.deskripsi}</p>
        <h5>Rp. {plush.harga}</h5>
    </Col>
    <Col>
        <InputGroup>
            <Button color="secondary" onclick={()=>ubahJumlahPesanan("-")}>-</Button>
            <Input type="number" min=0 max=100 bind:value={jumlahPesanan}/>
            <Button color="secondary" onclick={()=>ubahJumlahPesanan("+")}>+</Button>
        </InputGroup>
        <Row>
            <Col>
                <h5>Rp. {jumlahPesanan * plush.harga}</h5>
            </Col>
        </Row>
    </Col>
    
</Row>

<style>
    :global(.container .row .col .nama-plush:hover){
        color: pink;
        text-decoration: underline;
        cursor: pointer;
    }
</style>