In hash-browns, we begin with a simple login webpage. We can find that the hashed input password is checked locally against a hash stored within the page's source.
The page links to CrackStation, which turns out to be effective in cracking the hash. The password is "cheeseburger" and we can enter this into the form and submit.

This takes us to another page, which contains a bouncing string of epilepsy-inducing rainbow strobing text. Again, if we look at the page's source, we can see that
clicking the bouncing text calls the function print_flag(), which we can also simply call in the browser developer console for convenience. This decodes the bouncing text
into the flag, which can be copied from the inspect element tool. The flag is osu{ch33s3bu2g3r_h45h_8r0wn5--b3773r_7h4n_y0u_7h1nk!}
