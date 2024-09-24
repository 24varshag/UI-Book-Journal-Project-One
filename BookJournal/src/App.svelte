<script>
  import Navbar from './components/Navbar/Navbar.svelte';
  import Feature from './components/Features/Feature.svelte';
  import Content from './components/Content/Content.svelte';
  import Library from './components/Library/Library.svelte';
  import Books from './components/Books/Books.svelte';
  let selectedBook = ''; // will hold the selected book
  let ratingBook ; // will hold the selected book
  let totalFinishedBooks = 0;
  let totalReadingTime = 0;

  let finishedBooks =[];
  let readingBooks =[];
  let toBeReadBooks =[];
  let droppedBooks = [];
  // Holds all the books in their library
  let allBooks = [
  {
    name: 'Atomic Habits',
    bookCategory: 'Finished',
    rating: 5,
    timeSpent: 5,
    chaptersRead: 10,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-10',
        startChapter: 1,
        endChapter: 4,
        timeSpentReading: 2,
        textEntry: 'The concept of habit stacking is simple yet powerful. I’m already thinking of small changes I can incorporate into my daily routine.'
      },
      {
        name: 'Varsha',
        date: '2024-09-12',
        startChapter: 5,
        endChapter: 10,
        timeSpentReading: 3,
        textEntry: 'The idea that habits compound over time really resonated with me. Every chapter gave me a practical way to implement this.'
      }
    ]
  },
  {
    name: 'The Alchemist',
    bookCategory: 'To Be Read',
    rating: null,
    timeSpent: 0,
    chaptersRead: 0,
    journalEntries: []
  },
  {
    name: 'Sapiens: A Brief History of Humankind',
    bookCategory: 'Reading',
    rating: 5,
    timeSpent: 6,
    chaptersRead: 12,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-13',
        startChapter: 1,
        endChapter: 5,
        timeSpentReading: 2.5,
        textEntry: 'The early history of humankind is fascinating! I’m particularly intrigued by how homo sapiens outlasted other species.'
      },
      {
        name: 'Varsha',
        date: '2024-09-14',
        startChapter: 6,
        endChapter: 12,
        timeSpentReading: 3.5,
        textEntry: 'The agricultural revolution really changed the course of history. I hadn’t realized how dramatically it altered human social structures.'
      }
    ]
  },
  {
    name: '1984',
    bookCategory: 'Dropped',
    rating: 3,
    timeSpent: 2,
    chaptersRead: 4,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-11',
        startChapter: 1,
        endChapter: 4,
        timeSpentReading: 2,
        textEntry: 'The dystopian setting was compelling, but I struggled with the pacing. Might revisit in the future.'
      }
    ]
  },
  {
    name: 'The Power of Now',
    bookCategory: 'Finished',
    rating: 4,
    timeSpent: 4,
    chaptersRead: 8,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-05',
        startChapter: 1,
        endChapter: 4,
        timeSpentReading: 2,
        textEntry: 'The focus on mindfulness and being present is something I’m trying to incorporate daily. It’s refreshing but difficult at times.'
      },
      {
        name: 'Varsha',
        date: '2024-09-06',
        startChapter: 5,
        endChapter: 8,
        timeSpentReading: 2,
        textEntry: 'The chapter on the “pain-body” was deep. I’ll need to reread this to fully grasp the concept, but it feels transformative.'
      }
    ]
  },
  {
    name: 'Becoming',
    bookCategory: 'Reading',
    rating: 5,
    timeSpent: 3,
    chaptersRead: 6,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-08',
        startChapter: 1,
        endChapter: 3,
        timeSpentReading: 1.5,
        textEntry: 'Michelle Obama’s journey is inspiring. Her honesty about her challenges growing up really drew me in.'
      },
      {
        name: 'Varsha',
        date: '2024-09-09',
        startChapter: 4,
        endChapter: 6,
        timeSpentReading: 1.5,
        textEntry: 'I love how she balances vulnerability with strength in her storytelling. I’m excited to continue.'
      }
    ]
  },
  {
    name: 'The Subtle Art of Not Giving a F*ck',
    bookCategory: 'To Be Read',
    rating: null,
    timeSpent: 0,
    chaptersRead: 0,
    journalEntries: []
  },
  {
    name: 'Thinking, Fast and Slow',
    bookCategory: 'Dropped',
    rating: 3,
    timeSpent: 2.5,
    chaptersRead: 5,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-15',
        startChapter: 1,
        endChapter: 5,
        timeSpentReading: 2.5,
        textEntry: 'While the cognitive biases are interesting, I found the writing a bit dense and repetitive. Not sure I’ll finish.'
      }
    ]
  },
  {
    name: 'The Catcher in the Rye',
    bookCategory: 'Finished',
    rating: 4,
    timeSpent: 3,
    chaptersRead: 9,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-01',
        startChapter: 1,
        endChapter: 9,
        timeSpentReading: 3,
        textEntry: 'Holden’s character is complex and relatable in his search for authenticity. I can see why this book is a classic.'
      }
    ]
  },
  {
    name: 'Educated',
    bookCategory: 'Reading',
    rating: 5,
    timeSpent: 4,
    chaptersRead: 7,
    journalEntries: [
      {
        name: 'Varsha',
        date: '2024-09-18',
        startChapter: 1,
        endChapter: 4,
        timeSpentReading: 2,
        textEntry: 'Tara Westover’s story is unbelievable yet raw and real. Her resilience shines through in every chapter so far.'
      },
      {
        name: 'Varsha',
        date: '2024-09-19',
        startChapter: 5,
        endChapter: 7,
        timeSpentReading: 2,
        textEntry: 'The tension between family loyalty and personal growth is heartbreaking but also empowering. It’s been hard to put this book down.'
      }
    ]
  }
];
   // Reactive statement to calculate and assign the books into their arrays
   $: {
    finishedBooks = allBooks.filter(book => book.bookCategory === "Finished");
    totalFinishedBooks = finishedBooks.length;
  }

 // Reactive statement to update timeSpent for each book based on journal entries
 $: {
    allBooks = allBooks.map(book => {
      const totalTimeSpentReading = book.journalEntries.reduce((sum, entry) => sum + (entry.timeSpentReading || 0), 0);
      return {
        ...book,
        timeSpent: totalTimeSpentReading // Update the timeSpent to be the sum of journal entries
      };
    });
    
    // Now calculate total reading time based on updated allBooks
    totalReadingTime = allBooks.reduce((sum, book) => sum + (book.timeSpent || 0), 0);
  }
  $: {
    readingBooks = allBooks.filter(book => book.bookCategory === "Reading");
  }

  $: {
    toBeReadBooks = allBooks.filter(book => book.bookCategory === "To Be Read");
  }

  $: {
    droppedBooks = allBooks.filter(book => book.bookCategory === "Dropped");
  }
   

</script>


<main>
  <div id="wrapper">
    <div id="header">
      <Navbar />
    </div>
    <!-- <h1>{totalFinishedBooks}</h1> -->
    <!-- <p>Total Reading Time: {totalReadingTime} Hours</p> -->

    <Feature bind:totalFinishedBooks bind:totalReadingTime/>

    <Content bind:allBooks={allBooks} bind:selectedBook bind:ratingBook />
    <!-- <h1>{totalFinishedBooks}</h1> -->

   
  </div>
</main>

<style>
 
</style>
