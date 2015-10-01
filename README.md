# Setup

Python and Javascript libraries packaged together here;
mostly they are translations of each other, if both exist (as with LIWC, for example).

## LIWC

- liwc.py and liwc.js require `/usr/local/data/liwc_2007.trie`
  - MD5: bca2eeec79701ed88c40f8c9c75e5f7c
- liwc-regex.js requires `/usr/local/data/liwc_2007.csv`
  - MD5: 686df57d28941cf797704bd2d4f9a1a3

You can get the LIWC lexicon at [liwc.net](http://liwc.net/).

You can use dic2trie to convert the `.dic` file that the LIWC lexicon comes
with to a trie, for faster (streaming) analysis.

### Python example

    from collections import Counter
    from lexicons.liwc import Liwc
    liwc_lexicon = Liwc()
    gettysburg = '''Four score and seven years ago our fathers brought forth on
      this continent a new nation, conceived in liberty, and dedicated to the
      proposition that all men are created equal. Now we are engaged in a great
      civil war, testing whether that nation, or any nation so conceived and so
      dedicated, can long endure. We are met on a great battlefield of that war.
      We have come to dedicate a portion of that field, as a final resting place
      for those who here gave their lives that that nation might live. It is
      altogether fitting and proper that we should do this.'''
    # read_document() is a generator, but Counter will consume the whole thing
    category_counts = Counter(liwc_lexicon.read_document(gettysburg))
    print 'Basic category counts: {}'.format(category_counts)
    # print out a tabulation that looks like the LIWC software's text report
    full_counts = liwc_lexicon.summarize_document(gettysburg)
    liwc_lexicon.print_summarization(full_counts)


## Arabsenti

- arabsenti.py requires `/usr/local/data/arabsenti_lexicon.txt`
  - MD5: 0ed1192e4b6f10a29b353b3056ec179d

You can get the Arabsenti lexicon from [Muhammad Abdul-Mageed](http://mumageed.blogspot.com/).

Citation form:

    @inproceedings{abdul:2011,
      title={Subjectivity and sentiment analysis of modern standard Arabic},
      author={Abdul-Mageed, M. and Diab, M.T. and Korayem, M.},
      booktitle={Proceedings of the 49th Annual Meeting of the Association for
      Computational Linguistics: Human Language Technologies: short papers-Volume 2},
      pages={587--591},
      year={2011},
      organization={Association for Computational Linguistics}
    }

It requires Python 2.7+.

## AFINN

This is publicly available at [Finn Årup Nielsen's blog](http://fnielsen.posterous.com/afinn-a-new-word-list-for-sentiment-analysis), so I just left it in the code.

# Installation

    python setup.py install
    npm install
    npm link

## Scala

Add the following to your `build.sbt`:

    resolvers ++= Seq("repo.codahale.com" at "http://repo.codahale.com")
    libraryDependencies ++= Seq("com.codahale" % "jerkson_2.9.1" % "0.5.0")

Pull the `scala/lexicons.scala` file into your codebase, and run something like:

    val document = "Pierre Venken is my hero, every day I draw inspiration ..."
    val tokens = document.toLowerCase.replaceAll("\\W", " ").split("\\s+")
    val counts = com.henrian.Liwc(tokens)
    val normalized_counts = counts.mapValues(_ / counts("WC"))
