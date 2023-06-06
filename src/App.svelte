<script>
	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";
	import 'bootstrap/dist/css/bootstrap.min.css';
  
	let jsonData = [];
	let sortedData = [];
	let searchQuery = "";
	let tableVisible = false;
	let selectedCandidate = null;
	let currentPage = 1;
	let sortField = 'name'; // Track the sort field
	let sortAsc = true; // Track the sort order
	const itemsPerPage = 10;
  
	const dispatch = createEventDispatcher();
  
	onMount(async () => {
	  await fetchData();
	  tableVisible = true;
	});
  
	async function fetchData() {
	  const response = await fetch(`https://api.recruitly.io/api/candidate/?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E&search=${encodeURIComponent(searchQuery)}`);
	  const responseData = await response.json();
	  jsonData = responseData.data;
	  sortData(); // Sort the data initially
	}
  
	function handleTitleClick(candidate) {
	  selectedCandidate = candidate;
	  dispatch("showPopup");
	}
  
	function sortData() {
	  if (sortField === 'name') {
		sortedData = jsonData.sort((a, b) => {
		  if (sortAsc) {
			return a.firstName.localeCompare(b.firstName);
		  } else {
			return b.firstName.localeCompare(a.firstName);
		  }
		});
	  } else if (sortField === 'date') {
		sortedData = jsonData.sort((a, b) => {
		  if (sortAsc) {
			return new Date(a.date) - new Date(b.date);
		  } else {
			return new Date(b.date) - new Date(a.date);
		  }
		});
	  }
	}
  
	function toggleSortOrder(field) {
	  if (sortField === field) {
		sortAsc = !sortAsc;
	  } else {
		sortField = field;
		sortAsc = true;
	  }
	  sortData();
	}
  
	$: filteredData = sortedData.filter(candidate => candidate.firstName.toLowerCase().includes(searchQuery.toLowerCase()));
  </script>
  
  <main class="container mt-4">
	<div class="mb-3">
	  <button class="btn btn-primary" on:click={() => toggleSortOrder('name')}>Sort by Name {sortField === 'name' && (sortAsc ? 'Asc' : 'Desc')}</button>
	  <button class="btn btn-primary ml-2" on:click={() => toggleSortOrder('date')}>Sort by Date {sortField === 'date' && (sortAsc ? 'Asc' : 'Desc')}</button>
	</div>
  
	{#if tableVisible}
	  <table class="table">
		<thead class="thead-light">
		  <tr>
			<th>First Name</th>
			<th>Surname</th>
			<th>Email</th>
			<th>Mobile</th>
			<th>ID</th>
			<th>Date</th>
		  </tr>
		</thead>
		<tbody>
		  {#each filteredData.slice((currentPage - 1) * itemsPerPage, currentPage * itemsPerPage) as candidate}
			<tr>
			  <td>{candidate.firstName}</td>
			  <td>{candidate.surname}</td>
			  <td>{candidate.email}</td>
			  <td>{candidate.mobile}</td>
			  <td>{candidate.id}</td>
			  <td>{candidate.date}</td>
			</tr>
		  {/each}
		</tbody>
	  </table>
  
	  <ul class="pagination">
		{#each Array(Math.ceil(filteredData.length / itemsPerPage)).fill() as _, i}
		  <li class="page-item {currentPage === i + 1 ? 'active' : ''}">
			<a class="page-link" href="#" on:click|preventDefault={() => (currentPage = i + 1)}>{i + 1}</a>
		  </li>
		{/each}
	  </ul>
	{/if}
  </main>
  
  {#if selectedCandidate}
  <div class="popup">
	<div class="popup-content">
	  <h2>{selectedCandidate.firstName}</h2>
	  <p>ID: {selectedCandidate.id}</p>
	  <p>First Name: {selectedCandidate.firstName}</p>
	  <p>Surname: {selectedCandidate.surname}</p>
	  <p>Email: {selectedCandidate.email}</p>
	  <button class="btn btn-primary" on:click={() => (selectedCandidate = null)}>Close</button>
	</div>
  </div>
  {/if}
  
 
