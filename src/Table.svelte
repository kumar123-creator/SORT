<script>
    export let jsonData;
    export let handleUploadCV;
    export let handleDelete;
    export let handleEdit;
  </script>
  
  <style>
    @import 'bootstrap/dist/css/bootstrap.min.css';
  </style>
  
  <table class="table">
    <thead class="thead-light">
      <tr>
        <th>firstName</th>
        <th>surname</th>
        <th>email</th>
        <th>mobile</th>
        <th>ID</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      {#each jsonData as job}
        <tr>
          <td>{job.firstName}</td>
          <td>{job.surname}</td>
          <td>{job.email}</td>
          <td>{job.mobile}</td>
          <td>{job.id}</td>
          <td>
            <button on:click={() => handleEdit(job)} class="btn btn-info">Edit</button>
            <button on:click={() => handleDelete(job.id)} class="btn btn-danger">Delete</button>
            <button on:click={() => handleUploadCV(job.id)} class="btn btn-primary">Upload CV</button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
