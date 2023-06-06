<script>
   export let handleDeleteConfirm;
   export let handleClose;
</script>
  

  <div class="popup">
    <h4>Confirm Delete</h4>
    <p>Are you sure you want to delete this job?</p>
    <button on:click={handleDeleteConfirm} class="btn btn-danger">Delete</button>
    <button on:click={handleClose} class="btn btn-secondary">Cancel</button>
  </div>

  
  <style>
    .popup {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    }
  </style>
