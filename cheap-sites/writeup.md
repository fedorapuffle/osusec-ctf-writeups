We start at the web page alongside the source code for `server.js `and` admin.js`. We'll focus on `server.js` for now.

We can try to reserve a room on the web page, which prompts a verification math challenge. But no matter how quickly you solve it, it rejects you for being too slow and accuses you of being an AI.
If we examine `server.js`, we can see that reserving a room is handled under the `/reserve` endpoint. In the implementation, we see that the user must solve the math challenge in under 0.125 seconds.
This is naturally unrealistic for a human to do... but we can see that the `time` parameter comes as part of the HTTP GET request to the `/reserve` endpoint, which typically happens when you submit the challenge.

An easy way to circumvent this is to pop open your browser's development tools, go to the network tab, solve a verification challenge, then select the request that was sent from the developer tools network tab and right click
to "Edit and Resend." Just modify the time parameter to be some value under 0.1, and get the flag from the sent request's Response tab.

Our next flag is found under `admin.js`, so we appropriately navigate to the Admin link on the main web page. We're sent to an admin panel with a variety of options. Personally, from here on out, I find it easier to simply work
by looking at `admin.js` for vulnerabilities. But before you do, note that in `server.js`, `/admin` endpoint requests are handled by passing the request body like so: `await admin[action](request['body']);`. This corresponds to
a table of functions in `admin.js`, meaning that you can freely call any exported function using the Edit and Resend tool we used earlier.

Moving on, the function `getAdminPassword(type)` should catch your eye. If it's called requesting the admin password for `email`, then it returns the second flag! So do just that with your HTTP requests. Let action be
"getAdminPassword" and let type be "email", then send.

Now theres also a third flag... we can see "FLAG_3" mentioned/printed out in `sendEmail`, but this function seems impenetrable. The passwords are securely verified, and using `getAdminPassword` will only return
hashes aside from the `email` case. This function is in fact, a red herring. The real money is in `grantAdmin`, which might seem a little innocuous when skimming the source code. In reality, we can see that one of
it's parameters, `environment` can be manipulated to pull from anything in `process.env`, which includes our flags. It even gets returned (to you) as part of the HTTP response if it doesn't match the value "production".
Structure your request to invoke `grantAdmin`. Don't bother with the `user` parameter, the callback will return before it's used. Let `env` be "FLAG_3", so that the next parameter which depends on it expands to `process.env[FLAG_3]`.
Send the request, and get the flag.
