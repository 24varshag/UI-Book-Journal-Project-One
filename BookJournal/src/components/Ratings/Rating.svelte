<script>
    import './Rating.css';

    export let statusBook;
    export let allBooks;
    export let ratingBook = null; // Initialize with null or a default book
    let selectedRating = 0; // Track the selected rating
    let submissionMessage = ""; // Message to show after submission
    let selectedCategory = ""; // Track the selected category
    let newCategory = ""; // Variable for the new category selection
    let temp;
    newCategory = temp;
    let editCategory = false; // Track whether the user wants to edit the category
    let showPopup = false; // Manage popup visibility
    let popupMessage = ""; // Popup message

    function submitRating() {
        if (ratingBook) {
            const bookIndex = allBooks.findIndex(b => b.name === ratingBook);
            if (bookIndex !== -1) {
                // Update the rating and category
                allBooks[bookIndex].rating = selectedRating;
                if (editCategory) {
                    newCategory = temp;
                    allBooks[bookIndex].bookCategory = newCategory;
                }


                // Show popup message
                popupMessage = "Book updated successfully!";
                showPopup = true;

                // Reset editCategory to false after submission
                editCategory = false;
                temp = ""; // Reset temp as well

                // Hide popup after 3 seconds
                setTimeout(() => {
                    showPopup = false;
                }, 3000);
            } else {
                submissionMessage = "Book not found.";
                console.warn(submissionMessage);
            }
        } else {
            submissionMessage = "No book selected.";
            console.warn(submissionMessage);
        }
    }

    // Update the selected category and new category when the ratingBook changes
    $: if (ratingBook) {
        const selectedBook = allBooks.find(b => b.name === ratingBook);
        selectedCategory = selectedBook.bookCategory;
        newCategory = selectedCategory; // Initialize newCategory with the selected book's category
    } else {
        selectedCategory = "";
        newCategory = ""; // Reset newCategory if no book is selected
    }
</script>

<div class="ratingsDiv">
    <h2>Book Status & Rating</h2>
    <div class="selectBookDiv">
        <label>Select a Book</label>
        <select class="selectBook" bind:value={ratingBook}>
            {#each allBooks as book}
                <option value={book.name}>
                    {book.name}
                </option>
            {/each}
        </select>
    </div>

    <div>
        <label>
            <input type="checkbox" bind:checked={editCategory}>
            Edit Category
        </label>
    </div>

    {#if editCategory}
        <div class="selectBookDiv">
            <div class="currentCategory">Current Category: {selectedCategory}</div>
            <br>
            <label for="categorySelect">Select New Category</label>
            <select class="selectBook" id="categorySelect" bind:value={temp}>
                <option value="Reading">Reading</option>
                <option value="Finished">Finished</option>
                <option value="To Be Read">To Be Read</option>
                <option value="Completed">Completed</option>
            </select>
        </div>
    {/if}

    <div class="fiveStars">
        <div class="star-rating">
            <input type="radio" name="rating" id="star1" value="5" bind:group={selectedRating}>
            <label for="star1" class="star">★</label>
            <input type="radio" name="rating" id="star2" value="4" bind:group={selectedRating}>
            <label for="star2" class="star">★</label>
            <input type="radio" name="rating" id="star3" value="3" bind:group={selectedRating}>
            <label for="star3" class="star">★</label>
            <input type="radio" name="rating" id="star4" value="2" bind:group={selectedRating}>
            <label for="star4" class="star">★</label>
            <input type="radio" name="rating" id="star5" value="1" bind:group={selectedRating}>
            <label for="star5" class="star">★</label>
        </div>
    </div>
    <div style="display:flex; justify-content:end; align-items:end; width: 100%;">
        <button class="submitButton1" on:click={submitRating}>Submit</button>
    </div>
    
    {#if submissionMessage}
        <div class="submissionMessage">{submissionMessage}</div>
    {/if}

    {#if showPopup}
        <div class="popup">{popupMessage}</div>
    {/if}
</div>

<style>
    .popup {
        position: fixed;
        top: 50%;
        left: 50%;
        width: 500px;;
        height: 100px;
        transform: translate(-50%, -50%);
        background: #7fb6ed;
        color: white;
        border-radius: 5px;
        font-size: 20px;
        z-index: 1000;
        animation: fadeOut 7s forwards; 
        text-align: center; 
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    }

    @keyframes fadeOut {
        0% { opacity: 1; }
        100% { opacity: 0; }
    }
</style>
