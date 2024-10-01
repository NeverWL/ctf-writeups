# Retrieving Flag #1 from the Breakout Game

First, after entering the breakout game, I opened the sources tab in the web developer console and searched for the text "flag". This showed me the following code snippet:

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

This code shows that the flag is retrieved from the backend using a fetch request, with the syntax for it provided. Checking the backend code, I found the snippet that indicates the score to win: anything equal to or above 30 points.

if (score >= 5 * 6) {
    res.send(process.env.BRICK_FLAG);
}

All that was left was to enter the frontend console and input the following fetch command:

fetch('/breakout?score=30').then(res => res.text()).then(flag => console.log(flag));

This printed the flag onto the console
