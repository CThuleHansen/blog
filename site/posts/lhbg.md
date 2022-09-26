---
title: "Learn Haskell by Building a Blog Generator"
author: Casper Thule
date: Sep 26, 2022
tags: [Haskell, Book]
description: My impression of the book Learn Haskell by Building a Blog Generator
---

# Learn Haskell by Building a Blog Generator
I have recently gone through http://lhbg-book.link associated with https://github.com/soupi/learn-haskell-blog-generator. It was an absolute pleasure. I will not go through it in detail, just mention a few things I stumbled upon.

## Applicative introduction and type catch-up
The applicative introduction in https://lhbg-book.link/05-glue/04-optparse.html was a well-guided leap. A small, but VERY important part, was the type listing along the way:

```
pConvertSingle :: Parser Options

pInputFile :: Parser SingleInput
pOutputFile :: Parser SingleOutput

ConvertSingle :: SingleInput -> SingleOutput -> Options

(<$>) :: (a -> b) -> Parser a -> Parser b
  -- Specifically, here `a` is `SingleInput`
  -- and `b` is `SingleOutput -> Options`,

ConvertSingle <$> pInputFile :: Parser (SingleOutput -> Options)

(<*>) :: Parser (a -> b) -> Parser a -> Parser b
  -- Specifically, here `a -> b` is `SingleOutput -> Options`
  -- so `a` is `SingleOutput` and `b` is `Options`

-- So we get:
(ConvertSingle <$> pInputFile) <*> pOutputFile :: Parser Options
```
The reason why I so enjoyed the type catch-up is that functions inside a Parser is less intuitive than functions inside a list. For me, it is significantly more easy to relate to a list of functions, than a parser of `a -> b`, as the parser does not parse functions. Instead, it is a parser that is still lacking an argument to parse what is desired. At this point it is important to "follow the types", which the type catch-up helped a lot in achieving.

## Missing exercises
I was looking forward to "Handling errors and multiple files" as I hoped to get to write the glue that connects Haskell to the outside world.
Unfortunately, it was a lot of reading with very little coding from my part. I had hoped for some exercises (finally) in "Lets code already!", but this was unfortunately not the case. 
Up till this point, I have enjoyed the exercise-approach to learning, but this section did not follow this recipe. 

## Fancy Options Parsing Section
By coincidence I stumbled upon contributing to Cli library after going through this guide and I have visited the section called Fancy Options Parsing many times. It is well-described!

## Build error
Upon creating a new module in https://lhbg-book.link/03-html/05-modules.html HLS presented the following error on `module Html`:
```
Multi Cradle: No prefixes matched
pwd: /home/ctm/source/lhbd
filepath: /home/ctm/source/lhbd/app/Html.hs
prefixes:
("app/Main.hs",Cabal {component = Just "lhbd:exe:lhbd"})
cradle
```
I solved this by adding `other-modules: Html` to the related cabal file.

Many of the guides suggests creating an hie.yaml file, e.g. https://github.com/haskell/haskell-language-server/issues/1215
I did not attempt this, as it seemed strange that it should be necessary, thus I stuck to my solution above.
Thus, I believe the reason is that there is not path from Main.hs to Html.hs. Once a path, e.g. via imports, exists, then this error does not occur.

The book does not discuss creating a cabal file at this point, so following the book without doing manual stuff on the side would probably not present this error.

## Summary
I enjoyed the book a lot. Every once in a while it was necessary for me to write up a mind map to keep track of the different sections and I had a few issues on figuring out which file to place the code for the exercises in. Sometimes this was annoying, and at other times I considered it part of the challenge.

Improvement potential for the book could be a better feel for how the pieces fit together, perhaps by more manual testing.

Thanks for a great book with a good pace that I really enjoyed. Especially the section on fancy options parsing.
