<script>
    import Bookshelf from './Bookshelf/Bookshelf.svelte';
    import './Library.css';
    export let allBooks;

    let showModal = false;
    let newBook = {
        name: '',
        bookCategory: '',
        rating: 0,
        timeSpent: 0,
        chaptersRead: 0,
        journalEntries: []
    };

    function addBook() {
        allBooks = [...allBooks, { ...newBook }];
        resetForm();
    }

    function resetForm() {
        newBook = {
            name: '',
            bookCategory: '',
            rating: 0,
            timeSpent: 0,
            chaptersRead: 0,
            journalEntries: []
        };
        showModal = false;
    }
</script>

<div class="library">
    <div class="libraryHeader">
        <h2> My Library </h2>
        <div>
            <button class="addBookButton" on:click={() => showModal = true}> Add Book </button>
        </div>
    </div>
    <Bookshelf></Bookshelf>
    
    
    {#if showModal}
    <div class="modal">
        <div class="modal-content">
            <span class="close" on:click={resetForm}>&times;</span>
            <h2>Add New Book</h2>
            <label>
                Name:
                <input type="text" bind:value={newBook.name} required />
            </label>
            <label>
                Category:
                <input type="text" bind:value={newBook.bookCategory} required />
            </label>
            <label>
                Rating:
                <input type="number" bind:value={newBook.rating} step="0.1" min="0" max="5" required />
            </label>
            <label>
                Time Spent (hours):
                <input type="number" bind:value={newBook.timeSpent} step="0.1" min="0" required />
            </label>
            <label>
                Chapters Read:
                <input type="number" bind:value={newBook.chaptersRead} min="0" required />
            </label>
            <button on:click={addBook}>Submit</button>
            <button class="close-button" on:click={resetForm}>Close</button> <!-- Close button added -->
        </div>
    </div>
{/if}

</div>

<style>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000; /* Make sure it's above other content */
}

.modal-content {
    background-color: white; /* White background for the modal */
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2); /* Subtle shadow */
    width: 300px; /* Width of the modal */
    text-align: left; /* Align text to the left */
}

.close {
    cursor: pointer;
    font-size: 20px;
    position: absolute;
    top: 10px;
    right: 15px;
    color: #aaa; /* Color for the close button */
}

.close:hover {
    color: black; /* Change color on hover */
}

h2 {
    margin-bottom: 20px; /* Space below the header */
}

label {
    display: block; /* Make labels block elements */
    margin-bottom: 10px; /* Space between labels */
}

input {
    width: 100%; /* Full width of the input fields */
    padding: 8px; /* Padding for input fields */
    border: 1px solid #ccc; /* Light gray border */
    border-radius: 4px; /* Rounded corners */
}



</style>
