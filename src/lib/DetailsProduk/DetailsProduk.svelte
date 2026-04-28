<script>
    let {data = {}, ontutupDetailsProduk} = $props();
    import {Container, Col, Row, Carousel, CarouselControl, CarouselIndicators, CarouselItem, Button} from '@sveltestrap/sveltestrap'
    let activeIndex = $state(0);

    let formatSveltestrap = $derived(
        data.gambar?.other?.map(link => ({ src: link, altText: 'Plushie' })) || []
    );

</script>

<Container>
    <Row>
        <Col>
            <Button color="secondary" size="lg" onclick={ontutupDetailsProduk}>Back</Button>
        </Col>
    </Row>
    <Row>
        <Col>
            <Carousel items={formatSveltestrap} bind:activeIndex>
                <CarouselIndicators bind:activeIndex items={data.gambar.other} />
                <div class="carousel-inner">
                    {#each data.gambar.other as gambar, index}
                    <CarouselItem bind:activeIndex itemIndex={index}>
                        <img src={gambar} class="d-block w-100" alt="{gambar} {index + 1}" />
                    </CarouselItem>
                    {/each}
                </div>
        
                <CarouselControl direction="prev" bind:activeIndex items={formatSveltestrap} />
                <CarouselControl direction="next" bind:activeIndex items={formatSveltestrap} />
            </Carousel>
        </Col>
    </Row>
    <Row>
        <Col>
            <h4>{data.nama}</h4>
            <p>{data.deskripsi}</p>
            <h5>Rp. {data.harga}</h5>
        </Col>
    </Row>

</Container>

<style>
    :global(.row){
        margin-bottom: 20px;
    }
    :global(.carousel-control-next span, .carousel-control-prev span){
        background-color: salmon;
    }
</style>