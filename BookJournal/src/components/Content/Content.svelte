<script>
    import Books from '../Books/Books.svelte';
    import Library from '../Library/Library.svelte';
    import Rating from '../Ratings/Rating.svelte';
    import './Content.css';
    export let allBooks;
    export let selectedBook;
    export let ratingBook;
    let statusBook;

    let isOpen = true;

    let userName = '';
    let userCatogary = '';
    let entryDate = '';
    let startChapter = '';
    let endChapter = '';
    let timeSpent = '';
    let userText = '';

    let message = '';
    let showMessage = false;

    function addEntry(event) {
        event.preventDefault(); // Prevent page reload

        if (selectedBook) {
            const bookIndex = allBooks.findIndex(book => book.name === selectedBook);
            
            const newJournalEntry = {
                name: userName,
                catogary: userCatogary,
                date: entryDate,
                startChapter: parseInt(startChapter),
                endChapter: parseInt(endChapter),
                timeSpentReading: parseFloat(timeSpent),
                textEntry: userText
            };

            allBooks[bookIndex].journalEntries = [...allBooks[bookIndex].journalEntries, newJournalEntry];
            allBooks[bookIndex].chaptersRead += (newJournalEntry.endChapter - newJournalEntry.startChapter) + 1;

            // Clear input fields
            userName = '';
            entryDate = '';
            startChapter = '';
            endChapter = '';
            timeSpent = '';
            userText = '';

            // Shows gthe confirmation message
            message = 'Your journal entry has been saved!';
            showMessage = true;

            // Hide message after a few seconds
            setTimeout(() => {
                showMessage = false;
            }, 3000);
        }
    }
</script>

<div id="content">
    <div class="article column1">
        <div class="JournalHeader">
            <h2>Journal</h2>
        </div>

        <div class="JournalEntryPanel">
            <form on:submit={addEntry}>
                <div class="journalEntry">
                    <div class="userDetails">
                        <label class="visually-hidden" for="userName">Name</label>
                        <input id="userName" class="userName" bind:value={userName} required />
                        <label class="visually-hidden" for="date">Date</label>
                        <input id="date" class="date" type="date" bind:value={entryDate} required />
                    </div>
                    <div class="activityInputs">
                        <div class="selectBookJournalDiv">
                            <label class="visually-hidden" for="selectBookJournal">Select your book</label>
                            <select id="selectBookJournal" class="selectBookJournal" bind:value={selectedBook}>
                                {#each allBooks as book}
                                    <option value={book.name}>{book.name}</option>
                                {/each}
                            </select>
                        </div>

                        <div class="chaptersTime">
                            <div class="journalAttr">
                                <label class="visually-hidden" for="chapterStart">Start Chapter</label>
                                <input id="chapterStart" class="chapterStart" type="number" bind:value={startChapter} required />
                            </div>
                            <div class="journalAttr">
                                <label class="visually-hidden" for="chapterEnd">End Chapter</label>
                                <input id="chapterEnd" class="chapterEnd" type="number" bind:value={endChapter} required />
                            </div>
                            <div class="journalAttr">
                                <label class="visually-hidden" for="timeSpent">Reading Time</label>
                                <input id="timeSpent" class="timeSpent" type="text" bind:value={timeSpent} required />
                            </div>
                        </div>
                    </div>
                </div>
                <textarea class="entryTextBox" style="resize: none;" bind:value={userText} placeholder="Enter your journal here..."></textarea>
                <div style="display:flex; justify-content:end; align-items:end; width: 100%;">
                    <button class="submitButton" type="submit" style="margin-right: 20px;">Submit</button>
                </div>
            </form>
            {#if showMessage}
                <div class="popup">{message}</div>
            {/if}
        </div>
    </div>

    <div class="article column2">
        <Library bind:allBooks={allBooks}></Library>
        <Rating bind:allBooks={allBooks} bind:statusBook bind:ratingBook />
    </div> 
</div>
<Books bind:allBooks={allBooks}></Books>

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

