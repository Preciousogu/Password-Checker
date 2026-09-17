[README.md](https://github.com/user-attachments/files/32317835/README.md)
# Password Strength & Safety Checker

A self-contained password checker that goes beyond the usual "has a number, has a symbol" checklist. It actually looks for weak patterns, checks real breach data, and flags password reuse across accounts.

No installation, no build step, no dependencies. Just open the file in a browser.

## What it does

### 1. Strength Checker

Analyzes your password as you type and checks:

- Character variety (uppercase, lowercase, numbers, symbols)
- Length, with a higher bar than most checkers (12+ characters to score well, 16+ for the top score)
- Weak patterns that basic checklists usually miss, like:
  - Sequential runs (abc, 123, xyz)
  - Repeated characters (aaa, 111)
  - Keyboard walks (qwerty, asdfgh)
- A list of common, frequently leaked passwords
- Actual entropy, calculated using the standard bits of entropy formula, which measures how unpredictable a password really is

This matters because a password can check every box on a typical checklist and still be weak. Something like Qwerty1! has an uppercase letter, a lowercase letter, a number, and a symbol, so it looks strong at a glance. This tool catches that it's just a keyboard pattern and scores it low anyway.

### 2. Public Breach Check

Checks your password against the Have I Been Pwned database of known breached passwords.

It does this safely using something called k anonymity. Your password gets hashed right there in your browser, and only the first 5 characters of that hash are ever sent anywhere. Your actual password, and even the full hash, never leave your device. This is the same approach used by major browsers and password managers.

### 3. Reuse Checker

Lets you type in the password you use for a few different platforms, like Gmail, your bank, work, whatever, and it'll flag if you're using the same one more than once. To be clear, this only compares what you type into the tool itself. It has no way to see what you've actually typed into any real website.

## Running it

1. Download password_checker.html
2. Double click it, or open it in any browser
3. Start typing

That's it.

## How I built and tested this

I wrote and tested every piece of logic on its own before putting it into the actual page. That included:

- 43 tests covering the character checks, pattern detection, and scoring
- 10 tests covering the SHA-1 hashing and breach response parsing (I checked the hashing against Python's own hashlib separately, so it wasn't just testing itself)
- 12 tests covering the reuse detection

After the page was built, I pulled the logic back out of the HTML and ran the same tests against it again, just to make sure nothing got changed or broken when it was copied over.

One thing I'll be upfront about: I couldn't actually test the live network call to the breach check API myself, since I didn't have internet access to that specific site while building this. The hashing and parsing logic are verified correct, but the real, live API call was tested for the first time when I actually ran it myself in a browser.

## Why I built this

This came out of my background in risk and information security. A lot of password tools give people false confidence by only checking for character variety. A password can look complex and still be genuinely weak if it follows a predictable pattern, and that gap is something I wanted to actually address instead of just gesture at.

## What's next

- A bigger common password list
- A password generator built on the same entropy logic
- Saving reuse checker entries locally in the browser so they don't disappear on refresh, without ever sending that data anywhere
