<script>
    export let allBooks;

    let editingEntry = null;
    let editingBookIndex = null;
    let activeTab = 'Reading'; 

    const categories = ['Reading', 'Finished', 'To Be Read', 'Completed'];

    function startEditing(bookIndex, entry) {
        editingEntry = { ...entry }; 
        editingBookIndex = bookIndex; 
    }

    function saveEntry(bookIndex) {
        if (editingEntry) {
            allBooks[bookIndex].journalEntries = allBooks[bookIndex].journalEntries.map(entry =>
                entry.date === editingEntry.date ? editingEntry : entry
            );
            cancelEditing(); 
        }
    }

    function cancelEditing() {
        editingEntry = null; 
        editingBookIndex = null; 
    }
</script>



<div>
    <h3>Books and Journal Entries</h3>
    <div class="tabs">
        {#each categories as category}
            <div 
                class="tab {activeTab === category ? 'active' : ''}" 
                on:click={() => activeTab = category}
            >
                {category}
            </div>
        {/each}
    </div>

    {#each allBooks.filter(book => book.bookCategory === activeTab) as book, bookIndex}
        <div class="book-list">
            <div class="book">
                <div class="books">
                    <h2>{book.name}</h2>
                    <div class="bookDetails">
                        <p class="roundedDiv">Rating: {book.rating}</p>
                        <p class="roundedDiv2">Time Spent: {book.timeSpent} hours</p>
                        <p class="roundedDiv3">Chapters Read: {book.chaptersRead}</p>
                    </div>
                    
                </div>
                <div class="journals">
                    <ul>
                        {#each book.journalEntries as entry}
                            <li>
                                {#if editingEntry && editingBookIndex === bookIndex && editingEntry.date === entry.date}
                                <form on:submit|preventDefault={() => saveEntry(bookIndex)}>
                                    <div>
                                        <label>
                                            Entry Text
                                            <input type="text" bind:value={editingEntry.textEntry} placeholder="Entry text" required />
                                        </label>
                                    </div>
                                    <div>
                                        <label>
                                            Date
                                            <input type="date" bind:value={editingEntry.date} required />
                                        </label>
                                    </div>
                                    <div>
                                        <label>
                                            Start Chapter
                                            <input type="number" bind:value={editingEntry.startChapter} placeholder="Start Chapter" required />
                                        </label>
                                    </div>
                                    <div>
                                        <label>
                                            End Chapter
                                            <input type="number" bind:value={editingEntry.endChapter} placeholder="End Chapter" required />
                                        </label>
                                    </div>
                                    <div>
                                        <label>
                                            Time Spent Reading
                                            <input type="number" bind:value={editingEntry.timeSpentReading} placeholder="Time Spent Reading" required />
                                        </label>
                                    </div>
                                    <div>
                                        <button type="submit">Save</button>
                                        <button type="button" on:click={cancelEditing}>Cancel</button>
                                    </div>
                                </form>
                                
                                {:else}
                                    <div>
                                        <p>{entry.name}</p>
                                        <p>{entry.date}</p>
                                        <p>Chapters: {entry.startChapter} to {entry.endChapter}</p>
                                        <p>Time: {entry.timeSpentReading} hours</p>
                                        <p >Text: {entry.textEntry}</p>
                                        <button on:click={() => startEditing(bookIndex, entry)}>Edit</button>
                                    </div>
                                {/if}
                            </li>
                        {/each}
                    </ul>
                </div>
               
            </div>
        </div>
    {/each}
</div>

<style>
    .tabs {
        display: flex;
        border-bottom: 2px solid #ccc;
    }
    .tab {
        padding: 10px 20px;
        cursor: pointer;
        border: 1px solid transparent;
        margin-right: 5px;
        transition: background-color 0.3s;
    }
    .tab.active {
        border-bottom: 2px solid #007bff;
        font-weight: bold;
        background-color: #e7f3ff;
    }
    .tab:hover {
        background-color: #f0f0f0;
    }
    .book-list {
        background-color: #eaf2fb;
        margin-top: 20px;
    }
    .book {
        border: 1px solid #000;

        padding: 30px;
        margin-bottom: 10px;
        display: flex;
        flex-direction: column;
    }
    .books {
        display: flex;
        flex-direction: row;
        justify-content:space-between ;
        align-items: center;
    }

    ul{
        display: flex;
        flex-direction: row;
        gap:5%;
    }

    li{
        background-color: #f6faff;
        border: 1px solid var(--slate-gray);
        text-align: left;
        width: 400px;

    }

.journals {
  display: flex;
  flex-direction: row;
  overflow: auto;
}

.bookDetails{
    display: flex;
    flex-direction: row;
}

.roundedDiv{
    border: 2px solid var(--slate-gray);
    border-radius: 10px;
    padding:2px 10px;
    margin: 0px 5px;
    height:20px;
}

.roundedDiv2{
    border: 2px solid var(--slate-gray);

    border-radius: 10px;
    padding:2px 10px;
    margin: 0px 5px;
    height:20px;
}

.roundedDiv3{
    border: 2px solid var(--slate-gray);
    border-radius: 10px;
    padding:2px 10px;
    margin: 0px 5px;
    height:20px;
}
</style>