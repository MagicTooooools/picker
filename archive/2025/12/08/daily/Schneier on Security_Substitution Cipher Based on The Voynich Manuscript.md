---
title: Substitution Cipher Based on The Voynich Manuscript
url: https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html
source: Schneier on Security
date: 2025-12-08
fetch_date: 2025-12-09T03:21:29.122257
---

# Substitution Cipher Based on The Voynich Manuscript

# [Schneier on Security](https://www.schneier.com/)

Menu

* [Blog](https://www.schneier.com)
* [Newsletter](https://www.schneier.com/crypto-gram/)
* [Books](https://www.schneier.com/books/)
* [Essays](https://www.schneier.com/essays/)
* [News](https://www.schneier.com/news/)
* [Talks](https://www.schneier.com/talks/)
* [Academic](https://www.schneier.com/academic/)
* [About Me](https://www.schneier.com/blog/about/)

### Search

*Powered by [DuckDuckGo](https://duckduckgo.com/)*

Blog

Essays

Whole site

### Subscribe

[![Atom](https://www.schneier.com/wp-content/uploads/2019/10/rss-32px.png)](https://www.schneier.com/feed/atom/)[![Facebook](https://www.schneier.com/wp-content/uploads/2019/10/facebook-32px.png)](https://www.facebook.com/bruce.schneier)[![Twitter](https://www.schneier.com/wp-content/uploads/2019/10/twitter-32px.png)](https://twitter.com/schneierblog)[![Email](https://www.schneier.com/wp-content/uploads/2019/10/email-32px.png)](https://www.schneier.com/crypto-gram)

[Home](https://www.schneier.com)[Blog](https://www.schneier.com/blog/archives/)

## Substitution Cipher Based on The Voynich Manuscript

Here’s a fun paper: “[The Naibbe cipher: a substitution cipher that encrypts Latin and Italian as Voynich Manuscript-like ciphertext](https://www.tandfonline.com/doi/full/10.1080/01611194.2025.2566408)“:

> **Abstract:** In this article, I investigate the hypothesis that the Voynich Manuscript (MS 408, Yale University Beinecke Library) is compatible with being a ciphertext by attempting to develop a historically plausible cipher that can replicate the manuscript’s unusual properties. The resulting cipher­a verbose homophonic substitution cipher I call the Naibbe cipher­can be done entirely by hand with 15th-century materials, and when it encrypts a wide range of Latin and Italian plaintexts, the resulting ciphertexts remain fully decipherable and also reliably reproduce many key statistical properties of the Voynich Manuscript at once. My results suggest that the so-called “ciphertext hypothesis” for the Voynich Manuscript remains viable, while also placing constraints on plausible substitution cipher structures.

Tags: [academic papers](https://www.schneier.com/tag/academic-papers/), [encryption](https://www.schneier.com/tag/encryption/), [history of cryptography](https://www.schneier.com/tag/history-of-cryptography/)

[Posted on December 8, 2025 at 7:04 AM](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html) •
[8 Comments](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html#comments)

### Comments

[Mexaly](https://xkcd.com/722) •
[December 8, 2025 9:07 AM](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html/#comment-450502)

Q1: Who wrote it,

Clive Robinson •
[December 8, 2025 11:01 AM](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html/#comment-450503)

@ Mexaly,

> “Who did they expect to read it?”

That is the Million Dollar question…

Because “if we know that…” It allows us to come up with,

“Probable plain text words”

Because statistics alone won’t break it.

The reason for this is one of those quirks in life, I’ve mentioned before in a similar context.

Think of two basic ciphers,

1, A “One Time Pad”(OTP)

<https://en.wikipedia.org/wiki/One-time_pad>

2, A “straddling checkerboard”

<https://en.wikipedia.org/wiki/Straddling_checkerboard>

Which combined would have made the “VIC Cipher”,

<https://en.wikipedia.org/wiki/VIC_cipher>

Effectively unbreakable, rather than just unbroken for a considerable period as an OTP was not used but a lagged Fibonacci generator was.

As you probably know the OTP had two basic qualities,

1, It’s considered to have “Perfect Secrecy” due to the fact it’s unicity distance is longer than the sent message text.

2, It’s output is over any message length near statistically flat to various tests unlike other stream ciphers or substitution ciphers.

The down side of the second quality is that it makes the use of an OTP much more recognisable when the cipher text is examined.

The down side of this is due to “resource limitations” cryptographers are likely to not make an attempt to break it, if other more “statistically promising ciphertext” is available.

As someone who would rather such resources would waste their time trying to crack an OTP you thus have to change the cipher text statistics to look like another type of stream cipher or substitution cipher.

This is where the “straddling checkerboard comes in. As the Wikipedia article notes,

> *“A straddling checkerboard is a device for converting an alphanumeric plaintext into digits whilst simultaneously achieving fractionation (a simple form of information diffusion) and data compression relative to other schemes using digits.”*

Put simply it replaces eight of the “high frequency letters” given by one or other of the two memorable sentences,

1, “a sin to er”
2, “eat on irish”

With “Single Digit Numbers” and all the other letters and a couple of symbols “Two Digit Numbers” which numbers they get is most often done by using a plaintext sentence from a newspaper, journal or book.

The result is technically called “fractionation”,

<https://en.wikipedia.org/wiki/Transposition_cipher#Fractionation>

Or sometimes “flattening the statistics”. As well as a little compression thus also hiding the plaintext message length.

The thing is it’s used on the plaintext so has to be reversible.

Now consider if you use it on the ciphertext instead, and use the de-straddling on the ciphertext you get out of the OTP encryption.

The result will be the flat statistics of the OTP ciphertext look more like the statistics of another type of stream cipher or substitution cipher.

Thus cryptographers will see it as “breakable” rather than “unbreakable” and will waste time and very valuable resources on it.

Many people have wasted many resources on the “Voynich Manuscript” as they have on other historic ciphertexts, and I suspect they will continue to do so if for nothing more than “bragging rights”.

Which you can see displayed in the extract / quote given at the top of this thread,

> *“My results suggest that the so-called “ciphertext hypothesis” for the Voynich Manuscript remains viable, while also placing constraints on plausible substitution cipher structures.”*

I have demonstrated by cipher types that were known at the time and place of the alleged creation of the “Voynich Manuscript” that just as plausibly it can not be decrypted…

As the old saying has it,

“Pays yer money, takes your choice!”

Me I’d rather read a good book, as at least I will get something from so doing 😉

KC •
[December 8, 2025 11:50 AM](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manuscript.html/#comment-450504)

Michael Greshko gives a fun and graphical overview of his Naibbe cipher [here](https://youtu.be/ByARtG-GUPo?si=M1K7DC7HuspLGanL&t=5555).

From what I understand, the system breaks the plaintext into unigrams and bigrams …

> So **‘HELLO WORLD’** could be **‘H EL L OW OR L D’**

Every individual character then maps to *one of three* positions in an encryption table, as either a (1) unigram (2) bigram prefix *or* (3) bigram suffix.

> ‘H’ would map to the unigram encryption. ‘E’ to the bigram prefix. The following ‘L’ to the bigram suffix.

But we don’t have just one encryption table; we have six. And Michael uses playing cards to randomly select the encryption table.

From the video: ‘A unigram can be represented 6 different ways. A bigram can be represented 36 different ways.’

So in effect a plaintext could produce very different ciphertexts. All with 15th century technology. Really neat!

Clive Robinson •
[December 8, 2025 12:30 PM](https://www.schneier.com/blog/archives/2025/12/substitution-cipher-based-on-the-voynich-manusc...