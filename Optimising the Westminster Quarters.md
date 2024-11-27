# Optimising the Westminster Quarters

The Westminster Quarters (or Westminster Chimes) is the famous set of four
sequences rung from the Palace of Westminster to mark quarter hours.

Five bells are used: four quarter bells, nominally pitched G♯4, F♯4, E4, and B3;
and Big Ben, nominally E3.



## Pitch

To make them easier to type, we'll relabel these pitches.  From high to low: "e,
d, c, G, C".

In order to analyze harmonies, we'll also pretend that they're pure tones in
equal temperament.  (This isn't true.  The harmonics of bells aren't even close
to integer multiples of a single pitch.  Arguably they don't _have_ a singular
pitch.)

Moreover, we'll talk about harmonic relations of these idealized pitches,
and pretend that their relative intervals are their only distinguishable
feature.  (This also isn't true.  Most people can discriminate pitches to _some_
precision, even without reference.  But we'll pretend.)



## The sequences

Every quarter hour, one of these sequences rings:

- `e d c G _` **(first quarter)**
- `c e d G _ | c d e c _` **(second quarter)**
- `e c d G _ | G d e c _ | e d c G _` **(third quarter)**
- `c e d G _ | c d e c _ | e c d G _ | G d e c _` **(full hour)**

The full-hour sequence is followed by repeated `C`s counting the hour number.

Notice:

- Each sequence is divided into parts, called *changes*, which are like musical
  measures.
- There are five distinct changes.
- Each change has four notes, followed by a rest.  The time signature is 5/4.
- The first and third changes always end on `G`, creating a dominant harmony.
- The second and fourth changes always end on `c`, creating a tonic harmony.



## Optimising

Now, this is great and all, producing beautiful, distinctive phrases with a
limited pitch budget.  This is probably some of the most famous music in the
world.

But what if we didn't want any of that?

### Four-bell sequences

The quarter bells `G`, `c`, `d`, `e` could go in any order!  Why restrict
ourselves to arbitrary limits?  The world is so vast.

With four bells, the number of possible changes is 4! = 24.  What else has 24?

<!--
P ← {𝕩⊏˜(≍↕0){∾˝(0∾˘1+𝕩)⊸⊏˘⍒˘=⌜˜↕𝕨}´-⟜↕≠𝕩} # permutations of 𝕩
nl ← @+10           # newline
Join ← (≠⊣)↓·∾∾⚇1   # ", "Join⟨"a","bc","def"⟩
bells ← "CGcde"

changes ← <˘P 1↓bells
•Out nl Join 15⌽changes

# output:
#   dceG
#   deGc
#   decG
#   eGcd
#   eGdc
#   ecGd
#   ecdG
#   edGc
#   edcG
#   Gcde
#   Gced
#   Gdce
#   Gdec
#   Gecd
#   Gedc
#   cGde
#   cGed
#   cdGe
#   cdeG
#   ceGd
#   cedG
#   dGce
#   dGec
#   dcGe

# to be perfectly honest I don't know how `P` works
# and I don't intend to figure it out
# I copied it from BQNcrate and changed 𝕩 to be a 1-array instead of a length
# all the rest of the code in this document is mine though
-->

| Hour | Morning change | Afternoon change |
| :--: | :------------: | :--------------: |
|  12  |   `d c e G`    |    `G d e c`     |
|   1  |   `d e G c`    |    `G e c d`     |
|   2  |   `d e c G`    |    `G e d c`     |
|   3  |   `e G c d`    |    `c G d e`     |
|   4  |   `e G d c`    |    `c G e d`     |
|   5  |   `e c G d`    |    `c d G e`     |
|   6  |   `e c d G`    |    `c d e G`     |
|   7  |   `e d G c`    |    `c e G d`     |
|   8  |   `e d c G`    |    `c e d G`     |
|   9  |   `G c d e`    |    `d G c e`     |
|  10  |   `G c e d`    |    `d G e c`     |
|  11  |   `G d c e`    |    `d c G e`     |

Note that these sequences are in lexicographical order.  (With respect to the
pitches, not the letters.)  I put `Gdec` at noon 'cause it's my favourite.

> **Side note: Finding the changes.**  The BQN code for producing these changes
> is hidden as comments in the Markdown.  It interrupts the flow of the text,
> and this is important, serious research.

<!--
  I realize that this isn't very future-proof.  BQN code isn't easy to read or
  write.  Someday it'll be nearly impossible to run.

  But listen.

  I think we both know that you probably weren't going to try working with
  the code anyway.

  If you were, consider it an exercise to the reader.  Port it to your newest
  APL or Python or whatever.

  Does Fortran still exist?
-->

Anyway, all you need to do is memorize this simple table, and you've got an
unambiguous way to tell all 24 hours apart!  This is far more efficient than the
original, which takes 28 strikes (!) just to figure out that it's noon.

Now, the final bell is actually redundant.  Because we're always using all four
quarter bells, you can just deduce the missing pitch.  (See below.)  But I'm not
a monster.  If you miss the first bell, you can simply listen to the final three
and deduce the initial pitch.



### Smaller intervals than the hour

But, you might be asking, isn't this actually worse?  We don't have any
sequences for quarter hours.

Foolish reader.

You've been outwitted once again.

We've saved so much time from our optimisation that we can reuse the same table
to divide the hour into 24 segments, perfect 2½-minute intervals, rather than
the meagre 15-minute quarters of old!

| Division |  Change   | Division |  Change   |
| :------- | :-------: | :------- | :-------: |
|  :00     | `G d e c` |  :30     | `d c e G` |
|  :02½    | `G e c d` |  :32½    | `d e G c` |
|  :05     | `G e d c` |  :35     | `d e c G` |
|  :07½    | `c G d e` |  :37½    | `e G c d` |
|  :10     | `c G e d` |  :40     | `e G d c` |
|  :12½    | `c d G e` |  :42½    | `e c G d` |
|  :15     | `c d e G` |  :45     | `e c d G` |
|  :17½    | `c e G d` |  :47½    | `e d G c` |
|  :20     | `c e d G` |  :50     | `e d c G` |
|  :22½    | `d G c e` |  :52½    | `G c d e` |
|  :25     | `d G e c` |  :55     | `G c e d` |
|  :27½    | `d c G e` |  :57½    | `G d c e` |

Now to learn the time, you just have to walk outside, wait about 2 minutes, and
use your handy-dandy memorized table.  Finally, pocket watches are obsolete!



## Optimising again

Let's get Big Ben in on the action.

Let's say we can ring each bell a maximum of once in a change.  Furthermore,
a change should be nonempty (an empty sequence is funny but we're all about
practicality here).

### Pitch sets

Every 5-bell set is distinguishable without a reference pitch.

Every 4-bell set is too.  If `C` is missing, the lowest (`G`) and highest (`e`)
pitches differ by a 6th.  If `e` is missing, the lowest (`C`) and highest (`d`)
pitches differ by a 9th.  If any other pitch is missing, the lowest (`C`) and
highest (`e`) differ by a 10th.  In any case, the lowest pitch is either `C` or
`G`, and this lets us figure out the others.

Easy.

Every 3-bell set is distinguishable too.  This is a little more complicated,
but basically it's because almost every pitch pair forms a distinct interval.
There's one each of 10th, 9th, 8ve, 6th, 4th, and 3rd.  The only duplicate
intervals are the 5ths (`CG` and `Gd`) and the major 2nds (`cd` and `de`).
Any set with either of these intervals has a third note, which introduces two
intervals, at least one of which is not confusable.

As we saw, only a few 2-bell sets are distinguishable:  `Cc`, `Cd`, `Ce`, `Gc`,
`Ge`, and `ce`.  We arbitrarily choose the 5th- and 2nd-interval sets as `CG`
and `cd` (because those are the lower pitches, hence louder, hence better).

And for a 1-bell set we'll choose `C`.  (Remember, we're pretending that bells
can't be distinguished, even though they definitely can.)

### Combinations

- 5-bell sets: (1 set) * (5! combinations) = 120 sequences
- 4-bell sets: (5 sets) * (4! combinations) = 120 sequences
- 3-bell sets: (10 sets) * (3! combinations) = 60 sequences
- 2-bell sets: (8 sets) * (2! combinations) = 16 sequences
- 1-bell set: (1 set) * (1! combination) = 1 sequence

In total we've got 317 unique sequences, perfect for dividing the day into
4-minute 32.56-second chunks, or, as they're more commonly known,
trecentiseptendecidays.

This scheme is more optimal and interesting because there's only one whole-hour
division, which we'll arbitrarily put at noon.  We didn't really want to
schedule anything right on the hour anyway.

<!--
P ← {𝕩⊏˜(≍↕0){∾˝(0∾˘1+𝕩)⊸⊏˘⍒˘=⌜˜↕𝕨}´-⟜↕≠𝕩} # permutations of 𝕩
nl ← @+10           # newline
Join ← (≠⊣)↓·∾∾⚇1   # ", "Join⟨"a","bc","def"⟩
bells ← "CGcde"
sets ← bells⊸⊐¨⟨    # distinguishable sets
  "CGcde"
  "CGcd"⋄"CGce"⋄"CGde"⋄"Ccde"⋄"Gcde"
  "CGc"⋄"CGd"⋄"CGe"⋄"Ccd"⋄"Cce"⋄"Cde"⋄"Gcd"⋄"Gce"⋄"Gde"⋄"cde"
  "CG"⋄"Cc"⋄"Cd"⋄"Ce"⋄"Gc"⋄"Ge"⋄"cd"⋄"ce"
  "C"
⟩

Hm  ← {•Fmt¨⟨h←⌊𝕩÷60,⌊0.5+𝕩-h×60⟩} # hours and minutes
Pad ← {p←¯2↑𝕩 ⋄ '0'¨⌾((p=' ')⊸/)p} # pad to 2 digits with 0s
Print ← {':'Join Pad¨Hm𝕩}          # fractional minutes as "hh:mm"

times ← Print¨60×24×(÷×↕)317
changes ← ⊏⟜bells¨∧∾<˘¨P¨sets

•Out nl Join(<" | "⊸Join)˘⍉ times ≍ (' 'Join<¨)¨changes

# output:
#   00:00 | C
#   00:05 | C G
#   00:09 | C G c
#   00:14 | C G c d
#   00:18 | C G c d e
#   00:23 | C G c e
#   00:27 | C G c e d
#   00:32 | C G d
#   00:36 | C G d c
#   00:41 | C G d c e
#   00:45 | C G d e
#   00:50 | C G d e c
#   00:55 | C G e
#   00:59 | C G e c
#   01:04 | C G e c d
#   01:08 | C G e d
#   01:13 | C G e d c
#   01:17 | C c
#   01:22 | C c G
#   01:26 | C c G d
#   01:31 | C c G d e
#   01:35 | C c G e
#   01:40 | C c G e d
#   01:44 | C c d
#   01:49 | C c d G
#   01:54 | C c d G e
#   01:58 | C c d e
#   02:03 | C c d e G
#   02:07 | C c e
#   02:12 | C c e G
#   02:16 | C c e G d
#   02:21 | C c e d
#   02:25 | C c e d G
#   02:30 | C d
#   02:34 | C d G
#   02:39 | C d G c
#   02:44 | C d G c e
#   02:48 | C d G e
#   02:53 | C d G e c
#   02:57 | C d c
#   03:02 | C d c G
#   03:06 | C d c G e
#   03:11 | C d c e
#   03:15 | C d c e G
#   03:20 | C d e
#   03:24 | C d e G
#   03:29 | C d e G c
#   03:34 | C d e c
#   03:38 | C d e c G
#   03:43 | C e
#   03:47 | C e G
#   03:52 | C e G c
#   03:56 | C e G c d
#   04:01 | C e G d
#   04:05 | C e G d c
#   04:10 | C e c
#   04:14 | C e c G
#   04:19 | C e c G d
#   04:23 | C e c d
#   04:28 | C e c d G
#   04:33 | C e d
#   04:37 | C e d G
#   04:42 | C e d G c
#   04:46 | C e d c
#   04:51 | C e d c G
#   04:55 | G C
#   04:60 | G C c
#   05:04 | G C c d
#   05:09 | G C c d e
#   05:13 | G C c e
#   05:18 | G C c e d
#   05:23 | G C d
#   05:27 | G C d c
#   05:32 | G C d c e
#   05:36 | G C d e
#   05:41 | G C d e c
#   05:45 | G C e
#   05:50 | G C e c
#   05:54 | G C e c d
#   05:59 | G C e d
#   06:03 | G C e d c
#   06:08 | G c
#   06:12 | G c C
#   06:17 | G c C d
#   06:22 | G c C d e
#   06:26 | G c C e
#   06:31 | G c C e d
#   06:35 | G c d
#   06:40 | G c d C
#   06:44 | G c d C e
#   06:49 | G c d e
#   06:53 | G c d e C
#   06:58 | G c e
#   07:02 | G c e C
#   07:07 | G c e C d
#   07:12 | G c e d
#   07:16 | G c e d C
#   07:21 | G d C
#   07:25 | G d C c
#   07:30 | G d C c e
#   07:34 | G d C e
#   07:39 | G d C e c
#   07:43 | G d c
#   07:48 | G d c C
#   07:52 | G d c C e
#   07:57 | G d c e
#   08:02 | G d c e C
#   08:06 | G d e
#   08:11 | G d e C
#   08:15 | G d e C c
#   08:20 | G d e c
#   08:24 | G d e c C
#   08:29 | G e
#   08:33 | G e C
#   08:38 | G e C c
#   08:42 | G e C c d
#   08:47 | G e C d
#   08:51 | G e C d c
#   08:56 | G e c
#   09:01 | G e c C
#   09:05 | G e c C d
#   09:10 | G e c d
#   09:14 | G e c d C
#   09:19 | G e d
#   09:23 | G e d C
#   09:28 | G e d C c
#   09:32 | G e d c
#   09:37 | G e d c C
#   09:41 | c C
#   09:46 | c C G
#   09:51 | c C G d
#   09:55 | c C G d e
#   09:60 | c C G e
#   10:04 | c C G e d
#   10:09 | c C d
#   10:13 | c C d G
#   10:18 | c C d G e
#   10:22 | c C d e
#   10:27 | c C d e G
#   10:31 | c C e
#   10:36 | c C e G
#   10:41 | c C e G d
#   10:45 | c C e d
#   10:50 | c C e d G
#   10:54 | c G
#   10:59 | c G C
#   11:03 | c G C d
#   11:08 | c G C d e
#   11:12 | c G C e
#   11:17 | c G C e d
#   11:21 | c G d
#   11:26 | c G d C
#   11:30 | c G d C e
#   11:35 | c G d e
#   11:40 | c G d e C
#   11:44 | c G e
#   11:49 | c G e C
#   11:53 | c G e C d
#   11:58 | c G e d
#   12:02 | c G e d C
#   12:07 | c d
#   12:11 | c d C
#   12:16 | c d C G
#   12:20 | c d C G e
#   12:25 | c d C e
#   12:30 | c d C e G
#   12:34 | c d G
#   12:39 | c d G C
#   12:43 | c d G C e
#   12:48 | c d G e
#   12:52 | c d G e C
#   12:57 | c d e
#   13:01 | c d e C
#   13:06 | c d e C G
#   13:10 | c d e G
#   13:15 | c d e G C
#   13:19 | c e
#   13:24 | c e C
#   13:29 | c e C G
#   13:33 | c e C G d
#   13:38 | c e C d
#   13:42 | c e C d G
#   13:47 | c e G
#   13:51 | c e G C
#   13:56 | c e G C d
#   14:00 | c e G d
#   14:05 | c e G d C
#   14:09 | c e d
#   14:14 | c e d C
#   14:19 | c e d C G
#   14:23 | c e d G
#   14:28 | c e d G C
#   14:32 | d C
#   14:37 | d C G
#   14:41 | d C G c
#   14:46 | d C G c e
#   14:50 | d C G e
#   14:55 | d C G e c
#   14:59 | d C c
#   15:04 | d C c G
#   15:09 | d C c G e
#   15:13 | d C c e
#   15:18 | d C c e G
#   15:22 | d C e
#   15:27 | d C e G
#   15:31 | d C e G c
#   15:36 | d C e c
#   15:40 | d C e c G
#   15:45 | d G C
#   15:49 | d G C c
#   15:54 | d G C c e
#   15:58 | d G C e
#   16:03 | d G C e c
#   16:08 | d G c
#   16:12 | d G c C
#   16:17 | d G c C e
#   16:21 | d G c e
#   16:26 | d G c e C
#   16:30 | d G e
#   16:35 | d G e C
#   16:39 | d G e C c
#   16:44 | d G e c
#   16:48 | d G e c C
#   16:53 | d c
#   16:58 | d c C
#   17:02 | d c C G
#   17:07 | d c C G e
#   17:11 | d c C e
#   17:16 | d c C e G
#   17:20 | d c G
#   17:25 | d c G C
#   17:29 | d c G C e
#   17:34 | d c G e
#   17:38 | d c G e C
#   17:43 | d c e
#   17:48 | d c e C
#   17:52 | d c e C G
#   17:57 | d c e G
#   18:01 | d c e G C
#   18:06 | d e C
#   18:10 | d e C G
#   18:15 | d e C G c
#   18:19 | d e C c
#   18:24 | d e C c G
#   18:28 | d e G
#   18:33 | d e G C
#   18:37 | d e G C c
#   18:42 | d e G c
#   18:47 | d e G c C
#   18:51 | d e c
#   18:56 | d e c C
#   19:00 | d e c C G
#   19:05 | d e c G
#   19:09 | d e c G C
#   19:14 | e C
#   19:18 | e C G
#   19:23 | e C G c
#   19:27 | e C G c d
#   19:32 | e C G d
#   19:37 | e C G d c
#   19:41 | e C c
#   19:46 | e C c G
#   19:50 | e C c G d
#   19:55 | e C c d
#   19:59 | e C c d G
#   20:04 | e C d
#   20:08 | e C d G
#   20:13 | e C d G c
#   20:17 | e C d c
#   20:22 | e C d c G
#   20:26 | e G
#   20:31 | e G C
#   20:36 | e G C c
#   20:40 | e G C c d
#   20:45 | e G C d
#   20:49 | e G C d c
#   20:54 | e G c
#   20:58 | e G c C
#   21:03 | e G c C d
#   21:07 | e G c d
#   21:12 | e G c d C
#   21:16 | e G d
#   21:21 | e G d C
#   21:26 | e G d C c
#   21:30 | e G d c
#   21:35 | e G d c C
#   21:39 | e c
#   21:44 | e c C
#   21:48 | e c C G
#   21:53 | e c C G d
#   21:57 | e c C d
#   22:02 | e c C d G
#   22:06 | e c G
#   22:11 | e c G C
#   22:16 | e c G C d
#   22:20 | e c G d
#   22:25 | e c G d C
#   22:29 | e c d
#   22:34 | e c d C
#   22:38 | e c d C G
#   22:43 | e c d G
#   22:47 | e c d G C
#   22:52 | e d C
#   22:56 | e d C G
#   23:01 | e d C G c
#   23:05 | e d C c
#   23:10 | e d C c G
#   23:15 | e d G
#   23:19 | e d G C
#   23:24 | e d G C c
#   23:28 | e d G c
#   23:33 | e d G c C
#   23:37 | e d c
#   23:42 | e d c C
#   23:46 | e d c C G
#   23:51 | e d c G
#   23:55 | e d c G C

# 00:00 => noon
# 13:00 => 1:00am
# I now realize that I could do the subtraction / even more formatting in BQN
# but it was easier in a text editor
-->

|   Time   | Change      |   Time   | Change      |
| :------: | :---------- | :------: | :---------- |
| 12:02 am | `c G e d C` |   noon   | `C`         |
| 12:07 am | `c d`       | 12:05 pm | `C G`       |
| 12:11 am | `c d C`     | 12:09 pm | `C G c`     |
| 12:16 am | `c d C G`   | 12:14 pm | `C G c d`   |
| 12:20 am | `c d C G e` | 12:18 pm | `C G c d e` |
| 12:25 am | `c d C e`   | 12:23 pm | `C G c e`   |
| 12:30 am | `c d C e G` | 12:27 pm | `C G c e d` |
| 12:34 am | `c d G`     | 12:32 pm | `C G d`     |
| 12:39 am | `c d G C`   | 12:36 pm | `C G d c`   |
| 12:43 am | `c d G C e` | 12:41 pm | `C G d c e` |
| 12:48 am | `c d G e`   | 12:45 pm | `C G d e`   |
| 12:52 am | `c d G e C` | 12:50 pm | `C G d e c` |
| 12:57 am | `c d e`     | 12:55 pm | `C G e`     |
|  1:01 am | `c d e C`   | 12:59 pm | `C G e c`   |
|  1:06 am | `c d e C G` |  1:04 pm | `C G e c d` |
|  1:10 am | `c d e G`   |  1:08 pm | `C G e d`   |
|  1:15 am | `c d e G C` |  1:13 pm | `C G e d c` |
|  1:19 am | `c e`       |  1:17 pm | `C c`       |
|  1:24 am | `c e C`     |  1:22 pm | `C c G`     |
|  _etc._  |             |  _etc._  |             |

If we wanted, we could throw out 29 sequences, leaving 288 divisions of 5
minutes each.  This scheme would be compatible with the classic version, but
using only 5 strikes instead of up to 28.

But can we go further?



## Optimising again again

Why not strike some bells at the same time?

<!--
P ← {𝕩⊏˜(≍↕0){∾˝(0∾˘1+𝕩)⊸⊏˘⍒˘=⌜˜↕𝕨}´-⟜↕≠𝕩} # permutations of 𝕩
nl ← @+10           # newline
Join ← (≠⊣)↓·∾∾⚇1   # ", "Join⟨"a","bc","def"⟩
bells ← "CGcde"
sets ← bells⊸⊐¨⟨    # distinguishable sets
  "CGcde"
  "CGcd"⋄"CGce"⋄"CGde"⋄"Ccde"⋄"Gcde"
  "CGc"⋄"CGd"⋄"CGe"⋄"Ccd"⋄"Cce"⋄"Cde"⋄"Gcd"⋄"Gce"⋄"Gde"⋄"cde"
  "CG"⋄"Cc"⋄"Cd"⋄"Ce"⋄"Gc"⋄"Ge"⋄"cd"⋄"ce"
  "C"
⟩

Hm  ← {•Fmt¨⟨h←⌊𝕩÷60,⌊0.5+𝕩-h×60⟩} # hours and minutes
Pad ← {p←¯2↑𝕩 ⋄ '0'¨⌾((p=' ')⊸/)p} # pad to 2 digits with 0s
Print ← {':'Join Pad¨Hm𝕩}          # fractional minutes as "hh:mm"

Multi ← {⍷∧⥊(0<≠)¨⊸/¨⊔⟜𝕩¨↕(≠𝕩)¨↕≠𝕩}
# sequences of nonempty subsets of 𝕩 whose disjoint union is 𝕩
# this would be tricky to write in many languages

times ← Print¨60×24×(÷×↕)1071
changes ← (' 'Join⊏⟜bells¨)¨∧∾Multi¨sets

•Out nl Join(<" | "⊸Join)˘⍉ times ≍ 184⌽changes

# output:
#   00:00 | CGcde
#   00:01 | CGce
#   00:03 | CGce d
#   00:04 | CGd
#   00:05 | CGd c
#   00:07 | CGd c e
#   00:08 | CGd ce
#   00:09 | CGd e
#   00:11 | CGd e c
#   00:12 | CGde
#   00:13 | CGde c
#   00:15 | CGe
#   00:16 | CGe c
#   00:17 | CGe c d
#   00:19 | CGe cd
#   00:20 | CGe d
#   00:22 | CGe d c
#   00:23 | Cc
#   00:24 | Cc G
#   00:26 | Cc G d
#   00:27 | Cc G d e
#   00:28 | Cc G de
#   00:30 | Cc G e
#   00:31 | Cc G e d
#   00:32 | Cc Gd
#   00:34 | Cc Gd e
#   00:35 | Cc Gde
#   00:36 | Cc Ge
#   00:38 | Cc Ge d
#   00:39 | Cc d
#   00:40 | Cc d G
#   00:42 | Cc d G e
#   00:43 | Cc d Ge
#   00:44 | Cc d e
#   00:46 | Cc d e G
#   00:47 | Cc de
#   00:48 | Cc de G
#   00:50 | Cc e
#   00:51 | Cc e G
#   00:52 | Cc e G d
#   00:54 | Cc e Gd
#   00:55 | Cc e d
#   00:56 | Cc e d G
#   00:58 | Ccd
#   00:59 | Ccd G
#   01:01 | Ccd G e
#   01:02 | Ccd Ge
#   01:03 | Ccd e
#   01:05 | Ccd e G
#   01:06 | Ccde
#   01:07 | Ccde G
#   01:09 | Cce
#   01:10 | Cce G
#   01:11 | Cce G d
#   01:13 | Cce Gd
#   01:14 | Cce d
#   01:15 | Cce d G
#   01:17 | Cd
#   01:18 | Cd G
#   01:19 | Cd G c
#   01:21 | Cd G c e
#   01:22 | Cd G ce
#   01:23 | Cd G e
#   01:25 | Cd G e c
#   01:26 | Cd Gc
#   01:27 | Cd Gc e
#   01:29 | Cd Gce
#   01:30 | Cd Ge
#   01:31 | Cd Ge c
#   01:33 | Cd c
#   01:34 | Cd c G
#   01:35 | Cd c G e
#   01:37 | Cd c Ge
#   01:38 | Cd c e
#   01:39 | Cd c e G
#   01:41 | Cd ce
#   01:42 | Cd ce G
#   01:44 | Cd e
#   01:45 | Cd e G
#   01:46 | Cd e G c
#   01:48 | Cd e Gc
#   01:49 | Cd e c
#   01:50 | Cd e c G
#   01:52 | Cde
#   01:53 | Cde G
#   01:54 | Cde G c
#   01:56 | Cde Gc
#   01:57 | Cde c
#   01:58 | Cde c G
#   01:60 | Ce
#   02:01 | Ce G
#   02:02 | Ce G c
#   02:04 | Ce G c d
#   02:05 | Ce G cd
#   02:06 | Ce G d
#   02:08 | Ce G d c
#   02:09 | Ce Gc
#   02:10 | Ce Gc d
#   02:12 | Ce Gcd
#   02:13 | Ce Gd
#   02:14 | Ce Gd c
#   02:16 | Ce c
#   02:17 | Ce c G
#   02:18 | Ce c G d
#   02:20 | Ce c Gd
#   02:21 | Ce c d
#   02:23 | Ce c d G
#   02:24 | Ce cd
#   02:25 | Ce cd G
#   02:27 | Ce d
#   02:28 | Ce d G
#   02:29 | Ce d G c
#   02:31 | Ce d Gc
#   02:32 | Ce d c
#   02:33 | Ce d c G
#   02:35 | G C
#   02:36 | G C c
#   02:37 | G C c d
#   02:39 | G C c d e
#   02:40 | G C c de
#   02:41 | G C c e
#   02:43 | G C c e d
#   02:44 | G C cd
#   02:45 | G C cd e
#   02:47 | G C cde
#   02:48 | G C ce
#   02:49 | G C ce d
#   02:51 | G C d
#   02:52 | G C d c
#   02:53 | G C d c e
#   02:55 | G C d ce
#   02:56 | G C d e
#   02:57 | G C d e c
#   02:59 | G C de
#   03:00 | G C de c
#   03:02 | G C e
#   03:03 | G C e c
#   03:04 | G C e c d
#   03:06 | G C e cd
#   03:07 | G C e d
#   03:08 | G C e d c
#   03:10 | G Cc
#   03:11 | G Cc d
#   03:12 | G Cc d e
#   03:14 | G Cc de
#   03:15 | G Cc e
#   03:16 | G Cc e d
#   03:18 | G Ccd
#   03:19 | G Ccd e
#   03:20 | G Ccde
#   03:22 | G Cce
#   03:23 | G Cce d
#   03:24 | G Cd
#   03:26 | G Cd c
#   03:27 | G Cd c e
#   03:28 | G Cd ce
#   03:30 | G Cd e
#   03:31 | G Cd e c
#   03:32 | G Cde
#   03:34 | G Cde c
#   03:35 | G Ce
#   03:36 | G Ce c
#   03:38 | G Ce c d
#   03:39 | G Ce cd
#   03:41 | G Ce d
#   03:42 | G Ce d c
#   03:43 | G c
#   03:45 | G c C
#   03:46 | G c C d
#   03:47 | G c C d e
#   03:49 | G c C de
#   03:50 | G c C e
#   03:51 | G c C e d
#   03:53 | G c Cd
#   03:54 | G c Cd e
#   03:55 | G c Cde
#   03:57 | G c Ce
#   03:58 | G c Ce d
#   03:59 | G c d
#   04:01 | G c d C
#   04:02 | G c d C e
#   04:03 | G c d Ce
#   04:05 | G c d e
#   04:06 | G c d e C
#   04:07 | G c de
#   04:09 | G c de C
#   04:10 | G c e
#   04:11 | G c e C
#   04:13 | G c e C d
#   04:14 | G c e Cd
#   04:15 | G c e d
#   04:17 | G c e d C
#   04:18 | G cd
#   04:19 | G cd C
#   04:21 | G cd C e
#   04:22 | G cd Ce
#   04:24 | G cd e
#   04:25 | G cd e C
#   04:26 | G cde
#   04:28 | G cde C
#   04:29 | G ce
#   04:30 | G ce C
#   04:32 | G ce C d
#   04:33 | G ce Cd
#   04:34 | G ce d
#   04:36 | G ce d C
#   04:37 | G d C
#   04:38 | G d C c
#   04:40 | G d C c e
#   04:41 | G d C ce
#   04:42 | G d C e
#   04:44 | G d C e c
#   04:45 | G d Cc
#   04:46 | G d Cc e
#   04:48 | G d Cce
#   04:49 | G d Ce
#   04:50 | G d Ce c
#   04:52 | G d c
#   04:53 | G d c C
#   04:54 | G d c C e
#   04:56 | G d c Ce
#   04:57 | G d c e
#   04:58 | G d c e C
#   04:60 | G d ce
#   05:01 | G d ce C
#   05:03 | G d e
#   05:04 | G d e C
#   05:05 | G d e C c
#   05:07 | G d e Cc
#   05:08 | G d e c
#   05:09 | G d e c C
#   05:11 | G de
#   05:12 | G de C
#   05:13 | G de C c
#   05:15 | G de Cc
#   05:16 | G de c
#   05:17 | G de c C
#   05:19 | G e
#   05:20 | G e C
#   05:21 | G e C c
#   05:23 | G e C c d
#   05:24 | G e C cd
#   05:25 | G e C d
#   05:27 | G e C d c
#   05:28 | G e Cc
#   05:29 | G e Cc d
#   05:31 | G e Ccd
#   05:32 | G e Cd
#   05:33 | G e Cd c
#   05:35 | G e c
#   05:36 | G e c C
#   05:37 | G e c C d
#   05:39 | G e c Cd
#   05:40 | G e c d
#   05:42 | G e c d C
#   05:43 | G e cd
#   05:44 | G e cd C
#   05:46 | G e d
#   05:47 | G e d C
#   05:48 | G e d C c
#   05:50 | G e d Cc
#   05:51 | G e d c
#   05:52 | G e d c C
#   05:54 | Gc
#   05:55 | Gc C
#   05:56 | Gc C d
#   05:58 | Gc C d e
#   05:59 | Gc C de
#   06:00 | Gc C e
#   06:02 | Gc C e d
#   06:03 | Gc Cd
#   06:04 | Gc Cd e
#   06:06 | Gc Cde
#   06:07 | Gc Ce
#   06:08 | Gc Ce d
#   06:10 | Gc d
#   06:11 | Gc d C
#   06:12 | Gc d C e
#   06:14 | Gc d Ce
#   06:15 | Gc d e
#   06:16 | Gc d e C
#   06:18 | Gc de
#   06:19 | Gc de C
#   06:21 | Gc e
#   06:22 | Gc e C
#   06:23 | Gc e C d
#   06:25 | Gc e Cd
#   06:26 | Gc e d
#   06:27 | Gc e d C
#   06:29 | Gcd
#   06:30 | Gcd C
#   06:31 | Gcd C e
#   06:33 | Gcd Ce
#   06:34 | Gcd e
#   06:35 | Gcd e C
#   06:37 | Gcde
#   06:38 | Gcde C
#   06:39 | Gce
#   06:41 | Gce C
#   06:42 | Gce C d
#   06:43 | Gce Cd
#   06:45 | Gce d
#   06:46 | Gce d C
#   06:47 | Gd C
#   06:49 | Gd C c
#   06:50 | Gd C c e
#   06:51 | Gd C ce
#   06:53 | Gd C e
#   06:54 | Gd C e c
#   06:55 | Gd Cc
#   06:57 | Gd Cc e
#   06:58 | Gd Cce
#   06:59 | Gd Ce
#   07:01 | Gd Ce c
#   07:02 | Gd c
#   07:04 | Gd c C
#   07:05 | Gd c C e
#   07:06 | Gd c Ce
#   07:08 | Gd c e
#   07:09 | Gd c e C
#   07:10 | Gd ce
#   07:12 | Gd ce C
#   07:13 | Gd e
#   07:14 | Gd e C
#   07:16 | Gd e C c
#   07:17 | Gd e Cc
#   07:18 | Gd e c
#   07:20 | Gd e c C
#   07:21 | Gde
#   07:22 | Gde C
#   07:24 | Gde C c
#   07:25 | Gde Cc
#   07:26 | Gde c
#   07:28 | Gde c C
#   07:29 | Ge
#   07:30 | Ge C
#   07:32 | Ge C c
#   07:33 | Ge C c d
#   07:34 | Ge C cd
#   07:36 | Ge C d
#   07:37 | Ge C d c
#   07:38 | Ge Cc
#   07:40 | Ge Cc d
#   07:41 | Ge Ccd
#   07:43 | Ge Cd
#   07:44 | Ge Cd c
#   07:45 | Ge c
#   07:47 | Ge c C
#   07:48 | Ge c C d
#   07:49 | Ge c Cd
#   07:51 | Ge c d
#   07:52 | Ge c d C
#   07:53 | Ge cd
#   07:55 | Ge cd C
#   07:56 | Ge d
#   07:57 | Ge d C
#   07:59 | Ge d C c
#   08:00 | Ge d Cc
#   08:01 | Ge d c
#   08:03 | Ge d c C
#   08:04 | c C
#   08:05 | c C G
#   08:07 | c C G d
#   08:08 | c C G d e
#   08:09 | c C G de
#   08:11 | c C G e
#   08:12 | c C G e d
#   08:13 | c C Gd
#   08:15 | c C Gd e
#   08:16 | c C Gde
#   08:17 | c C Ge
#   08:19 | c C Ge d
#   08:20 | c C d
#   08:22 | c C d G
#   08:23 | c C d G e
#   08:24 | c C d Ge
#   08:26 | c C d e
#   08:27 | c C d e G
#   08:28 | c C de
#   08:30 | c C de G
#   08:31 | c C e
#   08:32 | c C e G
#   08:34 | c C e G d
#   08:35 | c C e Gd
#   08:36 | c C e d
#   08:38 | c C e d G
#   08:39 | c CG
#   08:40 | c CG d
#   08:42 | c CG d e
#   08:43 | c CG de
#   08:44 | c CG e
#   08:46 | c CG e d
#   08:47 | c CGd
#   08:48 | c CGd e
#   08:50 | c CGde
#   08:51 | c CGe
#   08:52 | c CGe d
#   08:54 | c Cd
#   08:55 | c Cd G
#   08:56 | c Cd G e
#   08:58 | c Cd Ge
#   08:59 | c Cd e
#   09:01 | c Cd e G
#   09:02 | c Cde
#   09:03 | c Cde G
#   09:05 | c Ce
#   09:06 | c Ce G
#   09:07 | c Ce G d
#   09:09 | c Ce Gd
#   09:10 | c Ce d
#   09:11 | c Ce d G
#   09:13 | c G
#   09:14 | c G C
#   09:15 | c G C d
#   09:17 | c G C d e
#   09:18 | c G C de
#   09:19 | c G C e
#   09:21 | c G C e d
#   09:22 | c G Cd
#   09:23 | c G Cd e
#   09:25 | c G Cde
#   09:26 | c G Ce
#   09:27 | c G Ce d
#   09:29 | c G d
#   09:30 | c G d C
#   09:31 | c G d C e
#   09:33 | c G d Ce
#   09:34 | c G d e
#   09:35 | c G d e C
#   09:37 | c G de
#   09:38 | c G de C
#   09:39 | c G e
#   09:41 | c G e C
#   09:42 | c G e C d
#   09:44 | c G e Cd
#   09:45 | c G e d
#   09:46 | c G e d C
#   09:48 | c Gd
#   09:49 | c Gd C
#   09:50 | c Gd C e
#   09:52 | c Gd Ce
#   09:53 | c Gd e
#   09:54 | c Gd e C
#   09:56 | c Gde
#   09:57 | c Gde C
#   09:58 | c Ge
#   09:60 | c Ge C
#   10:01 | c Ge C d
#   10:02 | c Ge Cd
#   10:04 | c Ge d
#   10:05 | c Ge d C
#   10:06 | c d
#   10:08 | c d C
#   10:09 | c d C G
#   10:10 | c d C G e
#   10:12 | c d C Ge
#   10:13 | c d C e
#   10:14 | c d C e G
#   10:16 | c d CG
#   10:17 | c d CG e
#   10:18 | c d CGe
#   10:20 | c d Ce
#   10:21 | c d Ce G
#   10:23 | c d G
#   10:24 | c d G C
#   10:25 | c d G C e
#   10:27 | c d G Ce
#   10:28 | c d G e
#   10:29 | c d G e C
#   10:31 | c d Ge
#   10:32 | c d Ge C
#   10:33 | c d e
#   10:35 | c d e C
#   10:36 | c d e C G
#   10:37 | c d e CG
#   10:39 | c d e G
#   10:40 | c d e G C
#   10:41 | c de
#   10:43 | c de C
#   10:44 | c de C G
#   10:45 | c de CG
#   10:47 | c de G
#   10:48 | c de G C
#   10:49 | c e
#   10:51 | c e C
#   10:52 | c e C G
#   10:53 | c e C G d
#   10:55 | c e C Gd
#   10:56 | c e C d
#   10:57 | c e C d G
#   10:59 | c e CG
#   11:00 | c e CG d
#   11:02 | c e CGd
#   11:03 | c e Cd
#   11:04 | c e Cd G
#   11:06 | c e G
#   11:07 | c e G C
#   11:08 | c e G C d
#   11:10 | c e G Cd
#   11:11 | c e G d
#   11:12 | c e G d C
#   11:14 | c e Gd
#   11:15 | c e Gd C
#   11:16 | c e d
#   11:18 | c e d C
#   11:19 | c e d C G
#   11:20 | c e d CG
#   11:22 | c e d G
#   11:23 | c e d G C
#   11:24 | cd
#   11:26 | cd C
#   11:27 | cd C G
#   11:28 | cd C G e
#   11:30 | cd C Ge
#   11:31 | cd C e
#   11:32 | cd C e G
#   11:34 | cd CG
#   11:35 | cd CG e
#   11:36 | cd CGe
#   11:38 | cd Ce
#   11:39 | cd Ce G
#   11:41 | cd G
#   11:42 | cd G C
#   11:43 | cd G C e
#   11:45 | cd G Ce
#   11:46 | cd G e
#   11:47 | cd G e C
#   11:49 | cd Ge
#   11:50 | cd Ge C
#   11:51 | cd e
#   11:53 | cd e C
#   11:54 | cd e C G
#   11:55 | cd e CG
#   11:57 | cd e G
#   11:58 | cd e G C
#   11:59 | cde
#   12:01 | cde C
#   12:02 | cde C G
#   12:03 | cde CG
#   12:05 | cde G
#   12:06 | cde G C
#   12:07 | ce
#   12:09 | ce C
#   12:10 | ce C G
#   12:11 | ce C G d
#   12:13 | ce C Gd
#   12:14 | ce C d
#   12:15 | ce C d G
#   12:17 | ce CG
#   12:18 | ce CG d
#   12:19 | ce CGd
#   12:21 | ce Cd
#   12:22 | ce Cd G
#   12:24 | ce G
#   12:25 | ce G C
#   12:26 | ce G C d
#   12:28 | ce G Cd
#   12:29 | ce G d
#   12:30 | ce G d C
#   12:32 | ce Gd
#   12:33 | ce Gd C
#   12:34 | ce d
#   12:36 | ce d C
#   12:37 | ce d C G
#   12:38 | ce d CG
#   12:40 | ce d G
#   12:41 | ce d G C
#   12:42 | d C
#   12:44 | d C G
#   12:45 | d C G c
#   12:46 | d C G c e
#   12:48 | d C G ce
#   12:49 | d C G e
#   12:50 | d C G e c
#   12:52 | d C Gc
#   12:53 | d C Gc e
#   12:54 | d C Gce
#   12:56 | d C Ge
#   12:57 | d C Ge c
#   12:58 | d C c
#   12:60 | d C c G
#   13:01 | d C c G e
#   13:03 | d C c Ge
#   13:04 | d C c e
#   13:05 | d C c e G
#   13:07 | d C ce
#   13:08 | d C ce G
#   13:09 | d C e
#   13:11 | d C e G
#   13:12 | d C e G c
#   13:13 | d C e Gc
#   13:15 | d C e c
#   13:16 | d C e c G
#   13:17 | d CG
#   13:19 | d CG c
#   13:20 | d CG c e
#   13:21 | d CG ce
#   13:23 | d CG e
#   13:24 | d CG e c
#   13:25 | d CGc
#   13:27 | d CGc e
#   13:28 | d CGce
#   13:29 | d CGe
#   13:31 | d CGe c
#   13:32 | d Cc
#   13:33 | d Cc G
#   13:35 | d Cc G e
#   13:36 | d Cc Ge
#   13:37 | d Cc e
#   13:39 | d Cc e G
#   13:40 | d Cce
#   13:42 | d Cce G
#   13:43 | d Ce
#   13:44 | d Ce G
#   13:46 | d Ce G c
#   13:47 | d Ce Gc
#   13:48 | d Ce c
#   13:50 | d Ce c G
#   13:51 | d G C
#   13:52 | d G C c
#   13:54 | d G C c e
#   13:55 | d G C ce
#   13:56 | d G C e
#   13:58 | d G C e c
#   13:59 | d G Cc
#   14:00 | d G Cc e
#   14:02 | d G Cce
#   14:03 | d G Ce
#   14:04 | d G Ce c
#   14:06 | d G c
#   14:07 | d G c C
#   14:08 | d G c C e
#   14:10 | d G c Ce
#   14:11 | d G c e
#   14:12 | d G c e C
#   14:14 | d G ce
#   14:15 | d G ce C
#   14:16 | d G e
#   14:18 | d G e C
#   14:19 | d G e C c
#   14:21 | d G e Cc
#   14:22 | d G e c
#   14:23 | d G e c C
#   14:25 | d Gc
#   14:26 | d Gc C
#   14:27 | d Gc C e
#   14:29 | d Gc Ce
#   14:30 | d Gc e
#   14:31 | d Gc e C
#   14:33 | d Gce
#   14:34 | d Gce C
#   14:35 | d Ge
#   14:37 | d Ge C
#   14:38 | d Ge C c
#   14:39 | d Ge Cc
#   14:41 | d Ge c
#   14:42 | d Ge c C
#   14:43 | d c
#   14:45 | d c C
#   14:46 | d c C G
#   14:47 | d c C G e
#   14:49 | d c C Ge
#   14:50 | d c C e
#   14:51 | d c C e G
#   14:53 | d c CG
#   14:54 | d c CG e
#   14:55 | d c CGe
#   14:57 | d c Ce
#   14:58 | d c Ce G
#   14:59 | d c G
#   15:01 | d c G C
#   15:02 | d c G C e
#   15:04 | d c G Ce
#   15:05 | d c G e
#   15:06 | d c G e C
#   15:08 | d c Ge
#   15:09 | d c Ge C
#   15:10 | d c e
#   15:12 | d c e C
#   15:13 | d c e C G
#   15:14 | d c e CG
#   15:16 | d c e G
#   15:17 | d c e G C
#   15:18 | d ce
#   15:20 | d ce C
#   15:21 | d ce C G
#   15:22 | d ce CG
#   15:24 | d ce G
#   15:25 | d ce G C
#   15:26 | d e C
#   15:28 | d e C G
#   15:29 | d e C G c
#   15:30 | d e C Gc
#   15:32 | d e C c
#   15:33 | d e C c G
#   15:34 | d e CG
#   15:36 | d e CG c
#   15:37 | d e CGc
#   15:38 | d e Cc
#   15:40 | d e Cc G
#   15:41 | d e G
#   15:43 | d e G C
#   15:44 | d e G C c
#   15:45 | d e G Cc
#   15:47 | d e G c
#   15:48 | d e G c C
#   15:49 | d e Gc
#   15:51 | d e Gc C
#   15:52 | d e c
#   15:53 | d e c C
#   15:55 | d e c C G
#   15:56 | d e c CG
#   15:57 | d e c G
#   15:59 | d e c G C
#   16:00 | de C
#   16:01 | de C G
#   16:03 | de C G c
#   16:04 | de C Gc
#   16:05 | de C c
#   16:07 | de C c G
#   16:08 | de CG
#   16:09 | de CG c
#   16:11 | de CGc
#   16:12 | de Cc
#   16:13 | de Cc G
#   16:15 | de G
#   16:16 | de G C
#   16:17 | de G C c
#   16:19 | de G Cc
#   16:20 | de G c
#   16:22 | de G c C
#   16:23 | de Gc
#   16:24 | de Gc C
#   16:26 | de c
#   16:27 | de c C
#   16:28 | de c C G
#   16:30 | de c CG
#   16:31 | de c G
#   16:32 | de c G C
#   16:34 | e C
#   16:35 | e C G
#   16:36 | e C G c
#   16:38 | e C G c d
#   16:39 | e C G cd
#   16:40 | e C G d
#   16:42 | e C G d c
#   16:43 | e C Gc
#   16:44 | e C Gc d
#   16:46 | e C Gcd
#   16:47 | e C Gd
#   16:48 | e C Gd c
#   16:50 | e C c
#   16:51 | e C c G
#   16:52 | e C c G d
#   16:54 | e C c Gd
#   16:55 | e C c d
#   16:56 | e C c d G
#   16:58 | e C cd
#   16:59 | e C cd G
#   17:01 | e C d
#   17:02 | e C d G
#   17:03 | e C d G c
#   17:05 | e C d Gc
#   17:06 | e C d c
#   17:07 | e C d c G
#   17:09 | e CG
#   17:10 | e CG c
#   17:11 | e CG c d
#   17:13 | e CG cd
#   17:14 | e CG d
#   17:15 | e CG d c
#   17:17 | e CGc
#   17:18 | e CGc d
#   17:19 | e CGcd
#   17:21 | e CGd
#   17:22 | e CGd c
#   17:23 | e Cc
#   17:25 | e Cc G
#   17:26 | e Cc G d
#   17:27 | e Cc Gd
#   17:29 | e Cc d
#   17:30 | e Cc d G
#   17:31 | e Ccd
#   17:33 | e Ccd G
#   17:34 | e Cd
#   17:35 | e Cd G
#   17:37 | e Cd G c
#   17:38 | e Cd Gc
#   17:39 | e Cd c
#   17:41 | e Cd c G
#   17:42 | e G
#   17:44 | e G C
#   17:45 | e G C c
#   17:46 | e G C c d
#   17:48 | e G C cd
#   17:49 | e G C d
#   17:50 | e G C d c
#   17:52 | e G Cc
#   17:53 | e G Cc d
#   17:54 | e G Ccd
#   17:56 | e G Cd
#   17:57 | e G Cd c
#   17:58 | e G c
#   17:60 | e G c C
#   18:01 | e G c C d
#   18:02 | e G c Cd
#   18:04 | e G c d
#   18:05 | e G c d C
#   18:06 | e G cd
#   18:08 | e G cd C
#   18:09 | e G d
#   18:10 | e G d C
#   18:12 | e G d C c
#   18:13 | e G d Cc
#   18:14 | e G d c
#   18:16 | e G d c C
#   18:17 | e Gc
#   18:18 | e Gc C
#   18:20 | e Gc C d
#   18:21 | e Gc Cd
#   18:23 | e Gc d
#   18:24 | e Gc d C
#   18:25 | e Gcd
#   18:27 | e Gcd C
#   18:28 | e Gd
#   18:29 | e Gd C
#   18:31 | e Gd C c
#   18:32 | e Gd Cc
#   18:33 | e Gd c
#   18:35 | e Gd c C
#   18:36 | e c
#   18:37 | e c C
#   18:39 | e c C G
#   18:40 | e c C G d
#   18:41 | e c C Gd
#   18:43 | e c C d
#   18:44 | e c C d G
#   18:45 | e c CG
#   18:47 | e c CG d
#   18:48 | e c CGd
#   18:49 | e c Cd
#   18:51 | e c Cd G
#   18:52 | e c G
#   18:53 | e c G C
#   18:55 | e c G C d
#   18:56 | e c G Cd
#   18:57 | e c G d
#   18:59 | e c G d C
#   19:00 | e c Gd
#   19:02 | e c Gd C
#   19:03 | e c d
#   19:04 | e c d C
#   19:06 | e c d C G
#   19:07 | e c d CG
#   19:08 | e c d G
#   19:10 | e c d G C
#   19:11 | e cd
#   19:12 | e cd C
#   19:14 | e cd C G
#   19:15 | e cd CG
#   19:16 | e cd G
#   19:18 | e cd G C
#   19:19 | e d C
#   19:20 | e d C G
#   19:22 | e d C G c
#   19:23 | e d C Gc
#   19:24 | e d C c
#   19:26 | e d C c G
#   19:27 | e d CG
#   19:28 | e d CG c
#   19:30 | e d CGc
#   19:31 | e d Cc
#   19:32 | e d Cc G
#   19:34 | e d G
#   19:35 | e d G C
#   19:36 | e d G C c
#   19:38 | e d G Cc
#   19:39 | e d G c
#   19:41 | e d G c C
#   19:42 | e d Gc
#   19:43 | e d Gc C
#   19:45 | e d c
#   19:46 | e d c C
#   19:47 | e d c C G
#   19:49 | e d c CG
#   19:50 | e d c G
#   19:51 | e d c G C
#   19:53 | C
#   19:54 | C G
#   19:55 | C G c
#   19:57 | C G c d
#   19:58 | C G c d e
#   19:59 | C G c de
#   20:01 | C G c e
#   20:02 | C G c e d
#   20:03 | C G cd
#   20:05 | C G cd e
#   20:06 | C G cde
#   20:07 | C G ce
#   20:09 | C G ce d
#   20:10 | C G d
#   20:11 | C G d c
#   20:13 | C G d c e
#   20:14 | C G d ce
#   20:15 | C G d e
#   20:17 | C G d e c
#   20:18 | C G de
#   20:19 | C G de c
#   20:21 | C G e
#   20:22 | C G e c
#   20:24 | C G e c d
#   20:25 | C G e cd
#   20:26 | C G e d
#   20:28 | C G e d c
#   20:29 | C Gc
#   20:30 | C Gc d
#   20:32 | C Gc d e
#   20:33 | C Gc de
#   20:34 | C Gc e
#   20:36 | C Gc e d
#   20:37 | C Gcd
#   20:38 | C Gcd e
#   20:40 | C Gcde
#   20:41 | C Gce
#   20:42 | C Gce d
#   20:44 | C Gd
#   20:45 | C Gd c
#   20:46 | C Gd c e
#   20:48 | C Gd ce
#   20:49 | C Gd e
#   20:50 | C Gd e c
#   20:52 | C Gde
#   20:53 | C Gde c
#   20:54 | C Ge
#   20:56 | C Ge c
#   20:57 | C Ge c d
#   20:58 | C Ge cd
#   20:60 | C Ge d
#   21:01 | C Ge d c
#   21:03 | C c
#   21:04 | C c G
#   21:05 | C c G d
#   21:07 | C c G d e
#   21:08 | C c G de
#   21:09 | C c G e
#   21:11 | C c G e d
#   21:12 | C c Gd
#   21:13 | C c Gd e
#   21:15 | C c Gde
#   21:16 | C c Ge
#   21:17 | C c Ge d
#   21:19 | C c d
#   21:20 | C c d G
#   21:21 | C c d G e
#   21:23 | C c d Ge
#   21:24 | C c d e
#   21:25 | C c d e G
#   21:27 | C c de
#   21:28 | C c de G
#   21:29 | C c e
#   21:31 | C c e G
#   21:32 | C c e G d
#   21:33 | C c e Gd
#   21:35 | C c e d
#   21:36 | C c e d G
#   21:37 | C cd
#   21:39 | C cd G
#   21:40 | C cd G e
#   21:42 | C cd Ge
#   21:43 | C cd e
#   21:44 | C cd e G
#   21:46 | C cde
#   21:47 | C cde G
#   21:48 | C ce
#   21:50 | C ce G
#   21:51 | C ce G d
#   21:52 | C ce Gd
#   21:54 | C ce d
#   21:55 | C ce d G
#   21:56 | C d
#   21:58 | C d G
#   21:59 | C d G c
#   22:00 | C d G c e
#   22:02 | C d G ce
#   22:03 | C d G e
#   22:04 | C d G e c
#   22:06 | C d Gc
#   22:07 | C d Gc e
#   22:08 | C d Gce
#   22:10 | C d Ge
#   22:11 | C d Ge c
#   22:12 | C d c
#   22:14 | C d c G
#   22:15 | C d c G e
#   22:16 | C d c Ge
#   22:18 | C d c e
#   22:19 | C d c e G
#   22:21 | C d ce
#   22:22 | C d ce G
#   22:23 | C d e
#   22:25 | C d e G
#   22:26 | C d e G c
#   22:27 | C d e Gc
#   22:29 | C d e c
#   22:30 | C d e c G
#   22:31 | C de
#   22:33 | C de G
#   22:34 | C de G c
#   22:35 | C de Gc
#   22:37 | C de c
#   22:38 | C de c G
#   22:39 | C e
#   22:41 | C e G
#   22:42 | C e G c
#   22:43 | C e G c d
#   22:45 | C e G cd
#   22:46 | C e G d
#   22:47 | C e G d c
#   22:49 | C e Gc
#   22:50 | C e Gc d
#   22:51 | C e Gcd
#   22:53 | C e Gd
#   22:54 | C e Gd c
#   22:55 | C e c
#   22:57 | C e c G
#   22:58 | C e c G d
#   22:59 | C e c Gd
#   23:01 | C e c d
#   23:02 | C e c d G
#   23:04 | C e cd
#   23:05 | C e cd G
#   23:06 | C e d
#   23:08 | C e d G
#   23:09 | C e d G c
#   23:10 | C e d Gc
#   23:12 | C e d c
#   23:13 | C e d c G
#   23:14 | CG
#   23:16 | CG c
#   23:17 | CG c d
#   23:18 | CG c d e
#   23:20 | CG c de
#   23:21 | CG c e
#   23:22 | CG c e d
#   23:24 | CG cd
#   23:25 | CG cd e
#   23:26 | CG cde
#   23:28 | CG ce
#   23:29 | CG ce d
#   23:30 | CG d
#   23:32 | CG d c
#   23:33 | CG d c e
#   23:34 | CG d ce
#   23:36 | CG d e
#   23:37 | CG d e c
#   23:38 | CG de
#   23:40 | CG de c
#   23:41 | CG e
#   23:43 | CG e c
#   23:44 | CG e c d
#   23:45 | CG e cd
#   23:47 | CG e d
#   23:48 | CG e d c
#   23:49 | CGc
#   23:51 | CGc d
#   23:52 | CGc d e
#   23:53 | CGc de
#   23:55 | CGc e
#   23:56 | CGc e d
#   23:57 | CGcd
#   23:59 | CGcd e

# same as before,
# 00:00 => noon
# 13:00 => 1:00am
-->

Gotta be honest, I don't know how to count these mathematically.  But I _do_
know that there are 1071 of them, dividing the day into nice even 1-minute
20.67-second chunks.

|   Time   | Change    |   Time   | Change    |
| :------: | :-------- | :------: | :-------- |
| 11:59 pm | `cde`     | noon     | `CGcde`   |
| 12:01 am | `cde C`   | 12:01 pm | `CGce`    |
| 12:02 am | `cde C G` | 12:03 pm | `CGce d`  |
| 12:03 am | `cde CG`  | 12:04 pm | `CGd`     |
| 12:05 am | `cde G`   | 12:05 pm | `CGd c`   |
| 12:06 am | `cde G C` | 12:07 pm | `CGd c e` |
| 12:07 am | `ce`      | 12:08 pm | `CGd ce`  |
|  _etc._  |           |  _etc._  |           |

As is tradition, we put the loudest (best) change at noon and leave the rest in
lexicographical order.



## We've had our fun

Well, we didn't manage 1440 divisions to signify each minute of the day.  Let
alone the 1561 required for daylight saving time and leap seconds.

(What change would a leap second have?  Send your answers by mail.)

Fortunately, we've built up to a perfect solution:  We have all the components
needed for a perfect clock _and_ calendar, all communicated via beautiful bell
sounds.

#### Year

First we ring the year using our 1071-division scheme, truncated to 1000-year
cycles.  Years ending in "000", like 2000, will of course take the all-important
`CGcde`.  Incidentally, the 2024 and 2025 changes look very pleasant.

| Year | Change    | Year | Change    | Year | Change     | Year | Change     |
| ---- | :-------- | ---- | :-------- | ---- | :--------- | ---- | :--------- |
| 2000 | `CGcde`   | 2010 | `CGde c`  | 2020 | `Cc G d e` | 2030 | `Cc d G`   |
| 2001 | `CGce`    | 2011 | `CGe`     | 2021 | `Cc G de`  | 2031 | `Cc d G e` |
| 2002 | `CGce d`  | 2012 | `CGe c`   | 2022 | `Cc G e`   | 2032 | `Cc d Ge`  |
| 2003 | `CGd`     | 2013 | `CGe c d` | 2023 | `Cc G e d` | 2033 | `Cc d e`   |
| 2004 | `CGd c`   | 2014 | `CGe cd`  | 2024 | `Cc Gd`    | 2034 | `Cc d e G` |
| 2005 | `CGd c e` | 2015 | `CGe d`   | 2025 | `Cc Gd e`  | 2035 | `Cc de`    |
| 2006 | `CGd ce`  | 2016 | `CGe d c` | 2026 | `Cc Gde`   | 2036 | `Cc de G`  |
| 2007 | `CGd e`   | 2017 | `Cc`      | 2027 | `Cc Ge`    | 2037 | `Cc e`     |
| 2008 | `CGd e c` | 2018 | `Cc G`    | 2028 | `Cc Ge d`  | 2038 | `Cc e G`   |
| 2009 | `CGde`    | 2019 | `Cc G d`  | 2029 | `Cc d`     | 2039 | `Cc e G d` |

> **Important note: Human lifespan privilege.**
>
> Most humans won't need to memorize more than 200 of these sequences.  Elves,
> however, will routinely see them repeat.  We've chosen 1000-year cycles
> because we expect that elves will find it possible to keep track of millenia,
> similarly to how humans can usually remember what season it is.
>
> This is an important inclusivity issue which we hope to remedy in the next
> version, perhaps when we raise enough funds to add a sixth bell.

#### Day

Next we ring the day using a dual 24-division scheme, first selecting a
half month, then a day in that half month.

| Half month |  Change   | Half month |  Change   |
| :--------- | :-------: | :--------- | :-------: |
| January    | `G d e c` | July       | `d c e G` |
| January½   | `G e c d` | July½      | `d e G c` |
| February   | `G e d c` | August     | `d e c G` |
| February½  | `c G d e` | August½    | `e G c d` |
| March      | `c G e d` | September  | `e G d c` |
| March½     | `c d G e` | September½ | `e c G d` |
| April      | `c d e G` | October    | `e c d G` |
| April½     | `c e G d` | October½   | `e d G c` |
| May        | `c e d G` | November   | `e d c G` |
| May½       | `d G c e` | November½  | `G c d e` |
| June       | `d G e c` | December   | `G c e d` |
| June½      | `d c G e` | December½  | `G d c e` |

|  Day   |  Change   |  Day   |  Change   |
| :----: | :-------: | :----: | :-------: |
|  1, 17 | `G d e c` | 13, 28 | `d c e G` |
|  2, 18 | `G e c d` | 14, 29 | `d e G c` |
|  3, 19 | `G e d c` | 15, 30 | `d e c G` |
|  4, 20 | `c G d e` | 16, 31 | `e G c d` |
|  5, 21 | `c G e d` |        | `e G d c` |
|  6, 22 | `c d G e` |        | `e c G d` |
|  7, 23 | `c d e G` |        | `e c d G` |
|  8, 24 | `c e G d` |        | `e d G c` |
|  9, 25 | `c e d G` |        | `e d c G` |
| 10, 26 | `d G c e` |        | `G c d e` |
| 11, 27 | `d G e c` |        | `G c e d` |
| 12, 28 | `d c G e` |        | `G d c e` |

This leaves at least 16 changes free for each month, suitable for holidays,
local events, and the like.  If you hear one of these you know you're in for a
good time.

Please note that "January½" is pronounced "January and a half".

#### Time

Finally, we ring the time using our 288-division system to a precision of 5
minutes, followed by a single strike to select the minute.

| Approximate time |  Change     | Minute | Bell |
| :--------------: | :---------- | :----: | :--: |
|     noon         | `C`         |   0    | `C`  |
|     12:05 pm     | `C G`       |   1    | `G`  |
|     12:10 pm     | `C G c`     |   2    | `c`  |
|     12:15 pm     | `C G c d`   |   3    | `d`  |
|     12:20 pm     | `C G c d e` |   4    | `e`  |
|     12:25 pm     | `C G c e`   |
|     _etc._       |             |



## Conclusion

Using only four changes plus a single note, we've successfully encoded minute
precision of each millenium into less time than it takes Westminster to ring a
single hour.  The listener simply needs to memorize a few tables (less than 1500
change meanings!) and they're all set.

In this way we can finally have bells ringing every single minute of every
single day, for a good 15 seconds each time, and as a bonus make all calendars
and clocks obsolete.

This is a good idea with no drawbacks.

— 2024 November 26
