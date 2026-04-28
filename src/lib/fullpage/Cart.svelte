  <script>
    import { Table, Button } from '@sveltestrap/sveltestrap';
    import ModalConfirmation from '$lib/KonfirmasiModal/ModalConfirmation.svelte';
    import {
      Card,
      CardBody,
      CardFooter,
      CardHeader,
      CardSubtitle,
      CardText,
      CardTitle, CardImg
    } from '@sveltestrap/sveltestrap';

    let {keranjang, oncheckOut} = $props();
    let hargaAkhir = $derived(
          keranjang.reduce((total, item) => total + item.hargaTotal, 0)
      );

    let telahCheckout = $state(false);

    let isshowCheckout = $state(false);
    function toggleisshowCheckout(){
      isshowCheckout = !isshowCheckout;
    }
    function handleConfirmisCheckout(){
      prosesCheckout();
      isshowCheckout = false;
    }
    function prosesCheckout(){
      oncheckOut();
      telahCheckout = true;
    }
    function handleCancelisCheckout(){
      isshowCheckout = false;
    }
  </script>

  <ModalConfirmation 
    open={isshowCheckout} 
    onConfirm={handleConfirmisCheckout} 
    onCancel={handleCancelisCheckout} 
    massage="Apakah anda ingin melakukan checkout?" 
  />
  <Table striped>
    <thead>
      <tr>
        <th>No</th>
        <th>Nama</th>
        <th>Gambar</th>
        <th>Harga</th>
        <th>Harga Total</th>
      </tr>
    </thead>
    <tbody>
      {#each keranjang as plush, index}
          <tr>
              <td>{index+1}</td>
              <td>{plush.nama}</td>
              <td><img src={plush.gambar} alt={plush.gambar} width="200"/></td>
              <td>Rp {plush.harga}</td>
              <td>Rp {plush.hargaTotal}</td>
          </tr>
          {:else}
          <h3>Belum ada yang dibeli nih</h3>
      {/each}
    </tbody>
  </Table>
  <h2>Rp {hargaAkhir}</h2>
  {#if keranjang.length !== 0}
    <Button color="success" onclick={toggleisshowCheckout}>Checkout</Button>
  {/if}
  {#if telahCheckout}

  <Card style="max-width: 400px; margin: 20px auto;">
    <CardHeader>
      <CardTitle>Pikan</CardTitle>
    </CardHeader>
    <CardImg src="Niko.jpg" alt="Niko" width="300"/>
    <CardBody>
      <CardSubtitle>Pikri Anwari</CardSubtitle>
      <CardText>Ceritanya udah checkout hehe(づ￣ 3￣)づ</CardText>
      <CardText>Sorry kalau jelek ＞︿＜ aku butuh waktu 54 jam 8 menit untuk menyelesaikan ini😭</CardText>
    </CardBody>
    <CardFooter>Catbox</CardFooter>
  </Card>
  {/if}