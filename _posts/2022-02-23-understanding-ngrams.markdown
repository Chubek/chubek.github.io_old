---
layout: post
title:  "Learn about N-Grams Interactively!"
date:   2022-02-20 09:12:17 +0330
categories: ml
---
  <script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
    </script>
# Learn N-Grams Interactively!

## The Problem of Encoding
Let's face it, if you go deep into it, a computer is really dumb. A computer's software is highly dependent on its hardware. An 8-bit microprocessor like CMOS 6502 can be used to create a computer like Apple II or it can be used to create a small gamebord whose only means of output is a small LED screen. From mid-70s to late 90s, most programmers coded in the Assembly language of the microprocessor they were coding it. And when you code in Assembly what becomes clear to you is, 

> The only thing that a computer understands are digits!

So, when we feed a text to a computer and tell it 'Classify this with this model!', if you just give it the text, as encoded inUTF-8 as a series of 8-bit octets, it will tell you 'W0t?'

That's true. Most modern text is encoded in UTF-8, Unicode's official codec for characters. UTF-8 uses an octet of 8 bits, which the peasantry call a byte (but that's actually kinda not true because a byte can be 6 or 12 bits, depending on the architecture of your computer. A byte only came to be defined as an octet because IBM PCs used Intel which defines a byte as a little-endian octet) to encode all the possible characters in the Unicode space. And oh, baby, Unicode's LARGE!


So you can't just say, ok, let's do label encoding, let's say the letter `a` is 0 and the emoji 🎉 is 1229 --- why? Because just god knows how many characters are in a large body of text, plus, you will have to account for characters which will be inputted by the end user. Basically you have to accoutn for a gazillion labels!

So what do we do? We, of course, could use one-hot-encoding instead of lable encoding but then we will be dealing with an array the size of the unicode space instead of just an inteeger for each characte, or lik eea good boy, we code take the text into a latent space. 

## But what is this Latent Space?

Do you know what a vector is? Well I bet you do. But let me explain. A vecor is an entity that has a size and a direction in the Carthesian coordinate system. Say par example this:

<svg
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   width="73.735382mm"
   height="69.264969mm"
   viewBox="0 0 73.735382 69.264969"
   version="1.1"
   id="svg1383"
   inkscape:version="0.92.5 (2060ec1f9f, 2020-04-08)"
   sodipodi:docname="vector example.svg">
  <defs
     id="defs1377">
    <marker
       inkscape:stockid="Arrow1Send"
       orient="auto"
       refY="0"
       refX="0"
       id="Arrow1Send"
       style="overflow:visible"
       inkscape:isstock="true">
      <path
         id="path2472"
         d="M 0,0 5,-5 -12.5,0 5,5 Z"
         style="fill:#000000;fill-opacity:1;fill-rule:evenodd;stroke:#000000;stroke-width:1.00000003pt;stroke-opacity:1"
         transform="matrix(-0.2,0,0,-0.2,-1.2,0)"
         inkscape:connector-curvature="0" />
    </marker>
    <marker
       inkscape:stockid="Arrow1Mend"
       orient="auto"
       refY="0"
       refX="0"
       id="Arrow1Mend"
       style="overflow:visible"
       inkscape:isstock="true">
      <path
         id="path2466"
         d="M 0,0 5,-5 -12.5,0 5,5 Z"
         style="fill:#808080;fill-opacity:1;fill-rule:evenodd;stroke:#808080;stroke-width:1.00000003pt;stroke-opacity:1"
         transform="matrix(-0.4,0,0,-0.4,-4,0)"
         inkscape:connector-curvature="0" />
    </marker>
    <clipPath
       id="clip0">
      <rect
         id="rect1928"
         height="289"
         width="481"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip1">
      <rect
         id="rect1931"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip2">
      <rect
         id="rect1934"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip3">
      <rect
         id="rect1937"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip4">
      <rect
         id="rect1940"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip5">
      <rect
         id="rect1943"
         height="209"
         width="424"
         y="141"
         x="591" />
    </clipPath>
    <clipPath
       id="clip6">
      <rect
         id="rect1946"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip7">
      <rect
         id="rect1949"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip8">
      <rect
         id="rect1952"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip9">
      <rect
         id="rect1955"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip10">
      <rect
         id="rect1958"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip11">
      <rect
         id="rect1961"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip12">
      <rect
         id="rect1964"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip13">
      <rect
         id="rect1967"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip14">
      <rect
         id="rect1970"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip15">
      <rect
         id="rect1973"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip16">
      <rect
         id="rect1976"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip17">
      <rect
         id="rect1979"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip18">
      <rect
         id="rect1982"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip19">
      <rect
         id="rect1985"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip20">
      <rect
         id="rect1988"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip21">
      <rect
         id="rect1991"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip0-1">
      <rect
         id="rect2323"
         height="289"
         width="481"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip1-3">
      <rect
         id="rect2326"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip2-9">
      <rect
         id="rect2329"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip3-2">
      <rect
         id="rect2332"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip4-4">
      <rect
         id="rect2335"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
    <clipPath
       id="clip5-0">
      <rect
         id="rect2338"
         height="260"
         width="452"
         y="106"
         x="571" />
    </clipPath>
    <clipPath
       id="clip6-1">
      <rect
         id="rect2341"
         height="288"
         width="480"
         y="92"
         x="557" />
    </clipPath>
  </defs>
  <sodipodi:namedview
     id="base"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageopacity="0.0"
     inkscape:pageshadow="2"
     inkscape:zoom="2"
     inkscape:cx="41.340386"
     inkscape:cy="140.39904"
     inkscape:document-units="mm"
     inkscape:current-layer="layer1"
     showgrid="false"
     fit-margin-top="0"
     fit-margin-left="0"
     fit-margin-right="0"
     fit-margin-bottom="0"
     inkscape:window-width="2560"
     inkscape:window-height="1017"
     inkscape:window-x="1358"
     inkscape:window-y="-8"
     inkscape:window-maximized="1">
    <inkscape:grid
       type="xygrid"
       id="grid2730"
       originx="-24.907397"
       originy="-127.99398" />
  </sodipodi:namedview>
  <metadata
     id="metadata1380">
    <rdf:RDF>
      <cc:Work
         rdf:about="">
        <dc:format>image/svg+xml</dc:format>
        <dc:type
           rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
        <dc:title></dc:title>
      </cc:Work>
    </rdf:RDF>
  </metadata>
  <g
     inkscape:label="Layer 1"
     inkscape:groupmode="layer"
     id="layer1"
     transform="translate(-24.907396,-99.741057)">
    <path
       id="path2370"
       stroke-miterlimit="10"
       d="m 82.285417,109.93958 c 0,0.63912 -0.518013,1.15716 -1.157144,1.15716 -0.639126,0 -1.157142,-0.51804 -1.157142,-1.15716 0,-0.63911 0.518016,-1.15717 1.157142,-1.15717 0.639131,0 1.157144,0.51806 1.157144,1.15717 z"
       inkscape:connector-curvature="0"
       style="fill:#4472c4;stroke:#4472c4;stroke-width:0.3857142;stroke-linejoin:round;stroke-miterlimit:10" />
    <path
       style="fill:#808080;stroke:#808080;stroke-width:0.64873821;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1;marker-end:url(#Arrow1Mend)"
       d="M 25.20231,168.60692 79.376072,111.79055"
       id="path2732"
       inkscape:connector-curvature="0" />
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:6.3499999px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.26458332"
       x="83.037529"
       y="109.3645"
       id="text2748">
	   <tspan
         sodipodi:role="line"
         id="tspan2746"
         x="83.037529"
         y="109.3645"
         style="stroke-width:0.26458332">(2,3)</tspan></text>
    <g
       clip-path="url(#clip1-3)"
       id="g2350"
       transform="matrix(0.15375124,0,0,0.26618221,-62.733434,71.392652)"
       style="stroke:#000000;stroke-width:2.96587205;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1">
      <path
         style="fill:none;fill-rule:evenodd;stroke:#000000;stroke-width:2.96587205;stroke-linejoin:round;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1"
         inkscape:connector-curvature="0"
         d="M 0,0 1.04987e-4,259"
         stroke-miterlimit="10"
         transform="matrix(1,0,0,-1,571.5,365.5)"
         id="path2348" />
    </g>
    <g
       clip-path="url(#clip3-2)"
       id="g2358"
       transform="matrix(0.1539991,0,0,0.26616226,-63.065662,71.394777)"
       style="stroke:#000000;stroke-width:2.47174525;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1">
      <path
         style="fill:none;fill-rule:evenodd;stroke:#000000;stroke-width:2.47174525;stroke-linejoin:round;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1"
         inkscape:connector-curvature="0"
         d="m 571.5,365.5 h 451"
         stroke-miterlimit="10"
         id="path2356" />
    </g>
  </g>
</svg>


What you see drawn above is this vector:

$$ \vec{v} = \begin{bmatrix} 2 \\ 3 \end{bmatrix} $$ 

Ok, buddy, so what?

Now imagine this vector includes attributes instead of coordinates. These attributes can be anything we wish them to be. But what would make these vectors represent a so-called *latent space* is them representing a text encoded in such a way that it's considerably smaller than the actual text. Imagine this piece of Python code

```python
my_text = "This is my text! I love it!"
print(hash(my_text))
```

That would print a series of numbers, a hash, in its own unique way, is a latent space. A non-reversible latent space however. A vector is a reversible latent space. You can give a large text, maybe the text to Count of Monte Cristo or the Prayers of the Great Shield, to this encoder, and it will input a small representation of the text. You can always reverse it back. But you can also do stuff with it! A lot of stuff! Know what I'm saying? 

This latent space is often called an *embedding* --- and, no, my dear Crypto Bro, the latent space is stil too large to be used in a smart contract!



## Embeddings, from TF-IDF, to  Bag of Words, to BERT, to...

One simple embeddings vector is TF-IDF. It's so simple, Scikit-Learn can do it. TF-IDF stands for **Terms Frequency Times Inverse Document Frequency**. Term frequencies are the coutns of each word in a document, and the inverse meas you'll divide each of these word counts by the number of documents they occur in. It's widely used in shallow ML.
A vector that is considered a classical "Bag of Words" is vector of word counts of frequencies. It's the simplest form of latent space.
On the deep side of spectrum we have embeddings that are ACTUAL latent spaces, such as BERT encodings. BERT is a neural network developed by Google that takes your text into a latent space represented by of so many attributes, not just frequeny. BERT has an 'attention' layer so it pays attention to the relevancy of the word, and as a result, the redundancy. Like it knows that in this sentence:

> I scream, you scream, we all scream for ice cream!

There are some redundancies so it factors them in. But how does it do that? How does it know that `you scream` and `I scream` and `we all scream` are all single units made up of smaller units?


# Bag of n-grams

So, now that we know what a latent space is well and good --- let us talk about them grams!


## What is an n-grams?

An n-gram is basically a of 1-ples, 2-ples, 3-ples, or more words that represent a single meaningful unit in computational linguistics. n-grams are computational semantics, not grammar, despite what their name might suggest. 

> Note: n-grams we consider in this piece are of words, but they are often used for characters as well.

n-grams come into play when we are tokenizing a sentence. We can tokenize a sentence on 1-grams, 2-grams, 3-grams or more. But at some point n-grams become so niche that it doesn't make computational sense to do a number further than that.

>Tokenization is basically splitting of a sentence into meaningful units


So, let's say we want to split the famous sentence "I scream, you scream, we all scream for ice cream" into 1-grams. That would simple be:

<svg
   width="100.54167mm"
   height="100.54166mm"
   viewBox="0 0 100.54167 100.54166"
   version="1.1"
   id="svg5"
   inkscape:version="1.1.1 (3bf5ae0, 2021-09-20)"
   sodipodi:docname="dra2wing.svg"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:svg="http://www.w3.org/2000/svg">
  <sodipodi:namedview
     id="namedview7"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageshadow="2"
     inkscape:pageopacity="0.0"
     inkscape:pagecheckerboard="0"
     inkscape:document-units="mm"
     showgrid="true"
     inkscape:zoom="0.75187991"
     inkscape:cx="397.00489"
     inkscape:cy="454.19487"
     inkscape:window-width="1876"
     inkscape:window-height="1016"
     inkscape:window-x="3884"
     inkscape:window-y="27"
     inkscape:window-maximized="1"
     inkscape:current-layer="layer1">
    <inkscape:grid
       type="xygrid"
       id="grid9"
       originx="3.8146971e-06"
       originy="-15.875" />
  </sodipodi:namedview>
  <defs
     id="defs2" />
  <g
     inkscape:label="Layer 1"
     inkscape:groupmode="layer"
     id="layer1"
     transform="translate(3.8146973e-6,-15.875)">
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33"
       width="47.625"
       height="15.875"
       x="-1.4305115e-06"
       y="15.875" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5"
       width="47.625"
       height="15.875"
       x="0"
       y="58.208332" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-8"
       width="47.625"
       height="15.875"
       x="52.916668"
       y="15.875" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5-5"
       width="47.625"
       height="15.875"
       x="52.916668"
       y="58.208332" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-4"
       width="47.625"
       height="15.875"
       x="-1.4210855e-14"
       y="37.041668" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5-1"
       width="47.625"
       height="15.875"
       x="-3.8146973e-06"
       y="79.375" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-8-5"
       width="47.625"
       height="15.875"
       x="52.916668"
       y="37.041668" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5-5-1"
       width="47.625"
       height="15.875"
       x="52.916668"
       y="79.375" />
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="21.449833"
       y="27.157915"
       id="text3805"><tspan
         sodipodi:role="line"
         id="tspan3803"
         style="stroke-width:0.264583"
         x="21.449833"
         y="27.157915">I</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="4.2371821"
       y="48.254025"
       id="text9473"><tspan
         sodipodi:role="line"
         id="tspan9471"
         style="stroke-width:0.264583"
         x="4.2371821"
         y="48.254025">Scream</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="12.616358"
       y="68.179169"
       id="text14963"><tspan
         sodipodi:role="line"
         id="tspan14961"
         style="stroke-width:0.264583"
         x="12.616358"
         y="68.179169">You</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="2.4820023"
       y="90.650864"
       id="text17431"><tspan
         sodipodi:role="line"
         id="tspan17429"
         style="stroke-width:0.264583"
         x="2.4820023"
         y="90.650864">Scream</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="67.30899"
       y="27.216146"
       id="text21175"><tspan
         sodipodi:role="line"
         id="tspan21173"
         style="stroke-width:0.264583"
         x="67.30899"
         y="27.216146">We</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="71.534317"
       y="49.203278"
       id="text23775"><tspan
         sodipodi:role="line"
         id="tspan23773"
         style="stroke-width:0.264583"
         x="71.534317"
         y="49.203278">All</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="56.149197"
       y="67.11203"
       id="text27595"><tspan
         sodipodi:role="line"
         id="tspan27593"
         style="stroke-width:0.264583"
         x="56.149197"
         y="67.11203">Scream</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="66.924461"
       y="90.822754"
       id="text31097"><tspan
         sodipodi:role="line"
         id="tspan31095"
         style="stroke-width:0.264583"
         x="66.924461"
         y="90.822754">For</tspan></text>
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5-1-8"
       width="47.625"
       height="15.875"
       x="0"
       y="100.54166" />
    <rect
       style="fill:#e6e6e6;fill-rule:evenodd;stroke-width:0.264583"
       id="rect33-5-5-1-4"
       width="47.625"
       height="15.875"
       x="52.916672"
       y="100.54166" />
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="14.514332"
       y="111.81753"
       id="text17431-3"><tspan
         sodipodi:role="line"
         id="tspan17429-6"
         style="stroke-width:0.264583"
         x="14.514332"
         y="111.81753">Ice</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="61.432114"
       y="112.57859"
       id="text31097-3"><tspan
         sodipodi:role="line"
         id="tspan31095-4"
         style="stroke-width:0.264583"
         x="61.432114"
         y="112.57859">Cream</tspan></text>
  </g>
</svg>       


Now let's get all the 2-, 3-, 4- and 5-grams for a more complex text, shown below:

```
I love Chicago deep dish pizza!
New York style pizza is also good.
San Francisco pizza, it can be very good
```

After removing the stropwords and punctions and using this code:

```
from nltk.util import ngrams
import re
from nltk.corpus import stopwords
import nltk

nltk.download('stopwords')

sw_set = list(set(stopwords.words('english')))

sent = '''
I love Chicago deep dish pizza!
New York style pizza is also good.
San Francisco pizza, it can be very good
'''

sent = sent.lower()

sent_rem = re.sub(r"(?!\s)\W", "", sent)
print("Sentence without puncutation is: ", sent_rem)

sent_wo_sw = ' '.join([w for w in sent_rem.split(" ") if w.lower() not in sw_set])

print("W/O stopwords: ", sent_wo_sw)

sent_split = set(re.split(r"\s+", sent_wo_sw))


print("2-grams are: ")
print('\n'.join(list([', '.join(ng) for ng in tuple(ngrams(sent_split, 2))])))
print("\n\n")
print("3-grams are: ")
print('\n'.join(list([', '.join(ng) for ng in tuple(ngrams(sent_split, 3))])))
print("\n\n")
print("4-grams are: ")
print('\n'.join(list([', '.join(ng) for ng in tuple(ngrams(sent_split, 4))])))
print("\n\n")
print("5-grams are: ")
print('\n'.join(list([', '.join(ng) for ng in tuple(ngrams(sent_split, 5))])))

```



```
[nltk_data] Downloading package stopwords to /home/chubak/nltk_data...
[nltk_data]   Package stopwords is already up-to-date!
Sentence without puncutation is:  
i love chicago deep dish pizza
new york style pizza is also good
san francisco pizza it can be very good

W/O stopwords:  
i love chicago deep dish pizza
new york style pizza also good
san francisco pizza good

2-grams are: 
, francisco
francisco, san
san, new
new, york
york, pizza
pizza, style
style, dish
dish, also
also, deep
deep, love
love, good
good, chicago
chicago, i



3-grams are: 
, francisco, san
francisco, san, new
san, new, york
new, york, pizza
york, pizza, style
pizza, style, dish
style, dish, also
dish, also, deep
also, deep, love
deep, love, good
love, good, chicago
good, chicago, i



4-grams are: 
, francisco, san, new
francisco, san, new, york
san, new, york, pizza
new, york, pizza, style
york, pizza, style, dish
pizza, style, dish, also
style, dish, also, deep
dish, also, deep, love
also, deep, love, good
deep, love, good, chicago
love, good, chicago, i



5-grams are: 
, francisco, san, new, york
francisco, san, new, york, pizza
san, new, york, pizza, style
new, york, pizza, style, dish
york, pizza, style, dish, also
pizza, style, dish, also, deep
style, dish, also, deep, love
dish, also, deep, love, good
also, deep, love, good, chicago
deep, love, good, chicago, i

```


As we get furthr, this formula starts shaping in our minds:

$$ N_{n-grams} = \begin{pmatrix} \vert S\vert \\ n \end{pmatrix}  $$


Where $$ \vert S \vert$$ is the size of the sentence expresed in words. Basically, number of n-grams is the combination of the number of words and the n itself.

Use the controls below to generate n-grams for any sentence you want. This will give you a much more clear idea of what they are.
<div style="display: flex; gap: 0.5rem; flex-direction: column">
<select id="num-n">
<option value="1">1 </option>
<option selected value="2">2 </option>
<option value="3">3 </option>
<option value="4">4 </option>
<option value="5">5 </option>
<option value="6">6 </option>
<option value="7">7 </option>

</select>

<textarea value="What a time to be alive!" id="sent-input"></textarea>

<button id="button-ngrams">Get n-grams!</button>
<div id="ngram-res"></div>
</div>

<script type="text/javascript">
function nGrams(sentence, n) {
  var words = sentence.split(/\W+/);
  var total = words.length - n;
  var grams = [];
  for(var i = 0; i <= total; i++) {
    var seq = '';
    for (var j = i; j < i + n; j++) {
      seq += ' ' + words[j];
    }
    grams.push(seq);
  }
  return grams;
}

document.getElementById("button-ngrams").addEventListener("click", () => {
	var num = document.getElementById("num-n").value;
	var sent = document.getElementById("sent-input").value;
	
	document.getElementById("ngram-res").innerHTML = nGrams(sent, parseInt(num));
})
</script>



# Uses of n-grams

So what are n-grams good for?

One use we can think of is, filtering the n-grams that are too frequent in the latent space. Take the n-gram `you, scream` in the second example as an instance of what a reapeatative ngram looks like.


The example we have is somehow wrong because when tokenizing based on n-grams all stopwords such `for`, `to`, `on` etc must be removed. Because they are simply too frequent.


But there are other uses of n-grams which we will discuss now.

## Using n-grams in Conjunction with Stats

### Using n-grams to Fill in the Holes

If we, for example, have a bigram (that is to say, a 2-gram) we can approximate the probability of a word given all the previous words only by using the conditional probability of one preceding word, In the bigram of {hello, world} the conditional probablity of *word* can be estimated using the following formula:

$$ P(wotld|hello) \simeq P(w_{world}|w_{1}^{hello}) \simeq P(w_{world}|w_{hello})  $$

This is basically what's called a *Markov assumption* --- since this bigram makes a *Markov chain model*. In fact we can generalize this to trigrams where it would be 
$$ P(n|\left\{n - 2, n - 1\right\}) $$.

<svg
   width="175.38922mm"
   height="74.159775mm"
   viewBox="0 0 175.38922 74.159775"
   version="1.1"
   id="svg5"
   inkscape:version="1.1.1 (3bf5ae0, 2021-09-20)"
   sodipodi:docname="d2.svg"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:svg="http://www.w3.org/2000/svg">
  <sodipodi:namedview
     id="namedview7"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageshadow="2"
     inkscape:pageopacity="0.0"
     inkscape:pagecheckerboard="0"
     inkscape:document-units="mm"
     showgrid="true"
     inkscape:zoom="0.75187991"
     inkscape:cx="127.67996"
     inkscape:cy="506.06486"
     inkscape:window-width="1876"
     inkscape:window-height="1016"
     inkscape:window-x="3884"
     inkscape:window-y="27"
     inkscape:window-maximized="1"
     inkscape:current-layer="layer1">
    <inkscape:grid
       type="xygrid"
       id="grid15853"
       originx="-0.70833703"
       originy="-14.790939" />
  </sodipodi:namedview>
  <defs
     id="defs2" />
  <g
     inkscape:label="Layer 1"
     inkscape:groupmode="layer"
     id="layer1"
     transform="translate(-0.70833701,-14.790939)">
    <ellipse
       style="fill:#b3b3b3;fill-rule:evenodd;stroke:#000000;stroke-width:0.847026;stroke-miterlimit:4;stroke-dasharray:3.38809, 0.847026;stroke-dashoffset:0;stroke-opacity:1"
       id="path61"
       cx="62.854225"
       cy="25.224689"
       rx="15.391146"
       ry="10.397597"
       transform="matrix(0.99998088,0.00618458,-0.01370045,0.99990614,0,0)" />
    <ellipse
       style="fill:#b3b3b3;fill-rule:evenodd;stroke:#000000;stroke-width:0.847026;stroke-miterlimit:4;stroke-dasharray:3.38809, 0.847026;stroke-dashoffset:0;stroke-opacity:1"
       id="path61-5"
       cx="64.020996"
       cy="49.057289"
       rx="15.391146"
       ry="10.397597"
       transform="matrix(0.99998088,0.00618458,-0.01370045,0.99990614,0,0)" />
    <ellipse
       style="fill:#b3b3b3;fill-rule:evenodd;stroke:#000000;stroke-width:0.847026;stroke-miterlimit:4;stroke-dasharray:3.38809, 0.847026;stroke-dashoffset:0;stroke-opacity:1"
       id="path61-5-3"
       cx="64.910789"
       cy="74.740112"
       rx="15.391146"
       ry="10.397597"
       transform="matrix(0.99998088,0.00618458,-0.01370045,0.99990614,0,0)" />
    <rect
       style="fill:#b3b3b3;stroke:#000000;stroke-width:1.69465;stroke-miterlimit:4;stroke-dasharray:6.77861, 1.69465;stroke-dashoffset:0;stroke-opacity:1"
       id="rect3285"
       width="40.777672"
       height="63.5"
       x="1.555662"
       y="21.166666" />
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="9.7437677"
       y="39.996296"
       id="text6281"><tspan
         sodipodi:role="line"
         id="tspan6279"
         style="stroke-width:0.264583"
         x="9.7437677"
         y="39.996296">pizza</tspan><tspan
         sodipodi:role="line"
         style="stroke-width:0.264583"
         x="9.7437677"
         y="53.225422"
         id="tspan6283">style</tspan><tspan
         sodipodi:role="line"
         style="stroke-width:0.264583"
         x="9.7437677"
         y="66.454544"
         id="tspan6285">dish</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:8.09458px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.202365"
       x="53.74157"
       y="26.831877"
       id="text9157"
       transform="scale(0.96889359,1.0321051)"><tspan
         sodipodi:role="line"
         id="tspan9155"
         style="stroke-width:0.202365"
         x="53.74157"
         y="26.831877">pizza</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:7.37636px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.184409"
       x="51.102074"
       y="54.296791"
       id="text12233"
       transform="scale(1.0544853,0.94832996)"><tspan
         sodipodi:role="line"
         id="tspan12231"
         style="stroke-width:0.184409"
         x="51.102074"
         y="54.296791">style</tspan></text>
    <text
       xml:spapodi:role="line"
         id="tspan33374"
         style="stroke-width:0.264583"
         x="105.83333"
         y="42.333332">p(pizza|style)</tspan></text>
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:10.5833px;line-height:1.25;font-family:sans-serif;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583"
       x="85.476982"
       y="86.45475"
       id="text33376-9"><tspan
         sodipodi:role="line"
         id="tspan33374-1"
         style="stroke-width:0.264583"
         x="85.476982"
         y="86.45475">p(pizza|dish)</tspan></text>
  </g>
</svg>

A Markov Chain can be described as a sequence of random 'states' where each new state is conditionally reliant on the previous state. So let's say...

#### A Room Full of Monkeys and Typewriters: Generating Text with n-grams


There's an age old question, can a room full of monkeys with type writers, vicariously and flippantly pressing the keys, collectively come up with a Shakespeare play?

Actually, we can come up with an answer using basic n-grams. Not n-grams of words, but n-grams of characters. 

Let's say we want these subordinate brute beasts to at least come up with one sentence:

```
to_be_or_not_to_be
```

Let's say we wish to calculate the probabiliteos of characters in tihs sentence appearing after the letter O.

$$ \begin{cases} o & \underset{25\%}{\rightarrow} & t \\ o & \underset{25\%}{\rightarrow} &  r  \\ o & \underset{50\%}{\rightarrow} & \_  \end{cases}$$

And we also have these:

$$ \begin{cases} t & \underset{67\%}{\rightarrow} & o \\ t & \underset{33\%}{\rightarrow} &  \_  \end{cases}$$

We can express this at:

```
't' -> ['o', '_', 'o']
'o' -> ['_', 'r', 't', '_']
```

Now we wish to generate a new text with these grams. We have to cold start it off wih a seed, that a is a node letter. Then we keep adding and adding iteratively until we a node that has nothing after it.

Press the button below, select an initial click "Generate".

<div style="display: flex; flex-direction: column; gap: 0.4rem;">
<select id='char-input'>
	<option value='t'>t</option>
	<option value='o'>o</option>
</select>
<button id="gen-mc">Generare</button>
<div id="gen-res">[Result]</div>
</div>

<script type="text/javascript">
var ngrams_ = {
  't': ['o', '_', 'o'],
  'o': ['_', 'r', 't', '_']
};

document.getElementById("gen-mc").addEventListener("click", () => {
var txt = document.getElementById("char-input").value;
var head = [txt];
for (var i = 0; i < 15; i++) {
  var possible = ngrams_[head[i]];
  if (!possible) {
    break;
  }
  var r = Math.floor(Math.random() * (possible.length));
  var next = possible[r];

	txt = next;
  head.push(next);
  document.getElementById("gen-res").innerHTML = head.join('');
  
}


})



</script>


Now we can do this, instead of bigrams of characters, let's o bigrams of phonemes. The basis is the same. Select an initial phoneme and see the code do its magic.

<div style="display: flex; flex-direction: column; gap: 0.4rem;">
<select id='ph-input'>
<option value="to">to</option>
<option value="o ">o </option>
<option value=" b"> b</option>
<option value="be">be</option>
<option value="e ">e </option>
<option value=" o"> o</option>
<option value="or">or</option>
<option value="r ">r </option>
<option value=" n"> n</option>
<option value="no">no</option>
<option value="ot">ot</option>
<option value="t ">t </option>
<option value=" t"> t</option>
<option value="e,">e,</option>
<option value=", ">, </option>
<option value="th">th</option>
<option value="ha">ha</option>
<option value="at">at</option>
<option value=" i"> i</option>
<option value="is">is</option>
<option value="s ">s </option>
<option value="he">he</option>
<option value=" q"> q</option>
<option value="qu">qu</option>
<option value="ue">ue</option>
<option value="es">es</option>
<option value="st">st</option>
<option value="ti">ti</option>
<option value="io">io</option>
<option value="on">on</option>
</select>
<div id="ph-res">[Result]</div>
</div>

<script type="text/javascript">
var ngrams = {
   "to": [ " ", ", " ],
   "o ": [ "b", "b" ],
   " b": [ "e", "e" ],
   "be": [ " ", "," ],
   "e ": [ "o", "q" ],
   " o": [ "r" ],
   "or": [ " " ],
   "r ": [ "n" ],
   " n": [ "o" ],
   "no": [ "t" ],
   "ot": [ " " ],
   "t ": [ "t", "i" ],
   " t": [ "o", "h", "h" ],
   "e,": [ " " ],
   ", ": [ "t" ],
   "th": [ "a", "e" ],
   "ha": [ "t" ],
   "at": [ " " ],
   " i": [ "s" ],
   "is": [ " " ],
   "s ": [ "t" ],
   "he": [ " " ],
   " q": [ "u" ],
   "qu": [ "e" ],
   "ue": [ "s" ],
   "es": [ "t" ],
   "st": [ "i" ],
   "ti": [ "o" ],
   "io": [ "n" ],
   "on": [ "." ]
};
function choice(somelist) {
  var i = Math.floor(Math.random() * (somelist.length));
  return somelist[i];
}


document.getElementById('ph-input').addEventListener('change', () => {
	var current = document.getElementById('ph-input').value;
	var output = current;
	
	for (var i = 0; i < 20; i++) {
  if (ngrams.hasOwnProperty(current)) {
    var possible = ngrams[current];
    var next = choice(possible);
	
    output += next;
	console.log("----")
	console.log(output)

	document.getElementById("ph-res").innerHTML = output;
    current = output.substring(output.length - next.length, output.length);
	console.log(current)
  } else {
    break;
  }
}
})




</script>



### Using n-grams and Stats to tell the Future!

So, as it turns out, if you have data in forms of n-grams, you can associatively predict the next part of the n-gram.

So let's say, if you have all the 2-, 3-, 4- and 5- grams of a user's keypresses between two keys on a keyword, you can predict in succession what key they are going to press next!

And this is exactly what [this little project](https://github.com/elsehow/aaronson-oracle) does.

So it's not just NLP where n-grams are useful in but also porbabilities. In fact we could say successive failures in getting head (!) in a coin toss game is by itself a practie in n-grams.


## Using n-grams to Fix and Highlight Errors

It is estimated that there are over 760 million EFL (English as Foreign Language) and 350 ESL (English as Second Language) speakers around the globe. With this volume of people who were not exposed to English from dat one, and people who associate English grammar and semantics with that of their native tongue, then there is definitely going to need for grammar-correcton software.

But how would these software achieve detecting of wrong grammar?

One way, of course, is n-grams. But n-grams alone cannot be used to detect errors. We need tagging alongside it, and then we need to annotate various related corpora with these tags, train a shallow or a deep classifier with bigrams or trigrams of of words.

Imagine these sentences:

```
1. I accuse you of make a bad decision!
2. I accuse you of making a bad decision!
```

Use the n-gram gnerator above to get the trigrams. If you do so, you'll realize the only difference between these two sentences is one simple n-gram.

We just need to do Maximum Likelihood (basially Bayesian stats) Calculation with the tagged n-grams to catch the error. The likelihood of a trigram containing 'bad' appearing in this sentence is less than a set threshold (most often 0.5) so the label would be 0, and this sentence would be wrong.

### Making Sense of Nonesense Using n-grams

Every teenager who's used MCs t generate text knows this, they most often generate total nonesense. The idea of generating 'sensible' (not sensual!) text using n-grams is, that the last word ($$ x^{n} $$) of the n-gram can be inferred from th other words that appear in it. So in the context of a bigram, let's say, `jimbo soup` the context of `jimbo` can be inferred from `soup`. Remember this from 2 minutes ago?

$$ P(x^{(t + 1)} | x^t, \dots, x^{(1)}) $$

And remember Baye's rule which we incited half a minute ago? (or less than 5 second, depending on whether you're actually reading this article).

$$ P(x^{t + 1} | x^t, \dots, x^{t - n + 2}) = \frac{P(x^{t + 1}, x^t, \dots, x^{t - n + 2}))}{P(x^t, \dots, x^{t - n + 2})} $$

So imagine we have the following strings of words:

```
- deep dish samurai septugenarian carwash next
- The septugenarian Samurai ordered deep dish pizza next to the carwash.
```

Now imagine we have scoured thousands of text, and we know this to be true:

```
P(dish | deep, piza) = 0.8
```

So we apply this to the first text, and nope! But the first text, yay!

And that's how we make sense of a text using n-grams.




#### About the Author

The author of this article is Chubak Bidpaa. When he's not doing [insert inane bullshit] he's doing [insert another inane bullshit]. The Markdown for this article is self-contained and can be downloaded from [here](). The HTML is also self-contained and can be downloaded from [here](https://raw.githubusercontent.com/Chubek/chubek.github.io/master/_posts/2022-02-23-understanding-ngrams.markdown). Chubak Bidpaa can be contacted on Discord at Chubak#7400. He can be reached via email at aurlito@riseup.net. Make sure to tag your email as [Gentooman] or [Gentoomen].

