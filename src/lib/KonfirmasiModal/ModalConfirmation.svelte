<script lang="ts">
  import { Modal, ModalBody, ModalFooter, ModalHeader, Button } from '@sveltestrap/sveltestrap';

  // Svelte 5 props
  let { open, onConfirm, onCancel , massage} = $props();
</script>

<!-- isOpen langsung pakai props 'open' dari parent -->
<Modal isOpen={open} backdrop="static">
  <ModalHeader>Konfirmasi Hapus</ModalHeader>
  <ModalBody>
    {massage}
  </ModalBody>
  <ModalFooter>
    <!-- Jalankan callback yang dikirim parent -->
    <Button color="danger" onclick={onConfirm}>Ya</Button>
    <Button color="secondary" onclick={onCancel}>Batal</Button>
  </ModalFooter>
</Modal>
