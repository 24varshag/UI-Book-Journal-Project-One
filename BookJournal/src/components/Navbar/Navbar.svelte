<script>
    import './Navbar.css';

    const userName = "Varsha";
    const startDate = new Date("2024-09-01"); // Replace with the actual start date
    const currentDate = new Date();

    const calculateDaysBetween = (start, end) => {
        const oneDay = 1000 * 60 * 60 * 24;
        return Math.floor((end - start) / oneDay);
    };

    const daysSinceBeginning = calculateDaysBetween(startDate, currentDate);
    const daysActive = daysSinceBeginning > 0 ? Math.floor(Math.random() * daysSinceBeginning) : 0;

    const formatDateTime = (date) => {
        return date.toLocaleString('en-US', {
            month: '2-digit',
            day: '2-digit',
            year: 'numeric',
            hour: '2-digit',
            minute: '2-digit',
            hour12: true
        });
    };

    const currentDateTime = formatDateTime(currentDate);
</script>

<div class="navbar">
    <div>
        <h1 class="navbarTitle">BookJournal</h1>
        <div class="userEntryDetails">
            <p class="noTop">{userName}&emsp;| &emsp;</p>
            <p class="noTop">&emsp;{currentDateTime} &emsp;|&emsp; {daysSinceBeginning} days since beginning, {daysActive} days active</p>
        </div>
    </div>
    
</div>
