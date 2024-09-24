<script>
    import './Content.css';
    export let allBooks;
    export let selectedBook;

    // Bind these variables to input fields to capture new journal entry details
    let journalName = '';
    let journalDate = '';
    let startChapter = '';
    let endChapter = '';
    let timeSpentReading = '';

    // Function to handle form submission
    function addJournalEntry() {
        // Check if selectedBook is set
        if (selectedBook) {
            // Find the selected book in the allBooks array
            const bookIndex = allBooks.findIndex(book => book.name === selectedBook);
            
            // Create the new journal entry object
            const newJournalEntry = {
                name: journalName,
                date: journalDate,
                startChapter: parseInt(startChapter),
                endChapter: parseInt(endChapter),
                timeSpentReading: timeSpentReading
            };

            // Add the new journal entry to the selected book's journalEntries
            allBooks[bookIndex].journalEntries = [...allBooks[bookIndex].journalEntries, newJournalEntry];

            // Clear input fields after submission
            journalName = '';
            journalDate = '';
            startChapter = '';
            endChapter = '';
            timeSpentReading = '';
        }
    }
</script>

<select id="selectBookJournal" class="selectBookJournal" bind:value={selectedBook}>
    {#each allBooks as book}
        <option value={book.name}>{book.name}</option>
    {/each}
</select>

<!-- Input fields for the new journal entry -->
<input id="name" placeholder="Name" bind:value={journalName} />
<input id="date" placeholder="Date" bind:value={journalDate} />
<input id="startChapter" placeholder="Start Chapter" bind:value={startChapter} />
<input id="endChapter" placeholder="End Chapter" bind:value={endChapter} />
<input id="timeSpentReading" placeholder="Time Spent Reading" bind:value={timeSpentReading} />

<!-- Submit button to add a new journal entry -->
<button on:click={addJournalEntry}>Submit</button>

<!-- Display updated list of books and their journal entries -->
<div>
    <h3>Books and Journal Entries</h3>
    {#each allBooks as book}
        <div>
            <h4>{book.name}</h4>
            <p>Rating: {book.rating}</p>
            <p>Time Spent: {book.timeSpent}</p>
            <p>Chapters Read: {book.chaptersRead}</p>
            <ul>
                {#each book.journalEntries as entry}
                    <li>{entry.name} - {entry.date} - Chapters: {entry.startChapter} to {entry.endChapter} - Time: {entry.timeSpentReading}</li>
                {/each}
            </ul>
        </div>
    {/each}
</div>

<style>
    /* Your CSS styles for the component */
</style>
